# GDD Examples

> *The measure of a methodology is whether it helps practitioners think more clearly.*
> *These examples are evidence, not instruction.*

Each example follows the same structure:
- **Domain and stakes** — what the project is and what goes wrong if an assumption is wrong
- **ICR cycle walkthrough** — the formalize → stress → check → surface → gate sequence
- **Unresolvable residue** — what the cycle surfaced that could not be resolved without human judgment
- **What changed** — how the project was different because the cycle ran

---

## Example 1: Legacy Banking Rule Extraction

**Domain:** Credit approval logic in a 40-year-old COBOL core banking system  
**Stakes:** Regulatory compliance, incorrect approvals or denials at scale, audit exposure  
**Context:** Phoenix Runtime pipeline being prepared to modernize the legacy system

---

### The Situation

A bank is preparing to run Project Phoenix on their loan origination system. Approximately 180,000 lines of COBOL. The original authors have retired. The business analysts who know the domain learned it from the code — not from documentation that preceded it.

The instinct is to start Phoenix immediately. Extract the business logic, rebuild from zero. The code is the source of truth.

**GDD says: not yet.**

---

### The ICR Cycle

**FORMALIZE — stated assumptions**

```
[1] We believe a loan is approved when DTI ratio is below 43%.
[2] We believe credit score threshold is 680 for standard approval.
[3] We believe employment is verified by a flag in the APPLICANT table.
[4] We believe the approval path handles self-employed applicants.
[5] We believe joint applications combine incomes for DTI calculation.
[6] We believe the 43% DTI threshold is a fixed regulatory requirement.
[7] We believe declined applications are logged with a reason code.
[8] We believe the COBOL branches identically for all loan types.
```

**STRESS — what breaks each**

```
[1] breaks if: DTI is calculated differently for revolving vs installment debt
              the threshold varies by loan type or loan amount
              DTI rounding is handled inconsistently across branches

[2] breaks if: score thresholds differ by product (mortgage vs personal)
              manual override paths exist that bypass the threshold
              the score used is bureau score vs internal score

[3] breaks if: the flag can be set by the system without a document on file
              self-employed applicants have no flag — they fall through
              flag staleness is not checked (set in 2019, still true?)

[4] breaks if: self-employed applicants hit a code branch that was never
              updated — producing approvals or denials based on 1987 logic

[5] breaks if: the COBOL definition of "joint" differs from the current
              regulatory definition introduced in 2018

[6] breaks if: 43% is a policy threshold the bank set — adjustable by
              executive decision — not a regulatory hard floor

[7] breaks if: reason codes are mapped to a table that has not been
              updated since 2011 and no longer matches current regulations

[8] breaks if: mortgage, personal, and auto loans share the same approval
              path in COBOL but should not — different products, different rules
```

**CHECK — internal consistency**

Three conflicts surface:

```
[CONFLICT A] Statement [6] assumes regulatory fixed floor.
             Statement [1] uses 43% as if it is policy-adjustable.
             These cannot both be true — different governance implications.

[CONFLICT B] Statement [3] assumes the flag covers all applicants.
             Statement [4] assumes self-employed are handled.
             If self-employed have no flag path, [4] cannot be true.

[CONFLICT C] Statement [8] assumes unified approval path.
             Stress test on [2] suggests product-specific thresholds exist.
             If thresholds differ by product, the path is not unified.
```

**SURFACE — unresolvable residue**

```
ITEM 1: Is the 43% DTI threshold regulatory or policy?
  Cannot resolve from: COBOL code (encodes the number, not its origin)
  Decision required from: Chief Compliance Officer
  Consequence if wrong: regulatory breach vs unnecessary constraint
  Deadline: before Phoenix agent pass 1

ITEM 2: What is the self-employed applicant path?
  Cannot resolve from: COBOL code (branch appears to reach a subroutine
                       that references a 1987 product that no longer exists)
  Decision required from: Head of Loan Origination + Compliance
  Options: replicate the existing behaviour, define a new path,
           exclude self-employed from initial migration scope
  Deadline: before Phoenix agent pass 1

ITEM 3: Are reason codes current?
  Cannot resolve from: COBOL code (codes are hardcoded integers)
  Cannot resolve from: current team knowledge (nobody mapped them)
  Decision required from: Operations + Compliance
  Options: map existing codes to current regulations,
           replace with new code set in migrated system
  Deadline: before Phoenix agent pass 2 (validation stage)
```

**GATE**

Three decisions made by human stakeholders before Phoenix runs:

1. CCO confirms: 43% is **regulatory** — it is a hard floor, not adjustable
2. Head of Loan Origination decides: self-employed applicants are **out of scope** for initial migration — separate workstream, separate Phoenix run
3. Operations agrees to map reason codes — assigned owner, two-week timeline

---

### What Changed

Phoenix ran against a constraint set that had been stress-tested. The migration produced Java that correctly treated the 43% threshold as immutable — not as a configurable policy value. The self-employed branch was not migrated and not accidentally replicated with broken logic. Reason codes were mapped before go-live, not discovered missing during a compliance audit.

The GDD cycle took four hours across two working sessions with five people.

The alternative — discovering the self-employed branch ambiguity in production — would have triggered a regulatory notification, a rollback, and a forensic investigation of how many self-employed applicants were incorrectly processed during the migration window.

**The unresolvable residue was the most valuable output Phoenix produced** — and it was produced before Phoenix ran a single line.

---

## Example 2: Multi-Agent Enterprise Procurement Deployment

**Domain:** Purchase order approval workflow across a mid-size manufacturing enterprise  
**Stakes:** Financial exposure from incorrect approvals, audit trail gaps, vendor relationship risk  
**Context:** Five OCTO agents being configured to automate procurement routing

---

### The Situation

A manufacturing company is deploying five agents across their procurement workflow using REACH + OCTO. Each agent has a defined responsibility: vendor verification, budget check, approval routing, PO generation, confirmation dispatch.

The workflow looks clean on a diagram. Each agent has a lane. The instinct is to configure and deploy.

**GDD says: what happens when the diagram lies?**

---

### The ICR Cycle

**FORMALIZE — stated assumptions**

```
[1] We believe an agent may modify a PO only if it holds the current lock.
[2] We believe approval limits are static — set quarterly by finance.
[3] We believe vendor verification is binary — approved or not approved.
[4] We believe the budget check agent sees real-time committed spend.
[5] We believe a PO above $50,000 requires VP approval.
[6] We believe agents operate sequentially — one completes before next starts.
[7] We believe the confirmation agent dispatches only after full approval.
[8] We believe rejected POs are returned to the requestor with a reason.
```

**STRESS — what breaks each**

```
[1] breaks if: two agents request the same PO simultaneously
              a lock expires mid-modification (timeout not defined)
              an agent fails after acquiring the lock but before releasing it

[3] breaks if: vendor is approved for one category but not another
              vendor approval expired — flag not updated in source system
              vendor is on a probationary status (partial approval)

[4] breaks if: committed spend is updated by a batch job that runs at midnight
              two POs are approved in the same minute — both see the same
              pre-commit balance and both pass the budget check

[5] breaks if: a PO is split into two sub-$50k orders to avoid VP approval
              the $50k threshold was last updated in 2019 and may have changed
              an emergency purchase path bypasses the threshold

[6] breaks if: the confirmation agent is triggered by a webhook that fires
              before the approval agent has written its decision
              (this is a race condition — not a hypothetical)

[2] breaks if: finance updates limits mid-quarter during a budget revision
              an agent caches the limit at startup and does not re-read it
```

**CHECK — internal consistency**

```
[CONFLICT A] Statement [6] assumes sequential operation.
             Statement [1] defines a locking model — locking is only
             necessary if concurrent access is possible.
             If truly sequential, locks are unnecessary.
             If locks are necessary, [6] is false.

[CONFLICT B] Statement [4] assumes real-time committed spend.
             Stress test reveals batch update cycle.
             Real-time and batch are mutually exclusive.
             The budget check agent is operating on stale data.

[CONFLICT C] Statement [3] assumes binary vendor status.
             Procurement policy (discovered during session) has three
             states: approved, probationary, suspended.
             Binary assumption will incorrectly route probationary vendors.
```

**SURFACE — unresolvable residue**

```
ITEM 1: Are agents sequential or concurrent?
  Cannot resolve from: OCTO configuration (not yet built)
  Cannot resolve from: current diagram (assumes sequential, implies concurrent)
  Decision required from: Engineering Lead + Procurement Operations
  Consequence: if concurrent, locking model must be designed before deployment
  Deadline: before OCTO arm configuration begins

ITEM 2: What is the budget check data freshness guarantee?
  Cannot resolve from: procurement system documentation (does not specify)
  Decision required from: Finance Systems + Procurement Operations
  Options: accept stale data with defined staleness window,
           require real-time API (may not exist),
           add a pre-commit reservation step before approval
  Deadline: before budget check agent is configured

ITEM 3: How does the system handle probationary vendors?
  Cannot resolve from: current agent design (binary only)
  Decision required from: Procurement Policy Owner
  Options: treat probationary as approved (current silent behaviour),
           treat as suspended (conservative),
           route to human review (correct but requires Synthesis Gate)
  Deadline: before vendor verification agent is configured
```

**GATE**

Three decisions before OCTO is configured:

1. Engineering confirms: agents are **concurrent** — locking model designed and specified before deployment
2. Finance and Procurement agree: budget check uses **reservation pattern** — a hold is placed at check time, released if PO is rejected — real-time problem solved architecturally
3. Procurement Policy Owner decides: probationary vendors route to **human review** — a Synthesis Gate is added to the vendor verification arm

---

### What Changed

The race condition in the confirmation webhook was caught before deployment. The budget double-approval exposure — which would have allowed two simultaneous POs to both pass a budget check that only one of them should have passed — was eliminated by the reservation pattern. Probationary vendors now surface to a procurement manager rather than being silently approved or silently blocked.

None of these were visible on the original workflow diagram.

**The GDD cycle ran as a two-hour workshop with six people. It prevented three categories of production incident before a single agent was configured.**

---

## Example 3: AI-First Feature Development

**Domain:** Loan origination feature in a fintech product  
**Stakes:** Regulatory compliance, incorrect lending decisions, user trust  
**Context:** Development team preparing to hand a feature spec to an AI coding agent

---

### The Situation

A fintech team has a product requirement: build a loan eligibility check. The spec is two paragraphs. The instinct is to paste the spec into Claude Code and iterate until it works.

This is reasonable for a prototype. This is not reasonable for a lending decision that will be applied to real applicants.

**GDD says: what does "correct" mean here — and who decided?**

---

### The ICR Cycle

**FORMALIZE — stated assumptions**

```
[1] We believe a loan is approved when DTI < 43% AND score > 680
    AND employment is verified.
[2] We believe DTI is calculated as monthly debt / monthly gross income.
[3] We believe "employment verified" means an active employment record
    in our database.
[4] We believe the eligibility check is deterministic — same inputs,
    same output, every time.
[5] We believe declined applicants receive a reason.
[6] We believe the check handles joint applications.
[7] We believe 680 is the minimum score for all loan products.
[8] We believe the eligibility check is the only gate before approval.
```

**STRESS — what breaks each**

```
[1] breaks if: an applicant has DTI of 42.9% and score of 679
              — do both thresholds need to be met or is one sufficient?
              the spec uses AND but does not address edge cases at boundary

[2] breaks if: income is variable (freelance, commission, seasonal)
              the applicant has recently started a new job
              gross income is not available — net income is used instead

[3] breaks if: the employment record was last updated 18 months ago
              the applicant is self-employed — no employment record exists
              the applicant is on parental leave — record shows active
              but income is not current

[4] breaks if: credit bureau scores fluctuate between API calls
              the same applicant applies twice in one day with different
              scores returned — different outcomes for identical applicants

[5] breaks if: the decline reason references a regulatory code that
              must meet adverse action notice requirements (ECOA, FCRA)
              — a plain-language reason is not sufficient

[7] breaks if: personal loans and mortgage products have different
              risk profiles — 680 for a personal loan is not the same
              risk as 680 for a mortgage

[8] breaks if: there is a manual review queue that exists in the current
              process that the spec does not mention
              — the check may be a pre-filter, not a final gate
```

**CHECK — internal consistency**

```
[CONFLICT A] Statement [4] assumes determinism.
             Stress test on [4] shows bureau score variance.
             Determinism and live bureau score lookup are incompatible.
             The check cannot be deterministic if input data changes
             between identical submissions.

[CONFLICT B] Statement [3] assumes employment record = verified.
             Stress test on [3] shows self-employed have no record.
             Statement [1] requires employment verified.
             Self-employed applicants cannot pass a check that requires
             a record that does not exist for them.

[CONFLICT C] Statement [8] assumes this is the only gate.
             Stress test surfaces a manual review queue.
             If a manual queue exists, this check is a pre-filter —
             its decline decisions are not final.
             Adverse action notices cannot be triggered by a pre-filter.
```

**SURFACE — unresolvable residue**

```
ITEM 1: What is the definition of "employment verified" for self-employed?
  Cannot resolve from: spec (not mentioned)
  Cannot resolve from: database schema (no self-employed path exists)
  Decision required from: Product Owner + Compliance
  Options: exclude self-employed from this product,
           define an alternative verification path,
           flag for manual review (Synthesis Gate)
  Deadline: before agent writes the employment check logic

ITEM 2: Is this check a final gate or a pre-filter?
  Cannot resolve from: spec (ambiguous)
  Decision required from: Product Owner + Legal
  Consequence: adverse action notice requirements differ significantly
  Deadline: before agent writes the decline response logic

ITEM 3: Are score thresholds product-specific?
  Cannot resolve from: spec (states 680 without qualification)
  Decision required from: Risk Team
  Options: confirm 680 applies to all products,
           define per-product thresholds before build
  Deadline: before agent writes threshold logic

ITEM 4: How is DTI calculated for variable income?
  Cannot resolve from: spec (assumes stable monthly income)
  Decision required from: Product Owner + Risk Team
  Options: use trailing 12-month average,
           require proof of stable income,
           exclude variable income applicants from this product version
  Deadline: before agent writes DTI calculation logic
```

**GATE**

Four decisions before the agent writes a line:

1. Product Owner + Compliance: self-employed applicants are **out of scope for v1** — flagged in the spec, separate workstream
2. Legal confirms: this is a **final gate** — adverse action notices required, reason codes must meet ECOA/FCRA requirements
3. Risk Team confirms: 680 applies to **personal loans only** — mortgage threshold defined separately
4. Risk Team defines: variable income uses **trailing 12-month average** — documented in the constraint set

---

### What Changed

The agent built an eligibility check with:
- Explicit handling of the self-employed exclusion (returns a specific code, not a generic decline)
- ECOA/FCRA compliant reason codes on all decline paths
- A personal-loan-specific threshold flag in configuration — not hardcoded
- A trailing 12-month income calculation for variable income applicants

None of this was in the original two-paragraph spec. All of it was in the Governed Constraint Set the GDD cycle produced.

**The agent did not make these decisions. It executed against decisions that had already been made and governed. That is the difference.**

---

## Contributing an Example

If GDD helped you think more clearly about a project — surface an assumption, catch a conflict, design a better gate — the most valuable thing you can do is document it here.

A good contribution does not need to be comprehensive. It needs to be honest.

**What makes a good example:**
- A real domain with real stakes (anonymized as needed)
- At least one `[CONFLICT]` that surprised you
- A non-trivial unresolvable residue item
- An honest account of what changed because the cycle ran

**What makes a poor example:**
- A domain where all assumptions turned out to be correct
- An example where GDD added process but not insight
- A theoretical scenario that was never actually run

Submit a PR with your example following the structure above. If GDD did not work for your situation — that is also worth documenting. Open a discussion.

---

*These examples are a starting point, not a ceiling.*
*The most valuable examples will come from practitioners who tried the cycle and were surprised by what it surfaced.*
*That is the point.*

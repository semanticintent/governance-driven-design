# Getting Started with GDD

> *Governance-Driven Design is a thinking discipline before it is a process.*
> *This document gives you three entry points depending on where you are.*

---

## Before You Begin: The Five Questions

GDD is not for every project. The real filter is a single question beneath all five:

**Would a wrong assumption, discovered after build, cost more than a GDD cycle?**

If no — prototype freely. Come back to GDD when the stakes rise.

If yes — run through the five questions below to understand *where* the risk lives. If you answer **yes to three or more**, the risk is distributed enough that a cycle belongs before your next project phase.

```
1. Are there business rules in this domain that exist in someone's
   head but not in a document?

2. Would a wrong assumption, discovered after build, cost more
   than a week to fix?

3. Are multiple agents or systems going to touch shared state?

4. Is there a regulatory or compliance dimension where
   "we didn't know" is not an acceptable answer?

5. Has this domain been through a previous modernization
   that didn't fully succeed?
```

If the answer is yes to fewer than three — prototype freely. Come back to GDD when the stakes rise.

---

## Entry Point 1: The Thinker

*You want to understand GDD before committing to anything.*

### What GDD Produces

A GDD cycle produces one artifact: a **Governed Constraint Set**.

It is not a requirements document. It is not a test plan. It is not a specification.

It is a structured answer to the question: *what do we know, what do we assume, and what do we genuinely not know — before anything is built?*

The most valuable part of a Governed Constraint Set is not what it resolves. It is what it surfaces as **unresolvable residue** — the decisions that genuinely require human judgment before any agent runs, any code is written, any pipeline is started.

An organization that proceeds with full awareness of its unresolved constraints is in a fundamentally different position than one that proceeds without knowing they exist.

### What GDD Is Not

- It is not a process that slows everything down
- It is not a compliance checkbox
- It is not a replacement for TDD, BDD, or DDD — it is their upstream completion
- It is not required on every project — it is available when the stakes warrant it

### The Right Relationship to GDD

Use it the way experienced practitioners used TDD: selectively, deliberately, and with a clear sense of what risk you are protecting against. The discipline is most powerful when chosen — not mandated.

---

## Entry Point 2: The Practitioner

*You want to run an ICR cycle. Here is what that looks like in practice.*

### The Starter Ritual

Before any agent runs, any code is written, any sprint is planned:

---

**Step 1 — Open a blank document**

Title it:
```
GDD — [Project Name] — [Date]
Domain: [the business area you are formalizing]
Stakes: [what goes wrong if an assumption is wrong]
```

---

**Step 2 — Write every assumption as a statement**

For every belief your team holds about this domain, write it as:

```
"We believe that X is true."
```

Do not filter. Do not qualify. Write the assumptions your team is
making whether or not anyone has said them out loud.

Examples:
```
We believe that a loan is approved when DTI is below 43%.
We believe that employment verification means a database flag.
We believe that the COBOL approval path handles self-employed applicants.
We believe that two agents can safely write to the same order record.
```

---

**Step 3 — Stress each statement**

For each belief, write the question that would break it:

```
"This breaks if..."
```

Examples:
```
We believe DTI below 43% triggers approval.
→ This breaks if the threshold is regulatory (fixed) not policy (adjustable).
→ This breaks if DTI is calculated differently for joint applications.

We believe employment verification means a database flag.
→ This breaks if the flag can be set without a document on file.
→ This breaks if self-employed applicants have no flag path at all.
```

---

**Step 4 — Mark each statement**

```
[RESOLVED]   we have evidence this holds — source documented
[ASSUMED]    we believe it but have not verified
[UNKNOWN]    we genuinely do not know
[CONFLICT]   this contradicts another statement in this document
```

---

**Step 5 — Identify your unresolvable residue**

Everything marked `[UNKNOWN]` or `[CONFLICT]` is your unresolvable residue.

Collect it into a named section:

```
## Unresolvable Residue

These items cannot be resolved from available information.
Each requires a human decision before execution proceeds.

ITEM 1: [statement]
  Cannot resolve because: [reason]
  Decision required from: [role / person]
  Options: [if known]
  Deadline: [before which phase]

ITEM 2: ...
```

---

**Step 6 — Gate**

Do not proceed until every `[UNKNOWN]` and `[CONFLICT]` is either:

- **Resolved** — absorbed into the constraint set with a documented decision and owner
- **Acknowledged** — explicitly accepted as a known risk with a named owner and a stated consequence if it surfaces later

The gate is the governance act. Proceeding without it is the risk.

---

**Step 7 — Name your Governed Constraint Set**

The completed document — statements, stress tests, classifications,
residue, and gate decisions — is your Governed Constraint Set.

Version it. Store it. Reference it in every downstream artifact.

It is the most valuable thing this project produces before anyone
writes a line of code.

---

### The ICR Cycle in Summary

```
╔══════════════════════════════════════════════════════╗
║  1. FORMALIZE   write every assumption explicitly    ║
║  2. STRESS      find what breaks each one            ║
║  3. CHECK       verify internal consistency          ║
║  4. SURFACE     collect what cannot be resolved      ║
║  5. GATE        human decision on the residue        ║
║  6. CONVERGE    exit with explicit acknowledgment    ║
╚══════════════════════════════════════════════════════╝
```

Exit condition: not "all resolved" — but "all residue explicitly
acknowledged with an owner."

---

## Entry Point 3: The Ecosystem User

*You are already using Semantic Intent tools. Here is how GDD connects.*

### If you are using Phoenix Runtime

Phoenix is a GDD implementation. The human gate before the pipeline
runs is the Synthesis Gate. The `.sil` artifact is the constraint carrier.

**Where GDD adds value:**
- Run an ICR cycle before the first Phoenix agent pass
- The unresolvable residue becomes your human gate checklist
- Business rules that cannot be extracted from COBOL surface here —
  before the migration, not during it
- Resolved constraints become the ground truth your migrated code
  is validated against

```
GDD cycle → Governed Constraint Set
         → Phoenix human gate checklist
         → .sil artifacts carry constraints forward
         → migrated code validated against the constraint set
```

### If you are using Strata Runtime

Strata excavates database schemas. The database can tell you structure.
It cannot tell you intent.

**Where GDD adds value:**
- GDD before the Schema Signal agent runs
- Surface what the schema cannot reveal: why a column exists,
  what a nullable field means, which constraints are enforced
  in application code rather than the database
- Gate on those gaps before the Classifier runs

```
GDD cycle → Governed Constraint Set
         → Strata human review gate
         → classification grounded in explicit constraints
         → schema archaeology with intent, not just structure
```

### If you are using REACH + OCTO

OCTO orchestrates multiple arms reaching into live systems.
Each arm operates within boundaries. GDD defines those boundaries
before OCTO is configured.

**Where GDD adds value:**
- Define the constraint boundaries each arm operates within
  before the orchestration is designed
- Surface conflicts between arms — cases where two arms might
  reach the same resource with different assumptions
- The Synthesis Gate is the designed-in ICR checkpoint

```
GDD cycle → constraint boundaries per arm
         → conflict surface identified before orchestration
         → Synthesis Gate positioned deliberately
         → OCTO operates within governed scope
```

### If you are using RECALL

RECALL is the language for authoring machine-legible, intent-dense
artifacts. GDD is the thinking that precedes the `.rcl` file.

**Where GDD adds value:**
- The RECALL source is the formalized output of a GDD cycle
- Constraint sets authored in RECALL are both human-readable
  and AI-composable
- The discipline of GDD and the legibility of RECALL compound —
  governed intent that travels

```
GDD cycle → Governed Constraint Set (thinking)
         → RECALL .rcl source (formalization)
         → compiled artifact (execution)
```

---

## Entry Point 4: Initializing GDD on a Project

*You want to add GDD to a real project right now.*

### The `.gdd/` Folder

GDD lives in a `.gdd/` directory at the root of your project.

```
your-project/
  .gdd/
    CONSTRAINTS.md     ← the governed constraint set (primary artifact)
    RESIDUE.md         ← open items requiring human decision
    GATES.md           ← record of human gate decisions
    CYCLES/            ← ICR cycle history (append-only)
      2026-06-26-init.md
    SPEC/              ← optional: domain specs feeding the cycle
  src/
  ...
  CLAUDE.md            ← instructs AI agents to respect the GDD layer
```

The dot-prefix signals to both humans and AI models:
*this is the governance layer, not the source.*

---

### Step 1: Create the Structure

```bash
mkdir -p .gdd/CYCLES .gdd/SPEC

touch .gdd/CONSTRAINTS.md
touch .gdd/RESIDUE.md
touch .gdd/GATES.md
```

Or say to Claude Code:
```
Initialize a GDD layer for this project.
Create the .gdd/ folder structure and scaffold
CONSTRAINTS.md, RESIDUE.md, and GATES.md.
```

---

### Step 2: Scaffold CONSTRAINTS.md

Every constraint follows this format:

```markdown
## CONSTRAINT-001
Status: [RESOLVED / ASSUMED / UNKNOWN / CONFLICT]
Statement: [what must be true]
Stress: [what would break it]
Resolution: [decision made, who made it, when]  ← RESOLVED only
Owner: [person or role responsible]
Date: [YYYY-MM-DD]
Downstream: [src/path/to/file.ts]  ← fill in after implementation
```

Start with your first ICR cycle output.
Use the Starter Ritual above to generate the first pass.

---

### Step 3: Add CLAUDE.md

> **The CLAUDE.md is step zero — separable from the full ICR cycle.** Even if your constraint set is empty, placing a CLAUDE.md in the project root makes Claude Code governance-aware: it will read `.gdd/` before acting, surface unknowns before generating, and stop at the Synthesis Gate. Step zero makes the agent a governed participant. Step one (the ICR cycle) gives it constraints to govern against. Both together is GDD in full. Either alone is still progress.

Copy the `CLAUDE.md` template from the GDD repo into your project root:

```
https://github.com/semanticintent/governance-driven-design/blob/main/templates/CLAUDE.md
```

This instructs Claude Code — and any other AI agent — to:
- Read `.gdd/CONSTRAINTS.md` before every implementation pass
- Surface `[UNKNOWN]` and `[CONFLICT]` items before generating
- Trace implementation output back to constraints
- Stop at the Synthesis Gate when ambiguity cannot be resolved

**The `CLAUDE.md` is what makes GDD an AI-first workflow** —
not a document humans maintain separately from the machine,
but a governance contract the AI operates within.

---

### Step 4: Run Your First ICR Cycle

With the structure in place, run the Starter Ritual above.

If you want AI assistance with the first cycle, say to Claude Code:

```
Read CLAUDE.md and the .gdd/ folder.
Then guide me through the first GDD ICR cycle for this project.
Ask me the questions. Help me stress the assumptions.
Surface the residue. Do not generate any implementation code
until we have a governed constraint set with at least one
resolved constraint and an explicit list of open residue items.
```

Claude Code will:
1. Ask you to name the domain and the stakes
2. Walk you through writing and stressing assumptions
3. Help you classify each one
4. Compile the unresolvable residue
5. Write the output to `.gdd/CONSTRAINTS.md` and `.gdd/RESIDUE.md`
6. Wait for your gate decisions before proceeding to build

---

### Step 5: Gate and Build

Once the first ICR cycle is complete:

1. Review `RESIDUE.md` with the relevant stakeholders
2. Record decisions in `GATES.md`
3. Update `CONSTRAINTS.md` — move resolved items from `[UNKNOWN]` to `[RESOLVED]`
4. Tell Claude Code: *"The gate is clear. Proceed."*

From this point forward, Claude Code operates within the governed
constraint set. Every implementation pass begins with a read of
`.gdd/CONSTRAINTS.md`. Every new unknown surfaces as a proposed
residue item before code is generated.

---

### The Ongoing Rhythm

GDD is not a one-time init. It is a cycle that runs alongside the project.

```
New feature or phase approaching?
  → Trigger a new ICR cycle
  → New .gdd/CYCLES/YYYY-MM-DD-[feature].md
  → New residue items surface
  → Gate clears them before build begins

Something unexpected surfaces during implementation?
  → Claude Code proposes a new constraint
  → You review and classify
  → Gate decision recorded
  → Build continues within the updated constraint set

Project handed to a new team member or new AI agent?
  → They read .gdd/ first
  → The constraint set is the onboarding document
  → The gate decisions are the institutional memory
```

---

### Tooling Notes

GDD does not require specific tooling. The `.gdd/` folder and
a `CLAUDE.md` file are sufficient to make GDD operational.

For practitioners in the Semantic Intent ecosystem:

- Author constraint sets in **RECALL** (`.rcl`) for machine-legibility
- Carry constraints forward as **EMBER** `.sil` artifacts across pipelines
- Design gates as **Synthesis Gates** — architectural nodes, not interrupts
- Record every gate decision in **TRACE** — append-only, attributable

---

## What to Do Next

| If you want to... | Go to... |
|---|---|
| See GDD in practice across real domains | [`EXAMPLES.md`](./EXAMPLES.md) |
| Understand the full concept and lineage | [`docs/governance-driven-design.md`](./docs/governance-driven-design.md) |
| Understand how GDD connects to the ecosystem | [`README.md`](./README.md) |
| Get the CLAUDE.md template | [`templates/CLAUDE.md`](./templates/CLAUDE.md) |
| Contribute an example from your own practice | Open a PR to [`EXAMPLES.md`](./EXAMPLES.md) |

---

*The starter ritual above is sufficient to run your first GDD cycle.*
*You do not need permission. You do not need tooling.*
*You need a blank document, the discipline to write down what you are assuming,*
*and the honesty to stop when you find something you do not know.*

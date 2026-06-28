# Governance-Driven Design (GDD)
### Formalizing Intent Before the Machine Runs

**Shatny, Michael**  
Independent Researcher | Semantic Intent Ecosystem  
June 2026

---

## Abstract

Governance-Driven Design (GDD) is a pre-execution discipline for AI-first delivery lifecycles. Where Test-Driven Development (TDD) defined correctness at the implementation layer, and Behaviour-Driven Development (BDD) elevated it to observable behaviour, GDD moves the definition of correctness upstream — into the governance layer, before any artifact is produced, before any agent is invoked. GDD proposes that in AI-first delivery, the most valuable and most fragile moment is not implementation but formalization: the act of making implicit expectations explicit, testable, and governed. This paper positions GDD within the established lineage of driven-design disciplines, connects it to the Semantic Intent body of work (Shatny, 2025–2026), and argues that GDD is not a replacement for prior practices but their upstream completion.

---

## 1. The Problem GDD Addresses

AI-first delivery is fast. Dangerously fast. Agents generate artifacts — code, schemas, analyses, decisions — at a pace that outstrips the organizational capacity to understand what was built, why it was built that way, and whether it is correct.

The response to this problem has largely been reactive: add governance after the fact, layer compliance checks onto existing pipelines, treat oversight as an audit function rather than a design function. This produces what the broader field is beginning to recognize as the **governance gap** — the distance between what an AI system was instructed to do and what it can be held accountable for doing.

The governance gap is not a tooling problem. It is a sequencing problem.

Governance applied after execution is remediation. Governance applied before execution is design.

GDD is the discipline of applying governance before execution — of treating the formalization of intent, constraints, and expected outcomes as the first deliverable of any AI-first lifecycle, not the last.

---

## 2. Lineage: The Upstream Migration of Correctness

GDD does not emerge from nowhere. It is the next step in a migration that has been underway for decades — the progressive movement of "correctness" earlier in the delivery lifecycle.

```
TDD  (2002–03)  →  correctness defined at implementation
DDD  (2003)     →  correctness defined at the domain model
BDD  (2006)     →  correctness defined at behaviour (built on DDD)
GDD  (2026)     →  correctness defined at the governance layer
                   before any artifact exists
```

Each transition shared a common pattern:

- A prior practice was producing correct-looking output that failed in ways the practice could not detect
- The failure was traced to assumptions made too late — after commitment, after cost
- A new discipline moved the definition of correctness earlier, making those assumptions explicit before they became expensive

TDD caught implementation errors before they reached production.  
DDD caught domain model incoherence before it fractured the architecture.  
BDD caught behaviour mismatches before they reached users.  
GDD catches **governance failures before any agent runs** — before any artifact is produced that embeds an unexamined assumption into the delivery chain.

The migration is not philosophical preference. It is the natural response to increasing delivery velocity. As the gap between "code written" and "code in production" compressed, each discipline moved correctness earlier to stay ahead of the cost curve. AI-first delivery compresses that gap to near-zero. GDD is the logical response.

*A note on DDD's place in this lineage:* Domain-Driven Design addressed a different dimension than TDD or BDD — not test coverage or behaviour specification, but the alignment of software models with the domain they represent. Its mechanism was ubiquitous language and bounded contexts, not test-first cycles. DDD belongs in this lineage because it moved design *intent* upstream, making implicit domain assumptions explicit before implementation began. GDD extends this: where DDD formalized what the domain *is*, GDD formalizes what the *governance layer requires* — before any artifact exists. The analogy across these four practices is structural, not identity. Each solved a different problem at a different layer.

---

## 3. What GDD Is — And Is Not

### 3.1 What GDD Is

GDD is a pre-execution formalization discipline. Its primary output is not code, not tests, not documentation — it is a **governed constraint set**: a formally structured, machine-readable and human-readable definition of what the system must hold true, what it must not violate, and what remains unresolved and requires human judgment before execution proceeds.

GDD operates through **Iterative Constraint Refinement (ICR)** — a cycle in which:

1. The AI proposes what it believes the domain expects
2. It generates the condition that would falsify that belief
3. It checks internal consistency and achievability
4. It surfaces what it cannot resolve — the **unresolvable residue** — as the primary human-facing deliverable

The unresolvable residue is the most valuable output of a GDD cycle. It is the set of decisions that genuinely require human judgment — the places where business rules are undefined, where regulatory constraints conflict, where the domain has not yet been fully understood. Surfacing these before implementation is the discipline. Resolving them before any agent runs is the governance act.

### 3.2 What GDD Is Not

GDD is not a universal mandate. Not every delivery lifecycle requires it. Prototypes, exploratory tooling, and low-stakes automation do not need the formalization overhead GDD introduces.

GDD is applicable when:
- Stakes are high and visibility is low
- Regulatory or compliance constraints are material
- Multiple agents will operate on shared state
- The domain contains implicit assumptions that have never been formally examined
- Failure is expensive to detect after the fact

GDD is the discipline you reach for when it matters — not the process you apply to everything. This is consistent with the original intent of TDD and BDD: powerful when applied where the cost of being wrong is real, wasteful when applied where it is not.

### 3.3 GDD and Other Pre-Build Disciplines

Event Storming (Brandolini, 2013) and Impact Mapping (Adzic, 2012) are established pre-build practices that surface domain assumptions, align stakeholders, and reduce delivery risk before development begins. GDD is aware of both — and does not replace them.

Two distinctions are worth stating explicitly.

**GDD is AI-first by design.** Event Storming and Impact Mapping produce outputs for human consumers: sticky-note maps, diagrams, story frameworks. GDD produces a Governed Constraint Set — a formally structured artifact that an AI agent can read, operate within, and surface violations of. The CLAUDE.md template is what makes this concrete: it converts the governed constraint set into a governance contract the model executes against, not a document that may or may not survive the handoff to the machine.

**GDD names the unresolvable as a primary deliverable.** Event Storming and Impact Mapping aim to resolve ambiguity through collaborative discovery. GDD adds a formal category — the unresolvable residue — for what *cannot* be resolved in the room and requires a named human decision before execution proceeds. This is not a gap in the process. It is the process's most valuable output.

A team that has already run Event Storming has a strong starting inventory for a GDD cycle. The domain events, commands, and aggregate assumptions from the storming session become the first pass at the FORMALIZE step. GDD does not replace the storming session — it extends it upstream into the governance layer, and makes the output machine-operable.

---

## 4. The Mechanism: Iterative Constraint Refinement

ICR is the operational core of GDD. It is not a linear process. It is a cycle with a defined exit condition.

```
CYCLE:
  1. FORMALIZE   — propose domain expectations as explicit constraints
  2. STRESS      — generate the condition that would break each constraint
  3. CHECK       — verify internal consistency across all constraints
  4. SURFACE     — identify what cannot be resolved without human input
  5. GATE        — human reviews the unresolvable residue
       → resolved: absorb into constraint set, continue cycle
       → unresolvable: escalate, defer, or reframe scope
  6. CONVERGE    — cycle terminates when constraint set is internally
                   consistent and all unresolvable residue is
                   explicitly acknowledged
```

The exit condition is not "all constraints resolved." Many will not be. The exit condition is **explicit acknowledgment of what remains unresolved** — which is itself a governance act. An organization that proceeds with full awareness of its unresolved constraints is in a fundamentally different position than one that proceeds without knowing they exist.

---

## 5. Connection to the Semantic Intent Ecosystem

GDD is the upstream source of the Semantic Intent (SI) contracts described in the prior body of work.

**Semantic Intent as Single Source of Truth** (Shatny, 2025) established that a unified WHAT + WHY construct, made immutable, governs AI behavior across transformations. GDD is where that construct is authored. The governed constraint set produced by a GDD cycle is the raw material from which SI contracts are derived.

**Semantic Intent as Governance Primitive** (Shatny, May 2026) extended SI to multi-agent systems, proposing immutability, addressability, and provenance as required properties of cross-agent contracts. GDD is the discipline that produces contracts with those properties — not by accident, but by design.

**Semantic Intent as Emitted Directive** (Shatny, June 2026) documented the runtime mode where governance is generated dynamically, calibrated to the artifact received. GDD produces the governance vocabulary that emitted directives draw from — the formal constraint language that makes dynamic generation coherent rather than arbitrary.

**The Synthesis Gate** (Shatny, June 2026) named the architectural moment where ambiguity surfaces and human judgment closes the loop. GDD designs that moment in deliberately — the human gate in the ICR cycle is a Synthesis Gate, placed upstream before any agent runs rather than encountered reactively mid-pipeline.

**Methodology-as-Infrastructure** (Shatny, June 2026) demonstrated that analytical methodologies can be compiled into deterministic execution layers. GDD is the methodology that, once formalized, becomes the governance infrastructure other systems build upon — the pre-compiled constraint set that CAL, REACH, OCTO, and downstream agents operate within.

The relationship is architectural:

```
GDD
 └─ produces: Governed Constraint Set
      └─ expressed as: Semantic Intent contracts
           └─ carried by: EMBER (.sil artifacts)
           └─ executed within: CAL / REACH / OCTO
           └─ gated by: Synthesis Gate
           └─ remembered by: TRACE
           └─ optimized across: GESA episodes
           └─ preserved in: CROC
```

GDD is not separate from the Semantic Intent ecosystem. It is its origin point.

---

## 6. GDD and the Human-as-Primitive Principle

A consistent property across the Semantic Intent body of work is **human-as-primitive** — the architectural position that human judgment is not an optional approval step but a first-class element of agentic system design.

GDD operationalizes this principle at the earliest possible moment. The ICR cycle does not treat human input as an interrupt — something that pauses the machine when something goes wrong. It treats human input as a **designed-in gate** — the moment where the machine has reached the boundary of what it can formalize and hands the unresolvable residue to the human who can resolve it.

This is the structural difference between governance-as-audit and governance-as-design. The former waits for failure. The latter designs for the conditions under which failure becomes visible before it becomes costly.

---

## 7. Why Now

Three conditions converge in 2026 to make GDD both possible and necessary.

**Velocity without structure produces debt that compounds.** The vibe coding pattern — AI-generated code produced without upfront formalization — is producing measurable degradation in delivery stability. DORA's 2024 data showed a 7.2% reduction in delivery stability accompanying increased AI adoption. The organizations that will avoid this pattern are those that invest in formalization before execution, not remediation after.

**Agentic systems require pre-execution contracts.** Single-agent workflows can absorb ambiguity through iteration. Multi-agent pipelines cannot — one agent's ambiguous output becomes another agent's unexamined input, and the propagation of unresolved assumptions compounds across the chain. GDD produces the contracts that make multi-agent coordination trustworthy.

**The governance conversation is already happening — without a design discipline to ground it.** Enterprise architecture, regulatory frameworks, and AI governance initiatives are all converging on the same insight: AI systems need governance. What is missing is a practice that makes governance a design act rather than a compliance act. GDD is that practice.

---

## 8. Applicability and Honest Scope

GDD will not be adopted universally. It should not be.

The practices in its lineage — TDD, BDD, DDD — were never universal either. Their value was not in ubiquity but in availability: known, understood, reachable by practitioners who recognized the conditions under which the discipline paid for itself.

GDD is most valuable in:
- Regulated industries where implicit assumptions carry compliance risk
- Legacy modernization programs where business rules are buried and unexamined
- Multi-agent enterprise deployments where governance gaps compound across agents
- High-stakes, low-visibility domains where the cost of discovering a wrong assumption in production is disproportionate to the cost of surfacing it in formalization

GDD is least valuable in:
- Rapid prototyping and exploratory work
- Low-stakes automation and tooling
- Contexts where the domain is well-understood and constraints are already explicit

The practitioner's judgment about which context they are in is itself a governance act.

---

## 9. Positioning Statement

GDD is not a replacement for TDD, BDD, or DDD. It is their upstream completion.

The lineage did not fail. It reached as far upstream as the tools and delivery contexts of its time allowed. AI-first delivery extends the frontier — moving execution so far downstream that a new upstream discipline becomes necessary.

The instinct to abandon established practices in favor of whatever is new is almost always overcorrected later. The better posture is to understand what those practices were protecting against, recognize that the underlying risk does not disappear because the tooling changed, and ask what the equivalent protection looks like at the new frontier.

GDD is that protection, positioned at the new frontier.

---

## 10. Conclusion

Governance-Driven Design proposes a simple resequencing: govern before you build, not after.

In an AI-first delivery lifecycle, where agents can produce thousands of lines of code, architectural decisions, and data transformations before a human has reviewed a single output, the governing question is not "did it work?" — it is "did we know what working meant before we started?"

GDD is the discipline that answers that question with something better than hope.

The governed constraint set it produces is the upstream artifact that makes everything downstream — the Semantic Intent contracts, the agentic pipelines, the multi-agent coordination, the human gates, the audit trails — trustworthy rather than merely fast.

Trustworthy is the harder property. GDD is the practice that earns it.

---

## References

Shatny, M. (2025). *Semantic Intent as Single Source of Truth: Immutable Governance for AI-Assisted Development*. Zenodo. https://doi.org/10.5281/zenodo.17114971

Shatny, M. (2026). *Semantic Intent as Governance Primitive for Agentic Systems*. Zenodo. https://doi.org/10.5281/zenodo.20436087

Shatny, M. (2026). *Semantic Intent as Emitted Directive: Dynamic Governance Primitive Generation in MCP Tool Pipelines*. Zenodo. https://doi.org/10.5281/zenodo.20563444

Shatny, M. (2026). *Methodology-as-Infrastructure: From Framework to Runtime*. Zenodo. https://doi.org/10.5281/zenodo.18946630

Shatny, M. (2026). *Intent-as-Infrastructure: When the Compiler Is Claude*. Zenodo. https://doi.org/10.5281/zenodo.20681522

Shatny, M. (2026). *The Synthesis Gate: What Makes a Workflow Agentic*. Zenodo. https://doi.org/10.5281/zenodo.20684282

Shatny, M. (2026). *GESA: Generative Episodic Simulated Annealing*. Zenodo. https://doi.org/10.5281/zenodo.20533843

Shatny, M. (2026). *CROC: Contextual Retrievable Organizational Corpus*. Zenodo. https://doi.org/10.5281/zenodo.20777675

Shatny, M. (2026). *EMBER: Semantic Intent Language for Project Phoenix*. Zenodo. https://doi.org/10.5281/zenodo.19751387

Shatny, M. (2026). *Project Phoenix: The Agentic Greenfield Pipeline for Legacy Modernization*. Zenodo. https://doi.org/10.5281/zenodo.19360727

Beck, K. (2003). *Test-Driven Development: By Example*. Addison-Wesley.

North, D. (2006). *Introducing BDD*. DanNorth.net.

Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.

Brandolini, A. (2013). *Introducing Event Storming*. Ziobrando.net.

Adzic, G. (2012). *Impact Mapping: Making a Big Impact with Software Products and Projects*. Provoking Thoughts.

DORA. (2024). *Accelerate State of DevOps Report*. Google Cloud.

---

*Governance-Driven Design (GDD) is proposed as an open concept. The author encourages its adoption, adaptation, and critique. No claim of exclusivity is made. The measure of a methodology is whether it helps practitioners think more clearly — not whether it is owned.*

---

**Keywords:** governance-driven design, GDD, semantic intent, agentic systems, iterative constraint refinement, AI governance, pre-execution formalization, methodology-as-infrastructure, human-as-primitive, delivery lifecycle

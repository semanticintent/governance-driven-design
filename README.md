[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20938777.svg)](https://doi.org/10.5281/zenodo.20938777)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0006--2011--3258-brightgreen?logo=orcid)](https://orcid.org/0009-0006-2011-3258)

# Governance-Driven Design (GDD)

> *Formalize intent before the machine runs.*

**GDD** is the organizing philosophy of the Semantic Intent ecosystem — a pre-execution discipline for AI-first delivery that asks one question before any agent is invoked, any artifact produced, or any pipeline started:

**Do we know what correct means — and can we prove it?**

This repository is the map. It explains what GDD is, why it exists, and how every component in this ecosystem serves a layer of the GDD discipline.

---

## The Problem

AI-first delivery is fast. Agents generate code, schemas, analyses, and decisions at a pace that outstrips the organizational capacity to understand what was built, why, and whether it is correct.

The instinctive response has been to add governance after the fact — compliance layers, audit trails, oversight dashboards applied once something has already been produced.

That is remediation. It is expensive, late, and reactive.

**GDD is the alternative: governance as design, not governance as audit.**

The discipline moves the definition of correctness to the earliest possible moment — before any artifact exists, before any agent runs — so that what gets built is what was actually meant.

---

## The Lineage

GDD does not emerge from nowhere. It is the next step in a migration that has been underway for decades.

```
TDD  (2002–03)  →  correctness defined at implementation
DDD  (2003)     →  correctness defined at the domain model
BDD  (2006)     →  correctness defined at behaviour (built on DDD)
GDD  (2026)     →  correctness defined at the governance layer
                   before any artifact exists
```

Each transition shared the same pattern: a prior practice was producing correct-looking output that failed in ways the practice could not detect. The failure was traced to assumptions made too late — after commitment, after cost. A new discipline moved the definition of correctness earlier.

AI-first delivery compresses the gap between "designed" and "in production" to near-zero. GDD is the logical response — the discipline that stays ahead of that compression.

**GDD is not a replacement for TDD, BDD, or DDD. It is their upstream completion.**

*On DDD specifically:* Domain-Driven Design addressed design intent — making domain assumptions explicit through ubiquitous language and bounded contexts — rather than test or behaviour correctness. It belongs in this lineage because it moved formalization upstream. GDD extends it: where DDD formalized what the domain *is*, GDD formalizes what the governance layer *requires*. The analogy is structural, not identity.

---

## The Core Mechanism: Iterative Constraint Refinement (ICR)

GDD operates through a cycle with a defined exit condition.

```
╔══════════════════════════════════════════════════════╗
║          ITERATIVE CONSTRAINT REFINEMENT             ║
╠══════════════════════════════════════════════════════╣
║                                                      ║
║  1. FORMALIZE   propose domain expectations          ║
║                 as explicit constraints              ║
║                                                      ║
║  2. STRESS      generate the condition that          ║
║                 would break each constraint          ║
║                                                      ║
║  3. CHECK       verify internal consistency          ║
║                 across all constraints               ║
║                                                      ║
║  4. SURFACE     identify what cannot be resolved     ║
║                 without human input                  ║
║                                                      ║
║  5. GATE ──────────────────────────────────────────► ║
║     human reviews the unresolvable residue           ║
║       → resolved: absorb, continue cycle             ║
║       → unresolvable: escalate, defer, reframe       ║
║                                                      ║
║  6. CONVERGE    exit when constraint set is          ║
║                 internally consistent and all        ║
║                 unresolvable residue is explicitly   ║
║                 acknowledged                         ║
║                                                      ║
╚══════════════════════════════════════════════════════╝
```

### The Primary Deliverable

The ICR cycle produces a **Governed Constraint Set** — a formally structured definition of:
- What the system must hold true
- What it must not violate
- What remains unresolved and requires human judgment before execution proceeds

The **unresolvable residue** is the most valuable output. It is the set of decisions that genuinely require human judgment — the places where business rules are undefined, where constraints conflict, where the domain has not been fully examined. Surfacing these before implementation is the discipline. Resolving them before any agent runs is the governance act.

### The Exit Condition

The cycle does not terminate when all constraints are resolved. Many will not be. The exit condition is **explicit acknowledgment of what remains unresolved** — which is itself a governance act.

An organization that proceeds with full awareness of its unresolved constraints is in a fundamentally different position than one that proceeds without knowing they exist.

---

## The Ecosystem Map

Every component in the Semantic Intent ecosystem serves a layer of the GDD discipline. This is not by design imposed from above — GDD was named after the pattern was already present across independently developed tools.

```
GDD  (this repo — the philosophy)
 │
 └─► GOVERNED CONSTRAINT SET produced by ICR cycle
      │
      ├─► expressed as  SEMANTIC INTENT contracts
      │    │
      │    ├── Semantic Intent as Single Source of Truth (2025)
      │    │   DOI: 10.5281/zenodo.17114971
      │    │
      │    ├── Semantic Intent as Governance Primitive (2026)
      │    │   DOI: 10.5281/zenodo.20436088
      │    │
      │    └── Semantic Intent as Emitted Directive (2026)
      │        DOI: 10.5281/zenodo.20563477
      │
      ├─► carried by   EMBER (.sil artifact files)
      │    npm: @semanticintent/ember
      │    — intent and state preserved across agents
      │
      ├─► executed within
      │    ├── CAL / @stratiqx/cal-runtime
      │    │   — deterministic cascade analysis language
      │    │
      │    ├── REACH + OCTO
      │    │   — local workflow practice, AI compilation,
      │    │     human judgment as first-class primitive
      │    │
      │    ├── Phoenix Runtime / @semanticintent/phoenix-runtime
      │    │   — 7-agent legacy modernization pipeline
      │    │
      │    └── Strata Runtime / @semanticintent/strata-runtime
      │        — 5-agent database archaeology pipeline
      │
      ├─► gated by     SYNTHESIS GATE
      │    github: semanticintent/synthesis-gate
      │    — the moment ambiguity surfaces to human judgment
      │    — designed in, not encountered reactively
      │
      ├─► written in   RECALL
      │    npm: @semanticintent/recall-compiler
      │    — machine-legible, intent-dense, COBOL-inspired
      │    — constraint sets that both humans and models read
      │
      ├─► remembered by TRACE
      │    github: semanticintent/trace
      │    — append-only execution record
      │    — every decision, every artifact, every gate
      │
      ├─► optimized across GESA episodes
      │    — generative episodic simulated annealing
      │    — the system improves across runs
      │
      └─► preserved in CROC
           github: semanticintent/croc-framework
           — institutional knowledge before it walks out the door
```

---

> **Step zero is the CLAUDE.md.** Placing a `CLAUDE.md` in your project root — pointing Claude Code to a `.gdd/` directory and instructing it on constraint status directives — is immediately actionable even before a full ICR cycle has run. The CLAUDE.md and the ICR cycle are separable: step zero makes AI agents governance-aware; step one fills the constraint set they operate within. Both together is GDD in full. Either alone is still a step forward.

---

## When GDD Applies

GDD is not a universal mandate. It is a discipline you reach for when the conditions warrant it.

### Reach for GDD when:
- Stakes are high and visibility is low
- Regulatory or compliance constraints are material
- Multiple agents will operate on shared state
- The domain contains implicit assumptions that have never been formally examined
- Failure is expensive to detect after the fact
- A legacy system holds business rules nobody has written down

### GDD is not needed when:
- Rapid prototyping and exploratory work
- Low-stakes automation and internal tooling
- The domain is well-understood and constraints are already explicit
- The cost of being wrong is low and recoverable

**The practitioner's judgment about which context they are in is itself a governance act.**

---

## GDD and Related Practices

Event Storming (Brandolini, 2013) and Impact Mapping (Adzic, 2012) are established pre-build disciplines that surface domain assumptions, align stakeholders, and reduce delivery risk before development begins. GDD does not replace them.

Two distinctions matter:

**GDD is AI-first by design.** Both Event Storming and Impact Mapping produce outputs for human consumers — maps, diagrams, story frameworks. GDD produces a machine-readable Governed Constraint Set. The CLAUDE.md is what makes this operational: it converts the constraint set into a governance contract an AI agent executes within, not a document that may or may not survive the handoff to the machine.

**GDD names the unresolvable as a primary deliverable.** Event Storming and Impact Mapping aim to resolve ambiguity through collaborative discovery. GDD adds a formal category — the unresolvable residue — for what cannot be resolved in the room and requires a named human decision before execution proceeds.

A team that has already run Event Storming has a strong starting inventory for a GDD cycle. The domain events, commands, and assumptions from the storming session become the first pass at the FORMALIZE step. GDD does not replace the prior practice — it extends it into the governance layer and makes the output machine-operable.

---

## The Human-as-Primitive Principle

A consistent property across this entire ecosystem is that **human judgment is a first-class architectural element** — not an optional approval step, not an interrupt when something goes wrong.

GDD operationalizes this at the earliest possible moment. The ICR cycle does not treat human input as a pause in the machine's execution. It treats the Synthesis Gate — the moment where the machine hands the unresolvable residue to a human — as a **designed-in architectural node**.

This is the structural difference between governance-as-audit and governance-as-design.

---

## The Philosophy Behind the Practice

The AI shift in software delivery is real and accelerating. The instinct to abandon established practices in favour of whatever is new is also real — and almost always overcorrected later.

The better posture: **look back before you leap forward.**

TDD, BDD, DDD were not arbitrary conventions. They were responses to real pain. They encoded hard-won understanding about where projects fail, where ambiguity compounds, where human judgment is irreplaceable. That knowledge does not expire because the tooling changed.

GDD is a reconnection — taking what those practices understood about discipline, formalization, and structured thinking, and asking what that looks like when AI is doing most of the execution.

It is also not the last word. GDD will have successors. The migration of correctness upstream will continue as delivery velocity continues to increase. What matters is not the specific practice but the underlying discipline: **define correctness before you build, not after you break.**

---

## Published Research

The formal academic grounding for GDD and the Semantic Intent ecosystem:

| Paper | Year | DOI |
|---|---|---|
| Semantic Intent as Single Source of Truth | 2025 | [10.5281/zenodo.17114971](https://doi.org/10.5281/zenodo.17114971) |
| Semantic Intent as Governance Primitive | 2026 | [10.5281/zenodo.20436088](https://doi.org/10.5281/zenodo.20436088) |
| Semantic Intent as Emitted Directive | 2026 | [10.5281/zenodo.20563477](https://doi.org/10.5281/zenodo.20563477) |
| Methodology-as-Infrastructure | 2026 | [10.5281/zenodo.18946630](https://doi.org/10.5281/zenodo.18946630) |
| Intent-as-Infrastructure | 2026 | [10.5281/zenodo.20681523](https://doi.org/10.5281/zenodo.20681523) |
| The Synthesis Gate | 2026 | [10.5281/zenodo.20684283](https://doi.org/10.5281/zenodo.20684283) |
| Governance-Driven Design (GDD) | 2026 | [10.5281/zenodo.20938777](https://doi.org/10.5281/zenodo.20938777) |
| Machine Legibility Dimensions | 2026 | [10.5281/zenodo.19546590](https://doi.org/10.5281/zenodo.19546590) |
| GESA | 2026 | [10.5281/zenodo.20533843](https://doi.org/10.5281/zenodo.20533843) |
| CROC | 2026 | [10.5281/zenodo.20777675](https://doi.org/10.5281/zenodo.20777675) |

*Full citation chain: see [Methodology-as-Infrastructure Appendix B](https://doi.org/10.5281/zenodo.18946630)*

---

## Ecosystem Packages

| Package | Role in GDD |
|---|---|
| [`@semanticintent/ember`](https://www.npmjs.com/package/@semanticintent/ember) | Constraint carrier — .sil artifacts across agents |
| [`@semanticintent/phoenix-runtime`](https://www.npmjs.com/package/@semanticintent/phoenix-runtime) | GDD in practice — 7-agent legacy pipeline with human gates |
| [`@semanticintent/strata-runtime`](https://www.npmjs.com/package/@semanticintent/strata-runtime) | GDD in practice — 5-agent database archaeology with human gates |
| [`@semanticintent/recall-compiler`](https://www.npmjs.com/package/@semanticintent/recall-compiler) | Constraint language — machine-legible intent authoring |
| [`@semanticintent/recall-ui`](https://www.npmjs.com/package/@semanticintent/recall-ui) | RECALL component library |
| [`@stratiqx/cal-runtime`](https://www.npmjs.com/package/@stratiqx/cal-runtime) | Deterministic execution within governed constraints |
| [`@semanticintent/semantic-wake-intelligence-mcp`](https://www.npmjs.com/package/@semanticintent/semantic-wake-intelligence-mcp) | Temporal intelligence — constraint durability across time |

---

## Citing GDD

If GDD is useful to your thinking, your project, or your research:

```
Shatny, M. (2026). Governance-Driven Design (GDD): Formalizing Intent 
Before the Machine Runs. Zenodo. https://doi.org/10.5281/zenodo.20938777
```

---

## Contributing

GDD is an open concept. No claim of exclusivity is made.

The measure of a methodology is whether it helps practitioners think more clearly — not whether it is owned.

If GDD resonates with your practice, the most valuable contribution is a documented example: a domain where you applied the ICR cycle, what the unresolvable residue surfaced, and what that changed about how you built. Open a discussion or submit a PR to `EXAMPLES.md`.

---

## Author

**Michael Shatny** — Independent Researcher, Semantic Intent Ecosystem  
[semanticintent.dev](https://semanticintent.dev) · [Zenodo profile](https://zenodo.org/search?q=shatny) · [GitHub @semanticintent](https://github.com/semanticintent)

---

*GDD was named after the pattern was already present. The ecosystem built it before anyone called it governance-driven design. That is usually how the right abstractions arrive — not invented, but recognized.*

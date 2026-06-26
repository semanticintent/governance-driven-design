# CLAUDE.md

> This project uses **Governance-Driven Design (GDD)**.
> This file instructs you on how to operate within the GDD governance layer.
> Read this file completely before taking any action in this project.

---

## What GDD Is

Governance-Driven Design is a pre-execution discipline that formalizes
intent before any artifact is produced. This project maintains a `.gdd/`
directory that contains the Governed Constraint Set — the ground truth
for what this system must hold true, must not violate, and has not yet
resolved.

Your job is to build within that constraint set — not around it.

Reference: https://github.com/semanticintent/governance-driven-design

---

## Before You Generate Anything

Before writing any implementation code, any schema, any configuration,
or any test — read the GDD layer:

```
1. Read .gdd/CONSTRAINTS.md       — the governed constraint set
2. Read .gdd/RESIDUE.md           — open items requiring human decision
3. Read .gdd/GATES.md             — human decisions already made
```

Then ask yourself:

```
→ Does any [UNKNOWN] or [CONFLICT] item in RESIDUE.md
  affect what I am about to generate?

  If YES  — surface it to the human before proceeding.
            Do not generate code that depends on an unresolved item.

  If NO   — proceed, but note which constraints govern your output.
```

---

## Constraint Status Reference

Every constraint in `.gdd/CONSTRAINTS.md` carries a status.
Treat each status as a directive:

```
[RESOLVED]   — implement exactly as stated
               do not reinterpret, do not optimize away
               the constraint exists because a human decided it

[ASSUMED]    — implement as stated, but flag the assumption
               add a comment: // GDD:ASSUMED — verify before production
               surface it if it affects correctness

[UNKNOWN]    — do not implement logic that depends on this item
               surface it to the human: "I cannot proceed on X
               without a decision on CONSTRAINT-NNN"

[CONFLICT]   — do not implement either side of the conflict
               surface both sides to the human for resolution
               implementing one silently is worse than stopping
```

---

## While You Generate

When your output implements a governed constraint, trace it:

```typescript
// GDD:CONSTRAINT-001 — DTI threshold is regulatory floor, not policy
// Resolution: CCO confirmed 43% immutable (2026-06-26)
if (dtiRatio >= 0.43) {
  return decline(REASON.DTI_EXCEEDED);
}
```

The comment format is:
```
// GDD:[CONSTRAINT-ID] — [one-line statement of what constraint this implements]
// Resolution: [who decided, what they decided, when]  (for RESOLVED items)
// Assumed: [what is being assumed]                    (for ASSUMED items)
```

This is not optional ceremony. It is the traceability that makes
the governed constraint set useful over time — especially when
the next developer (or the next agent) asks "why is this number 43?"

---

## When You Encounter New Unknowns

If during implementation you discover an assumption that is not in
`.gdd/CONSTRAINTS.md` — stop and surface it.

Do not resolve it yourself. Do not pick the most reasonable option
and proceed silently. Do not note it in a comment and move on.

Surface it as a proposed residue item:

```
I've encountered an assumption that is not in the GDD constraint set:

  Statement: [what you are about to assume]
  Stress: this breaks if [what would make it wrong]
  Proposed status: [ASSUMED / UNKNOWN]
  Affects: [what you cannot safely generate without a decision]

Should I add this to .gdd/RESIDUE.md and pause,
or has this been decided and I should proceed?
```

This is the Synthesis Gate — the moment where your execution
surfaces ambiguity to human judgment rather than absorbing it silently.

---

## Business Logic Rules

Business logic — eligibility checks, approval thresholds, calculation
methods, routing rules, compliance conditions — is always governed.

Before generating any business logic:

```
→ Check whether it is covered by a constraint in .gdd/CONSTRAINTS.md
→ If covered — implement against the constraint, trace it
→ If not covered — ask: "Should this be a governed constraint
                         before I implement it?"
```

Business logic that is not in the constraint set is an assumption.
Assumptions that are not in the constraint set are invisible risk.
Invisible risk is what GDD exists to eliminate.

---

## Tests and the GDD Layer

When generating tests for governed logic:

```
→ Tests for [RESOLVED] constraints are required — not optional
→ The test should assert the exact constraint, not an approximation
→ Name the test after the constraint:

  describe('CONSTRAINT-001: DTI threshold is regulatory floor', () => {
    it('declines when DTI equals 43%', ...
    it('declines when DTI exceeds 43%', ...
    it('does not decline when DTI is 42.99%', ...
  })
```

A test that does not reference the constraint it is protecting
is a test that can be silently deleted without anyone knowing
what governance it was enforcing.

---

## What You Must Never Do

```
✗ Do not implement logic that depends on an [UNKNOWN] item
✗ Do not resolve a [CONFLICT] by picking a side silently
✗ Do not reinterpret a [RESOLVED] constraint for elegance
✗ Do not delete or modify .gdd/ files — append only
✗ Do not add governance comments that are not traceable to a constraint
✗ Do not proceed past a Synthesis Gate without human confirmation
```

---

## What You Should Always Do

```
✓ Read .gdd/ before every implementation pass
✓ Trace every governed output back to its constraint
✓ Surface new unknowns immediately — before generating
✓ Name tests after the constraints they protect
✓ Treat the unresolvable residue as a feature, not a blocker
✓ Ask: "is there a constraint that governs this?" before assuming
```

---

## Updating the GDD Layer

You may propose additions to the GDD layer. You may not make them
without human review.

**To propose a new constraint:**
```
I propose adding the following to .gdd/CONSTRAINTS.md:

  CONSTRAINT-[NNN]
  Status: [ASSUMED / UNKNOWN]
  Statement: [what you believe to be true]
  Stress: [what would break it]
  Reason: [why this emerged during implementation]

Shall I add this and continue, or would you like to resolve
the status before I proceed?
```

**To propose closing a residue item:**
```
RESIDUE item [ID] appears to have been resolved by [gate decision / code review / stakeholder input].

Proposed resolution: [statement]
Proposed owner: [who decided]
Proposed date: [when]

Shall I update .gdd/CONSTRAINTS.md and .gdd/GATES.md?
```

All `.gdd/` file updates require explicit human confirmation before being written.

---

## The GDD Folder Structure

```
.gdd/
  CONSTRAINTS.md     — the governed constraint set (primary artifact)
  RESIDUE.md         — unresolved items requiring human decision
  GATES.md           — record of human gate decisions
  CYCLES/            — ICR cycle history (append-only)
    YYYY-MM-DD-[name].md
  SPEC/              — optional: domain specifications feeding the cycle
```

---

## First Run Checklist

If `.gdd/CONSTRAINTS.md` is empty or does not exist:

```
→ Do not generate implementation code yet
→ Tell the human: "This project has a GDD layer initialized but
                   no constraints have been formalized yet."
→ Offer to run the GDD initialization sequence:
  "Shall I guide you through the first ICR cycle to populate
   the governed constraint set before we begin building?"
→ If yes — follow the ICR cycle sequence in GETTING_STARTED.md
→ If no  — ask for explicit confirmation to proceed without
            a governed constraint set, and note the risk
```

---

*This file is a governance contract between you and this project.*
*It exists because fast and correct are not the same thing.*
*GDD is the discipline that earns the right to be fast.*

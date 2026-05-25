# Phase 1 — Purpose & contract

**Purpose:** Force a single-sentence definition of what this composite is *for*, what it promises, and what it explicitly does NOT do. This phase prevents scope creep — every later phase will be evaluated against this contract.

**Produces:** `SPEC.md` sections: Purpose, Contract, Non-goals.

**Estimated time:** 8–12 minutes.

---

## The three questions

### Question 1 — Purpose

> "In one sentence — no commas, no `and` — what is this composite *for*? What user job does it serve? Use a verb."

Good answers:
- "Lets users scan and act on a list of records with sortable columns and inline edit."  ✓
- "Lets users invoke any app action from anywhere with keyboard."  ✓

Bad answers (the user gets pushed back):
- "A table component with filtering and sorting and inline edit and bulk actions and pagination and export."  ✗ (`and` count too high — this is a feature list, not a purpose)
- "Shows data."  ✗ (no verb, no job)
- "DataTable."  ✗ (the name is not the purpose)

If the user answers badly: ask again. Specifically: "What's the user *doing* when they reach for this composite?"

### Question 2 — Contract

> "What does this composite promise the user? List 3–7 commitments. Each starts with 'The composite will'."

These are *guarantees*, not features. Examples for DataTable:
- The composite will preserve sort order across page reloads.
- The composite will keep row identity stable across re-sorts so selections don't drift.
- The composite will render the first paint within Doherty (<400ms) for up to 1000 rows.
- The composite will commit inline edits optimistically with rollback on failure.

If a "commitment" is actually a feature ("supports column resizing"), push back: "Is that a promise to the user, or a capability the implementer can choose? Reword as a promise."

### Question 3 — Non-goals

> "Name 3–5 things this composite explicitly does NOT do. These are the boundaries that prevent feature creep in implementation."

Examples for DataTable:
- Not a spreadsheet — cells don't reference each other.
- Not a pivot table — no aggregation rollups.
- Not a Kanban — no drag-to-status. (That's a separate composite.)
- Not an analytics chart — no visualizations.
- Not a tree — no expandable nested rows in this canonical SPEC. (A `TreeTable` variant exists separately.)

If the user can't name non-goals, prompt: "What composite from another product looks similar but is the *wrong* abstraction for our use? That's a non-goal."

---

## What to write

In the composite's `SPEC.md`, fill these three sections:

```markdown
## Purpose

<one-sentence purpose>

## Contract

The composite will:
- <commitment 1>
- <commitment 2>
- <commitment 3>
- <commitment 4>

## Non-goals

This composite does NOT:
- <non-goal 1> — <why; what else handles it>
- <non-goal 2> — <why; what else handles it>
- <non-goal 3> — <why; what else handles it>
```

---

## Success criteria for phase 1

- Purpose is one sentence, contains a verb, names a user job
- Contract has 3–7 user-facing promises (not implementation details)
- Non-goals has 3–5 explicit exclusions, each pointing at what *does* handle that case
- Every contract item could in principle be tested against a real user flow

---

## What NOT to do in phase 1

- Don't list features. Features are about what the composite has; this phase is about what the user gets.
- Don't make the purpose a paragraph. If it's a paragraph, the composite is overscoped — split it.
- Don't accept a non-goal of "we won't do this because it's hard." Non-goals are about *fit*, not effort.

---

## Checkpoint

Show the user the three sections side by side. Ask:

> "Does this match what you wanted? Specifically:
> - Is the purpose narrow enough that a junior could decide what's in/out from this sentence alone?
> - Does any contract item feel optional? (If so, it's a feature — move it.)
> - Are there other 'looks similar but wrong abstraction' patterns we should add as non-goals?"

Iterate until the user accepts. Lock the section before moving to phase 2.

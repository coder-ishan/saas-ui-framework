# Phase 2 — Entry points & flow enumeration

**Purpose:** Enumerate every flow that lives in this module. A flow is a user journey with an entry, steps, and an exit. Without this enumeration, phase 3 designs nothing concrete.

**Produces:** `FLOWS.md` → enumeration section (the flow specs themselves come in phase 3).

**Estimated time:** 15 minutes.

---

## What counts as a flow

A flow is a *user journey* through the module:

- Has an **entry** (user arrives via one of the entry points from OVERVIEW.md)
- Has a **goal** (what success looks like for this flow)
- Has **steps** (the sequence of interactions)
- Has an **exit** (success exit, abandon exit, or pivot to a different flow)

A flow is NOT:
- A feature ("filtering" — that's a capability within flows)
- A screen ("the detail page" — that's a node within a flow)
- A composite ("the data table" — that's a building block flows use)

---

## The two-step enumeration

### Step 1 — Walk every entry point, list what the user wants to do

For each entry point from OVERVIEW.md, ask: "User arrives via this entry point. What are they trying to do?"

Each distinct intent becomes a candidate flow.

For an Inbox module:
- Entry: sidebar nav → Intent: triage today's incoming
- Entry: notification email → Intent: respond to this specific item
- Entry: search/palette → Intent: find a specific item from history
- Entry: bookmark to inbox → Intent: triage today's incoming (same as sidebar)
- Entry: after-action (just sent a reply, returned to inbox) → Intent: continue triaging

Candidate flows from this:
- Triage flow (the default)
- Single-item action flow (from notification or search)
- Historical lookup flow
- Resume-after-action flow (a sub-variant of triage)

### Step 2 — Classify flows by pattern

Reference: `references/flow-patterns.md`.

Five canonical flow patterns:

1. **Linear flow** — one path, no branches. Example: invoice creation wizard.
2. **Branching flow** — decision points within the flow. Example: refund flow (full refund vs partial vs decline).
3. **Save-and-resume flow** — flow can be paused and re-entered. Example: filing a long support ticket.
4. **Single-action flow** — a single click/keystroke achieves the goal. Example: marking an inbox item read.
5. **Continuous flow** — the user is in the module doing the same thing repeatedly. Example: triaging inbox.

Classify each candidate flow. The pattern governs how phase 3 specs the flow:
- Linear flows need a progress indicator (Goal-Gradient).
- Branching flows need clear decision affordances.
- Save-and-resume flows need resumability design (Zeigarnik).
- Single-action flows need keyboard primary affordances.
- Continuous flows need fatigue management (Flow law).

### Step 3 — Drop, merge, prioritize

- **Drop:** flows that don't actually live in this module (the user goes to a different module to do them).
- **Merge:** flows that share steps and differ only in entry — keep one canonical flow with multiple entry conditions.
- **Prioritize:** by Pareto from PRINCIPLES.md. If 80% of time in this module is one flow, that flow gets the full phase 3 treatment; secondary flows abbreviate.

---

## What to write

`FLOWS.md` → enumeration:

```markdown
# <Module name> — Flows

## Flow enumeration

| # | Flow name | Pattern | Entry points | Goal | Priority |
|---|---|---|---|---|---|
| 1 | <name> | linear / branching / save-resume / single-action / continuous | <which entry points> | <one-line goal> | P0 / P1 / P2 |
| 2 | <name> | | | | |

### Pareto: the 80/20

- **Flows getting 80% of polish:** <list of P0 flows>
- **Everything else:** functional, not delightful, by design.

### Dropped candidates (audit trail)

- <flow> — dropped because <reason>

### Cross-module flows (handed off)

If a candidate flow starts here but ends in another module, document the hand-off:

| Flow start | Hand-off point | Continues in | Hand-off contract |
|---|---|---|---|
| Triage → "Reply" | Click "Reply" button | Compose module | URL: `/compose?in_reply_to=<id>` + draft pre-populated |
```

---

## Success criteria for phase 2

- Every entry point from OVERVIEW.md is accounted for (mapped to one or more flows, or noted as "no flow originates here, it's a re-entry for an in-progress flow")
- Every flow is classified by pattern from the five canonical patterns
- Pareto identification matches PRINCIPLES.md — the P0 flows are the ones polish concentrates on
- Cross-module hand-offs are documented with hand-off contracts

---

## What NOT to do in phase 2

- **Don't list features as flows.** "Filtering" is not a flow; the flow that uses filtering is.
- **Don't make a flow per screen.** Multi-step flows traverse screens; the flow is the journey, not the rooms.
- **Don't priorize by feature value to product.** Prioritize by *user time spent in this module*. A flow that the user does 100x daily gets more polish than a flow they do once a quarter, regardless of strategic importance.
- **Don't skip the dropped-candidates list.** The audit trail prevents this enumeration from being re-litigated next time someone reads MODULES.md and asks "why isn't X a flow here?"

---

## Checkpoint

> "Three checks:
> 1. Walk through a typical day for the user — every time they're in this module, which enumerated flow are they in? If there's a time you can't map to a flow, it's missing.
> 2. Are the P0 flows the ones the user actually spends time in, or the ones the team finds most strategically interesting? Pareto says polish time follows user time.
> 3. Any cross-module hand-offs that feel awkward? Awkward hand-offs are a sign the flow boundary is in the wrong place."

Iterate. Lock. Move to phase 3.

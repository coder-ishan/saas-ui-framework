# Phase 11 — Implementation Strategy

**Purpose:** Translate the composite SPEC into an implementation plan tuned to the project's EXECUTION-POSTURE. The same SPEC ships differently under SPRINT, BALANCED, or CRAFT — this phase makes the difference explicit so implementation is fastest under the project's actual constraints.

**Produces:** `IMPLEMENTATION.md` per composite.

**Estimated time:** 10–15 minutes.

---

## Inputs

Required:
- `.saas-ui/EXECUTION-POSTURE.md` (from `saas-ui-bootstrap` phase 10)
- This composite's full SPEC (phases 1–10 output): SPEC, STATES, KEYBOARD, MOTION, EDGE-CASES, LAWS, VARIANTS

If `EXECUTION-POSTURE.md` is missing:
> "EXECUTION-POSTURE.md is not present in .saas-ui/. Run `saas-ui-bootstrap` phase 10 first to commit a posture. Without it, implementation strategy is ungrounded."

Halt.

---

## The five-step strategy

### Step 1 — MVP cut

What's the minimum implementation that delivers this composite's Contract (from phase 1) under the SPRINT posture?

```markdown
## MVP cut (for SPRINT posture)

### States required for v0

From STATES.md, the states that MUST work for the composite to fulfill its contract:

- idle ✓
- loading-initial ✓ (skeleton matching final layout)
- error (recoverable) ✓ (basic copy)
- empty (true-empty) ✓ (per CONSTITUTION Copy → Empty state structure)
- success exit / post-mutation idle ✓

### States deferred to V1

- hover (visual treatment may default to CSS hover; full hover state design deferred)
- focus (use atom's default focus ring; advanced focus contract deferred)
- selected (only if composite supports selection in MVP)
- loading-mutation (only if composite has mutations; else N/A)
- empty (filter-empty) (only if composite supports filtering in MVP)
- error (terminal) (404/permission)
- partial-permission
- offline / stale
- long-content
- active-edit (only if composite supports edit in MVP)

### Actions deferred from Action Inventory

Refer to phase 2's table. For each action, decide MVP / V1 / V2:

| Action | MVP | V1 | V2/CRAFT |
|---|---|---|---|
| <primary action> | ✓ | | |
| <secondary action> | | ✓ | |
| <power-user shortcut> | | | ✓ |

### Keyboard for MVP

- Primary action keyboard shortcut: required
- Tab traversal: required (browser default; advanced focus traversal in V1)
- Escape ladder: minimum (Esc closes; nested unwinding deferred to V1)
- Advanced shortcuts: deferred

### Motion for MVP

- CONSTITUTION canonical patterns: required (modal open, popover, toast)
- Composite-specific overrides from MOTION.md: deferred to V1 unless they fix a perceptual problem

### Copy for MVP

- Shared registry: required
- Composite-specific copy from EMPTY-STATES.md: deferred to V1; defaults from CONSTITUTION suffice
```

### Step 2 — V1 cut

What's the polished V1 of this composite under the BALANCED posture?

```markdown
## V1 cut (for BALANCED posture)

### States required

All 13 canonical states from STATES.md.

### Actions

All actions from phase 2's Action Inventory, primary + secondary modalities.

### Keyboard

Full keyboard map per KEYBOARD.md.
Full focus contract.
Full escape ladder.

### Motion

All CONSTITUTION canonical patterns + composite-specific overrides from MOTION.md.

### Copy

Module-specific copy per EMPTY-STATES.md (when consumed by a module — module skill handles).
Composite's own placeholder copy applies otherwise.

### Edge cases

Every category from EDGE-CASES.md addressed for the in-MVP states.
```

### Step 3 — CRAFT cut

What's the polished, best-in-class version under the CRAFT posture?

```markdown
## CRAFT cut

### Everything from V1, plus:

- Composite-level Laws of UX from LAWS.md fully applied:
  - Fitts verified per anatomy node
  - Miller counts enforced
  - Hick choice budgets enforced
  - Von Restorff: exactly one primary in each region
  - Gestalt laws applied to anatomy layout
  - Prägnanz check: simplest interpretation is the true one

- Motion calibrated:
  - Per-state motion durations tuned (e.g., selection toggle faster than canonical state-change)
  - Composite-specific patterns promoted to CONSTITUTION canonical if reused

- Variants polished:
  - Per-module deltas designed, not just functionally satisfied
  - Cross-variant consistency check passed

- Edge cases:
  - Every category from EDGE-CASES.md addressed
  - Composite-specific edge cases listed and handled
  - Reduced motion, high-contrast, RTL, print all tested

- Performance:
  - Streaming SSR within Doherty budget
  - Virtualization where row count justifies
  - Cache tags tuned per usage pattern

- Accessibility:
  - ARIA roles complete per anatomy node
  - Live region announcements tested with screen reader
  - Keyboard-only walkthrough passes
```

### Step 4 — Build order within the composite

The composite has internal dependencies. Order them:

```markdown
## Build order within composite

The composite implementation has these stages, in order:

1. **Shell + atoms** — the static structure from Anatomy (phase 3). Pure rendering of the composite's outer frame using INVENTORY atoms.

2. **Data fetch + Server / Client boundary** — wire the rendering boundary from Rendering section (phase 7). Server fetch, Suspense, hydration.

3. **Idle + hover + focus states** — the resting states + interaction visuals. Most CSS, minimal JS.

4. **Action handlers (without optimistic UI)** — wire actions to Server Actions; submission works but feels slow.

5. **Optimistic UI** — `useOptimistic` + rollback toast for mutations.

6. **Loading + error states** — skeleton matching layout, error copy per CONSTITUTION.

7. **Empty states** — true-empty and filter-empty per STATES.md.

8. **Keyboard + focus contract** — KEYBOARD.md fully implemented; focus contract live.

9. **Edge cases** — per EDGE-CASES.md, in priority order: data shape > concurrency > permission > browser > a11y.

10. **Motion polish** — composite-specific overrides from MOTION.md.

11. **Variants** — per VARIANTS.md, instantiated for each consuming module.

Stop at:
- Stage 7 for **SPRINT** posture (covers Contract)
- Stage 10 for **BALANCED** posture (covers V1)
- Stage 11 for **CRAFT** posture (covers full SPEC)
```

### Step 5 — Parallelization hints

Within the composite, what can be built in parallel?

```markdown
## Parallelization

| Sub-stream | Stages | Owner pattern | Depends on |
|---|---|---|---|
| A — atoms + shell | 1 | frontend | INVENTORY atoms present |
| B — data + Server Actions | 2, 4, 5 | backend or full-stack | data layer ready |
| C — keyboard + a11y | 8 | accessibility-leaning | A at 70% |
| D — copy + states | 6, 7 | designer/PM | EMPTY-STATES.md (from module skill or CONSTITUTION) |
| E — motion + variants | 10, 11 | design-engineer | A, B, C, D at 90% |

**Recommended parallelization for this posture:**
- SPRINT: A + B in parallel; D combined with A; skip C beyond defaults and E entirely
- BALANCED: A + B + D in parallel; C in week 2; E at end
- CRAFT: all five streams; E gets dedicated polish budget
```

### Step 6 — Defer list & test priority

```markdown
## Defer list

For posture <current>, the following are explicitly deferred:

| Item | Reason for defer | Re-consider at |
|---|---|---|
| <state or action> | <reason> | <posture upgrade> |

## Test priority

Tests required for ship at this posture:

- **MVP/SPRINT:** smoke test of Contract — does the composite render and complete its primary action without error?
- **V1/BALANCED:** state coverage — every required state renders correctly; edge cases for data-shape addressed.
- **CRAFT:** full state coverage + keyboard walkthrough + screen-reader walkthrough + edge case suite + motion correctness under reduced-motion.

Test files to write for this posture: <list with priority>.

## Stub strategy

For deferred states/actions, how to scaffold so they don't ship as broken:

- Deferred state: stub component renders empty-with-tooltip "Available in <next iteration>"
- Deferred action: omit affordance entirely (don't render disabled button without explanation — that violates Forbidden Pattern #10)
- Deferred variant: that module either doesn't consume this composite yet, or consumes the canonical SPEC without variant deltas
```

---

## What to write

`IMPLEMENTATION.md`:

```markdown
# <Composite name> — Implementation Strategy

**Posture:** <from EXECUTION-POSTURE.md>
**Last updated:** <YYYY-MM-DD>

## MVP cut (SPRINT)

<from Step 1>

## V1 cut (BALANCED)

<from Step 2>

## CRAFT cut

<from Step 3>

## Build order within composite

<from Step 4 — with stop-at line for this posture>

## Parallelization

<from Step 5>

## Defer list

<from Step 6>

## Test priority

<from Step 6>

## Stub strategy

<from Step 6>

## Re-evaluation triggers

This IMPLEMENTATION.md is recomputed when:
- EXECUTION-POSTURE.md changes
- SPEC.md, STATES.md, or LAWS.md changes
- A consuming module's BUILD-ORDER.md surfaces a new variant need
```

---

## Posture-shaped recommendations

The skill outputs different recommendations based on posture:

### SPRINT

- **States 5 only** (idle, loading-initial, error-recoverable, empty-true, success exit).
- **Actions: primary only.** No secondary modalities (keyboard advanced shortcuts, command-palette entry deferred).
- **Motion: CONSTITUTION canonical only.** No composite-specific tuning.
- **Variants: none — use canonical SPEC.** Per-module deltas deferred.
- **Test: smoke only.** Contract works.

### BALANCED

- **All 13 canonical states.**
- **All actions per Action Inventory, with primary + at least one secondary modality each.**
- **Motion: CONSTITUTION canonical + any composite-specific overrides that fix perceptual problems.**
- **Variants: for consuming modules in scope per their BUILD-ORDER.md.**
- **Test: state coverage + data-shape edge cases.**

### CRAFT

- **Everything above, plus:**
- **All 11 composite-level Laws verified.**
- **Motion fully tuned per MOTION.md including composite-specific patterns.**
- **All variants polished and consistency-checked.**
- **Edge cases: every category from EDGE-CASES.md.**
- **Test: state + keyboard + screen reader + reduced-motion + RTL + print suite.**

---

## What NOT to do in phase 11

- **Don't ship MVP cut promising V1 quality.** Be honest about the cut. The downstream module BUILD-ORDER.md will plan accordingly; over-promising here corrupts module-level scope.
- **Don't generate fake estimates.** Stages have order, not durations. "Estimated stage 1 duration: 3 days" is false precision unless you're calibrated. Calendar mathematics is downstream.
- **Don't add stages not in the canonical 11.** If the composite needs a stage not listed, your SPEC is missing something — fix it upstream, not here.
- **Don't skip stub strategy.** Deferred surfaces that ship as broken disabled buttons are worse than absent ones.

---

## Checkpoint

> "Three checks:
> 1. Does the MVP cut deliver the composite's Contract from phase 1? If a Contract item requires a state not in MVP, either move the state up or weaken the Contract.
> 2. Is the build-order stop-at line right for this posture? SPRINT stops at stage 7 (no motion polish, no variants); BALANCED stops at stage 10. If you're tempted to push further, you're drifting toward a different posture.
> 3. Defer list — is anything on it that the composite's consuming modules will need? Cross-check with module BUILD-ORDER.md if available."

Iterate. Lock. Phase 12 (hand-off) updates STATE.md and surfaces this IMPLEMENTATION.md to downstream module skills.

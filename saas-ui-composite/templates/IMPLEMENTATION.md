# <Composite name> — Implementation Strategy

**Posture:** <SPRINT / BALANCED / CRAFT — from EXECUTION-POSTURE.md>
**Last updated:** <YYYY-MM-DD>

---

## MVP cut (SPRINT)

### States required for v0
- idle ✓
- loading-initial ✓
- error (recoverable) ✓
- empty (true-empty) ✓
- success exit / post-mutation idle ✓

### States deferred
- ...

### Actions deferred

| Action | MVP | V1 | V2/CRAFT |
|---|---|---|---|
| | | | |

### Keyboard for MVP
- ...

### Motion for MVP
- CONSTITUTION canonical patterns only

### Copy for MVP
- Shared registry only

---

## V1 cut (BALANCED)

### States required
- All 13 canonical states

### Actions
- All from Action Inventory, primary + secondary modalities

### Keyboard
- Full keyboard map per KEYBOARD.md

### Motion
- CONSTITUTION canonical + composite-specific overrides from MOTION.md

### Copy
- Module-specific per EMPTY-STATES.md

### Edge cases
- Every category from EDGE-CASES.md for in-MVP states

---

## CRAFT cut

### Everything from V1, plus:
- All composite-level Laws applied per LAWS.md
- Motion calibrated
- Variants polished + consistency check passed
- All edge cases addressed including a11y / RTL / print
- Performance tuned (streaming, virtualization, cache)
- Full ARIA + live regions tested with screen reader

---

## Build order within composite

1. Shell + atoms
2. Data fetch + Server / Client boundary
3. Idle + hover + focus states
4. Action handlers (without optimistic UI)
5. Optimistic UI
6. Loading + error states
7. Empty states
8. Keyboard + focus contract
9. Edge cases (data shape > concurrency > permission > browser > a11y)
10. Motion polish
11. Variants

**Stop at stage:** <7 for SPRINT | 10 for BALANCED | 11 for CRAFT>

---

## Parallelization

| Sub-stream | Stages | Owner | Depends on |
|---|---|---|---|
| A — atoms + shell | 1 | frontend | INVENTORY atoms |
| B — data + actions | 2, 4, 5 | backend | data layer |
| C — keyboard + a11y | 8 | a11y | A at 70% |
| D — copy + states | 6, 7 | designer | EMPTY-STATES.md |
| E — motion + variants | 10, 11 | design-engineer | A–D at 90% |

**Recommended for this posture:** ...

---

## Defer list

| Item | Reason | Re-consider at |
|---|---|---|

---

## Test priority

- ...

---

## Stub strategy

- Deferred state: ...
- Deferred action: omit affordance (don't render disabled-without-explanation)
- Deferred variant: ...

---

## Re-evaluation triggers

- EXECUTION-POSTURE.md changes
- SPEC.md, STATES.md, or LAWS.md changes
- A consuming module's BUILD-ORDER.md surfaces a new variant need

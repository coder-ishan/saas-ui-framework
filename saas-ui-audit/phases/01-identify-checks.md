# Phase 1 — Target identification & check enumeration

**Purpose:** Determine which checks apply to this target. Different artifacts get different check sets. Project-wide audits run a reduced cross-artifact set.

**Produces:** In-memory list of checks to run in phase 2 (and 3 for manual ones).

---

## Check categories

Reference: `references/audit-rules.md` for the full rule catalog. Each rule is tagged with applicability:

- **functional** — always applies; always strict (atom invention, duplicate buttons, generic empty states)
- **constitution** — per-CONSTITUTION rule; strictness scales with posture
- **principle** — per-PRINCIPLES law; strictness scales
- **composite-law** — composite-level Laws of UX; scales
- **module-law** — module-level Laws of UX; scales
- **pathology** — the 17 LLM pathologies; always strict
- **drift** — cross-artifact consistency; always reported, severity scales

---

## Per-target check sets

### Target: `composite/<name>`

Run checks (artifact mode):

1. **Functional set (always):**
   - Atom invention sweep (every atom referenced in SPEC.md → Anatomy / Atoms in use must exist in INVENTORY.md by exact name)
   - Action Inventory contract (every action has exactly one primary affordance per region; duplicates have `Argue:` lines)
   - Primitive Gaps resolution (every gap has Resolution: Add / Defer / Substitute)
   - State enumeration completeness (all 13 canonical states addressed in STATES.md; N/A entries justified)

2. **CONSTITUTION set:**
   - Motion overrides have Reason: lines
   - Density adherence (composite uses CONSTITUTION grid + scale)
   - Copy adherence (every user-visible string follows CONSTITUTION → Copy structure)
   - State Model compliance (loading-mutation has rollback toast; no full-screen spinners; error copy structure)
   - Forbidden Patterns check (each of the 17 patterns NOT present)

3. **Composite-level Laws (LAWS.md presence + verification):**
   - All 11 laws have entries
   - Each verification line is concrete

4. **Drift:**
   - SPEC's Atom references match INVENTORY exactly
   - SPEC's Motion references match CONSTITUTION canonical patterns
   - VARIANTS.md exists if Type = canonical

### Target: `module/<name>`

Run checks (artifact mode):

1. **Functional set (always):**
   - Composite references in FLOWS.md exist in COMPOSITES.md
   - Composite states referenced in FLOWS exist in those composites' STATES.md
   - Empty-state copy follows CONSTITUTION → Copy → Empty state structure
   - No generic "No data" (Pathology #5)

2. **CONSTITUTION set:**
   - Motion (any module-specific motion follows CONSTITUTION or has override Reason:)
   - Copy compliance
   - State Model compliance

3. **Module-level Laws (LAWS.md presence + verification):**
   - All 6 laws have entries (Peak-End, Goal-Gradient, Zeigarnik, Serial Position, Flow, Selective Attention)
   - Peak-End identifies a peak + end per primary flow
   - Multi-step flows have progress indicators specified
   - Save-and-resume flows have persistence contracts

4. **BUILD-ORDER.md presence and posture-match:**
   - BUILD-ORDER.md exists
   - Posture in BUILD-ORDER matches EXECUTION-POSTURE.md
   - Per-flow cuts are explicit

### Target: `code:<path>` (post-implementation)

Run checks (code mode):

1. **Action Inventory contract in code:**
   - Every action from the corresponding SPEC's Action Inventory has exactly one implementation site for its primary affordance
   - No duplicate event handlers wired to two affordances in the same region
   - Keyboard shortcut implementations match KEYBOARD.md

2. **State coverage in code:**
   - Every required state from posture-applied IMPLEMENTATION.md has a render branch
   - Loading-initial uses Suspense + skeleton (not isLoading spinner)
   - Empty states match EMPTY-STATES.md copy (or shared registry)
   - Error states match CONSTITUTION → State Model structure

3. **Atom usage:**
   - Imports come from the inventory atom path (no inline custom buttons)
   - Atom prop usage matches INVENTORY (no unknown variants)

4. **Next.js boundary:**
   - Server / Client Component boundary matches SPEC.md → Rendering
   - 'use client' is on the smallest leaf, not the whole composite
   - Suspense boundaries present per SPEC

5. **Pathology detection in code (full 17):**
   - Run per `references/pathology-detection.md`

### Target: project-wide

Reduced check set across all artifacts:

1. **Atom invention sweep across all composites** (most common drift)
2. **Action Inventory consistency** (same action labeled differently across composites)
3. **Posture freshness** (warn if posture is stale)
4. **Pathology #5 sweep** (generic empty states project-wide)
5. **Cross-variant consistency** (per VARIANTS.md cross-checks)

---

## Project-wide check budget

Project-wide audits can grow expensive on large projects. The audit caps at:
- 50 artifacts examined per run (top by tier: anchor first, then occasional, then rare)
- 1000 checks total

If the cap is hit, the scorecard notes "Partial audit — N artifacts examined; rerun with `--scope=<area>` for full coverage."

---

## Identifying checks per artifact target — output

The phase produces an in-memory list:

```text
checks_to_run = [
  { id: "atom-invention", category: "functional", target: "composite/data-table", strictness: "always" },
  { id: "action-inventory-contract", category: "functional", target: "composite/data-table", strictness: "always" },
  { id: "state-enumeration", category: "functional", target: "composite/data-table", strictness: "always" },
  { id: "motion-override-reason", category: "constitution", target: "composite/data-table", strictness: "posture-scaled" },
  { id: "composite-law-fitts", category: "composite-law", target: "composite/data-table", strictness: "posture-scaled" },
  ...
]
```

Each check has:
- `id` — stable identifier
- `category` — for posture scaling and severity
- `target` — what it's checking
- `strictness` — `always` or `posture-scaled`

Phase 2 runs `strictness: always` strictly; phase 4 adjusts `posture-scaled` based on posture.

---

## What NOT to do in phase 1

- **Don't run checks here.** This is enumeration only.
- **Don't skip checks based on assumed posture.** All applicable checks enumerate; scaling happens in phase 4.
- **Don't accept user-supplied check skips.** If the user wants to skip a check, that's an `Argue:` line in the artifact itself, not a runtime skip.

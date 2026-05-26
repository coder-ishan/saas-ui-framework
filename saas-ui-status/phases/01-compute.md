# Phase 1 — Compute derived status

**Purpose:** Translate the loaded anchors into a single in-memory status object that phase 2 will render. This phase is pure computation — no output yet.

**Produces:** In-memory status structure (used by phase 2).

---

## The status structure

```text
status = {
  bootstrap: {
    started: <date or null>,
    complete: <bool>,
    current_phase: <0–10 | done>,
    phases_complete: { 0: bool, 1: bool, ..., 10: bool },
    days_since_last_update: <int>,
  },
  posture: {
    committed: <bool>,
    name: <SPRINT | BALANCED | CRAFT | null>,
    committed_on: <date or null>,
    days_since_commit: <int>,
    re_evaluation_trigger: <text>,
    is_stale: <bool — true if days_since_commit > posture-specific threshold>,
  },
  inventory: {
    present: <bool>,
    atom_count: <int>,
    molecule_count: <int>,
    gap_count: <int>,
    p0_gaps: <int — gaps blocking composites>,
  },
  composites: [
    {
      name: <string>,
      type: <canonical | single-module | bespoke>,
      tier_used_in: <anchor | occasional | rare — highest tier of using modules>,
      catalog_status: <in COMPOSITES.md | missing from catalog>,
      spec_status: <not-started | in-progress (phase N) | spec'd | implementation-strategy-done>,
      blocking_modules: <int — count of modules waiting on this>,
      atom_gaps: <list of unresolved primitive gaps>,
      drift_flag: <list of detected drift, e.g., "SPEC exists but catalog lists as unspec'd">,
    }, ...
  ],
  modules: [
    {
      name: <string>,
      tier: <anchor | occasional | rare>,
      catalog_status: <in MODULES.md | missing from catalog>,
      design_status: <not-started | in-progress (phase N) | designed | build-order-done>,
      blocking_composites: <list of composites this module needs that aren't spec'd>,
      drift_flag: <list of detected drift>,
    }, ...
  ],
  next_actions: [
    { priority: 1, action: <string>, reason: <string>, command: <slash command> },
    ...
  ],
  warnings: [<list of non-blocking issues>],
  errors: [<list of blocking issues>],
}
```

---

## The eight computations

### 1. Bootstrap progress

From STATE.md:
- Set `bootstrap.started`, `bootstrap.complete`, `bootstrap.current_phase`
- For phases 0–10, set `phases_complete[N]`
- Compute `days_since_last_update` from `last_updated` field

If bootstrap is incomplete:
- `next_actions` gets a P0 entry: "Resume bootstrap at phase N" with command `/saas-ui-bootstrap`

### 2. Posture freshness

From EXECUTION-POSTURE.md:
- Set `posture.committed`, `posture.name`, `posture.committed_on`
- Compute `days_since_commit`
- Set `is_stale` per posture:
  - SPRINT: stale after 30 days (sprint horizons shouldn't last that long without re-commitment)
  - BALANCED: stale after 90 days
  - CRAFT: stale after 180 days

If `is_stale`:
- `warnings` gets: "Posture committed N days ago. Re-evaluation trigger says <text>. Consider re-running bootstrap phase 10."

### 3. Inventory health

From INVENTORY.md:
- Count atoms, molecules
- Count gaps from Gaps observed table
- Count P0 gaps (gaps with priority P0)

If `p0_gaps > 0` and any of them blocks a composite the user might want to spec next, surface as a P1 next-action.

### 4. Composite progress

For each composite in COMPOSITES.md:
- Match against globbed `.saas-ui/composites/<name>/SPEC.md`
- If SPEC.md exists: `spec_status = spec'd`
- If SPEC.md + IMPLEMENTATION.md exist: `spec_status = implementation-strategy-done`
- If neither exists: `spec_status = not-started`
- If STATE.md shows in-progress at phase N: `spec_status = in-progress (phase N)`

For each composite, compute `blocking_modules`:
- Read MODULES.md, count modules in `Likely composites used` referencing this composite that are not yet `designed`

Detect drift:
- If SPEC.md exists but STATE.md claims `not-started` → `drift_flag` += "SPEC exists but state shows unspec'd"
- If STATE.md claims `spec'd` but SPEC.md missing → `drift_flag` += "State claims spec'd but file missing" (do not auto-heal; surface to user)

### 5. Module progress

For each module in MODULES.md:
- Match against globbed `.saas-ui/modules/<name>/OVERVIEW.md`
- If OVERVIEW.md exists: at least pre-flight passed
- If FLOWS.md exists: in design (phase 2+)
- If BUILD-ORDER.md exists: `design_status = build-order-done` (full design)
- Otherwise: `design_status = not-started`

For each module, compute `blocking_composites`:
- Read module's `Likely composites used` from MODULES.md
- For each, check `composites[i].spec_status` — if not `spec'd` or better, it's a blocker
- `blocking_composites = <list of those composite names>`

If module has no blockers and is `not-started`: ready to design. Add to `next_actions`.

If module has blockers: not ready. Add to `warnings` if it's an anchor.

### 6. Atom gaps blocking composites

For each gap in INVENTORY.md:
- Read `Blocks composites` field
- For each blocked composite, if that composite is queued for SPEC: gap is on critical path
- Surface in `next_actions` as: "Add atom `<name>` to design system — blocks <list of composites>"

### 7. Drift detection

Compile all `drift_flag` entries from composites and modules. If any:
- `warnings` includes them
- If drift is safe-direction (file exists but state claims not-started): note that auto-heal is available; phase 3 may offer it

### 8. Next-action prioritization

Reference: `references/next-action-rules.md`.

Compose a prioritized list. The first action is the recommended single thing to do next. The list goes 3–5 deep so the user has alternatives.

Priority order (varies by posture; reference doc has full rules):

For SPRINT:
1. Spec the highest-leverage unspec'd P0 composite (anchor-blocking, no atom gaps)
2. Design the highest-leverage ready-to-design anchor module
3. Resolve a P0 atom gap if it unblocks 2+ composites

For BALANCED:
1. Spec the next composite in COMPOSITES.md build order
2. Design the next module in MODULES.md build order
3. Address drift warnings

For CRAFT:
1. Polish the most-used anchor composite (re-run phase 11 or improve LAWS implementation)
2. Polish the dominant anchor module's Peak-End
3. Address every warning

If bootstrap is incomplete: P0 action is always "finish bootstrap." Posture-based recommendations come after.

---

## What NOT to do in phase 1

- **Don't write any files.** Computation is in-memory.
- **Don't make assumptions about posture if EXECUTION-POSTURE.md is missing.** Mark posture as `not-committed`; downstream recommendations adapt.
- **Don't double-count drift.** If a composite has drift AND a module depends on it, list the issue once per affected scope.
- **Don't surface trivia.** The report has limited space; only surface what affects next-action priority.

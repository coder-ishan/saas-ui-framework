# Status checks — what each check measures and how to interpret it

> Reference for phase 1 (compute) — what to look at, what the values mean.

---

## Bootstrap checks

### `bootstrap.complete`

True only if all 11 phases (0–10) are marked complete in STATE.md.

A bootstrap can be "almost complete" (10 of 11 phases done) but still gate downstream skills if phase 10 (Execution Posture) hasn't run — downstream skills halt without EXECUTION-POSTURE.md.

### `bootstrap.days_since_last_update`

Distance between today and the `last_updated` field in STATE.md.

Interpretations:
- 0–3 days: active work
- 4–14 days: project in flight but slowing
- 15–60 days: cold; user is returning
- 60+ days: stale; user should re-read CONSTITUTION before continuing

If > 60 days, status report includes a gentle nudge: "Project hasn't been touched in N days. Review CONSTITUTION + PRINCIPLES before resuming?"

---

## Posture checks

### `posture.is_stale`

Posture-specific thresholds:
- SPRINT: stale after **30 days**. SPRINT means "weeks horizon" — 30+ days without re-evaluation is a sign the posture should have upgraded.
- BALANCED: stale after **90 days**. BALANCED means "months horizon" — 90 days marks a quarter; reassess.
- CRAFT: stale after **180 days**. CRAFT is the slowest-changing posture, but half a year still merits reassessment.

### `posture.re_evaluation_trigger`

From EXECUTION-POSTURE.md. May be a date ("after demo on Mar 14"), an event ("after first 10 paying customers"), or a fuzzy interval ("quarterly").

Status surfaces this verbatim. If the trigger references a past date, mark high-priority.

---

## Inventory checks

### Atom / molecule counts

Counted from INVENTORY.md → Atoms (available) and Molecules (available) sections.

Sanity check: an enterprise SaaS UI typically needs 30–60 atoms and 10–25 molecules. If a project has < 15 atoms, the inventory is probably incomplete.

### Gaps

Count from INVENTORY.md → Gaps observed table.

P0 gaps (blocking composites) are the highest priority. Status lists them with which composites each blocks.

### Composite-blocking gap impact

For each P0 gap, count how many composites the gap blocks. A gap blocking 1 composite is low-impact; a gap blocking 5 composites is on the critical path.

Status surfaces top 3 highest-impact gaps.

---

## Composite checks

### Spec status

Detected from filesystem (presence of files), validated against STATE.md (claimed state). The order of authority is **filesystem first** — if a SPEC.md exists, the composite is spec'd, regardless of STATE.md.

Drift detection happens when filesystem and STATE.md disagree.

### Blocking modules count

For each composite, the count of modules in MODULES.md that:
1. List this composite in their `Likely composites used`
2. Are not yet `designed` themselves

This is the leverage metric — high-leverage composites unblock many module designs.

### Atom gap impact

For each composite, list the unresolved primitive gaps from INVENTORY.md. If any gaps exist, composite is `blocked` until they're resolved (or substituted, or the composite is deferred).

---

## Module checks

### Design status

Detected from filesystem:
- OVERVIEW.md exists → at least phase 1 done
- FLOWS.md exists → at least phase 3 done
- LAWS.md exists → at least phase 7 done
- BUILD-ORDER.md exists → full design done (phase 11 reached)

### Blocking composites

For each module, list composites it `Likely composites used` that are not `spec'd` or better. If any, module is `blocked`.

A module with 0 blocking composites is **ready to design** — high-priority next-action candidate.

---

## Drift detection

Drift cases the status detects:

### Safe drift (auto-healable)

1. **Composite SPEC exists but STATE.md shows not-started.** Filesystem is authoritative; update STATE.md to spec'd.
2. **Module BUILD-ORDER.md exists but STATE.md shows not-started.** Same pattern.

### Unsafe drift (surface only)

1. **STATE.md claims spec'd but SPEC.md missing.** Data loss risk. Surface as error.
2. **COMPOSITES.md type field disagrees with SPEC.md type.** Surface as conflict.
3. **EXECUTION-POSTURE.md changed but IMPLEMENTATION.md files weren't recomputed.** Surface as warning — user should re-run composite phase 11.
4. **A module's `Likely composites used` references a composite not in COMPOSITES.md.** Catalog inconsistency.

---

## Health scoring (optional)

The status can compute a single "health" number 0–100:

```text
health = 100 - <penalty>

Penalties:
- 10 for each phase of bootstrap not done
- 5 if posture is stale
- 5 for each P0 gap
- 2 for each drift warning
- 1 for each safe-heal drift

Floor at 0; cap at 100.
```

90+ is healthy. 70–89 is in progress. 50–69 needs attention. <50 means the project state has issues that should be addressed before more design work.

This is *optional* — the report can include it or not. Default: include if health is below 80.

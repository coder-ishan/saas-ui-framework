# Phase 0 — Pre-flight & anchor load

**Purpose:** Confirm `.saas-ui/` exists and load the read-only anchors. Tight phase — most projects pass this in seconds.

**Produces:** No artifact. Validation only.

---

## The two checks

### Check 1 — Project state directory

Does `.saas-ui/` exist in the current working directory?

If not:
> "No saas-ui project here.
>
> This directory doesn't contain a `.saas-ui/` planning directory. To start one, run `/saas-ui-bootstrap` — that's the 11-phase interview that produces the design constitution, module catalog, composite catalog, and execution posture for a new SaaS UI project.
>
> If you expected `.saas-ui/` to exist, you may be in the wrong directory. Try `cd` to your project root."

Halt.

### Check 2 — Anchor load

Load these files (note absence rather than halting; bootstrap may be partial):

| File | Required? | If missing |
|---|---|---|
| `.saas-ui/STATE.md` | Yes | Halt with "STATE.md is missing — bootstrap is corrupted. Investigate or restart bootstrap." |
| `.saas-ui/EXECUTION-POSTURE.md` | Recommended | Warn: "No posture committed yet. Bootstrap phase 10 will create this. Status report continues but downstream skills can't run." |
| `.saas-ui/CONSTITUTION.md` | Recommended | Warn: "No CONSTITUTION. Bootstrap is incomplete." |
| `.saas-ui/PRINCIPLES.md` | Recommended | Warn |
| `.saas-ui/INVENTORY.md` | Recommended | Warn |
| `.saas-ui/MODULES.md` | Recommended | Warn |
| `.saas-ui/COMPOSITES.md` | Recommended | Warn |

Also glob:

- `.saas-ui/composites/*/SPEC.md` → list of spec'd composites (actual)
- `.saas-ui/composites/*/IMPLEMENTATION.md` → list of composites with implementation strategy
- `.saas-ui/modules/*/OVERVIEW.md` → list of modules in design
- `.saas-ui/modules/*/BUILD-ORDER.md` → list of modules with build order

These globs are the *actual* state. STATE.md is the *claimed* state. The drift between them is what phase 1 surfaces.

---

## Success criteria

- `.saas-ui/` exists
- STATE.md loaded
- All other anchors either loaded or noted as absent
- Globs returned the actual lists of spec'd composites and designed modules

---

## What NOT to do in phase 0

- **Don't halt on missing optional anchors.** Note them as warnings; phase 2's report will show the missing pieces.
- **Don't fix anything yet.** Phase 1 detects drift; phase 2 reports it. Auto-healing happens after the user sees the report.

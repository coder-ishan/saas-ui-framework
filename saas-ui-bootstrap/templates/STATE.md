# .saas-ui State

> This file is maintained by the saas-ui-bootstrap skill and consumed by all downstream saas-ui-* skills.
> Do not edit manually unless you know what you're doing — phases re-key off the structure here.

## Bootstrap status

- bootstrap_started: <YYYY-MM-DD HH:MM>
- bootstrap_complete: <true | false>
- last_updated: <YYYY-MM-DD HH:MM>
- current_phase: <0–10 | done>
- execution_posture: <SPRINT | BALANCED | CRAFT | not-yet-committed>

## Phase completion

- phase_0_design_system_complete: <true | false>
- phase_1_product_complete: <true | false>
- phase_2_modules_complete: <true | false>
- phase_3_composites_complete: <true | false>
- phase_4_motion_complete: <true | false>
- phase_5_density_complete: <true | false>
- phase_6_copy_complete: <true | false>
- phase_7_state_complete: <true | false>
- phase_8_forbidden_complete: <true | false>
- phase_9_references_complete: <true | false>
- phase_10_execution_posture_complete: <true | false>

## Artifacts produced

- CONSTITUTION.md: <not started | in-progress (phase N section) | complete>
- PRINCIPLES.md: <not started | partial (N of 8 laws) | complete>
- INVENTORY.md: <not started | partial | complete>
- MODULES.md: <not started | partial | complete>
- COMPOSITES.md: <not started | partial | complete>
- EXECUTION-POSTURE.md: <not started | complete (posture: <name>)>
- references/: <empty | <N> references captured>

## Next skills available

(Populated once bootstrap_complete: true)

- saas-ui-module: <list of modules with status — pending | in-progress (phase N) | designed | implemented>
- saas-ui-composite: <list of composites with status — pending | in-progress (phase N) | spec'd | implemented>
- saas-ui-status: ready
- saas-ui-audit: ready

## Posture-driven artifacts (downstream)

When `saas-ui-composite` runs against a composite, it produces:
- `composites/<name>/IMPLEMENTATION.md` — MVP/V1/CRAFT cuts per current EXECUTION-POSTURE

When `saas-ui-module` runs against a module, it produces:
- `modules/<name>/BUILD-ORDER.md` — flow priority cuts, critical path, parallel streams per current EXECUTION-POSTURE

If EXECUTION-POSTURE is re-committed (project phase changes), these downstream artifacts should be re-run.

## Notes

<free-text notes from the bootstrap process; user concerns, deferred decisions, etc.>

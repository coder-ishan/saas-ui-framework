# Phase 11 — Checkpoint & hand-off

**Purpose:** Lock the module's artifacts, update project state, propagate any cross-cutting changes, and surface the next action.

**Produces:** Updates to `.saas-ui/STATE.md`. Cross-references to other artifacts as needed.

**Estimated time:** 5 minutes.

---

## The four hand-off actions

### Action 1 — Update STATE.md

In `.saas-ui/STATE.md`, find the modules section. Add or update this module's entry:

```markdown
| Module | Status | Tier | Posture-applied scope | Last updated |
|---|---|---|---|---|
| <module-name> | designed | <tier> | <SPRINT/BALANCED/CRAFT> | <YYYY-MM-DD> |
```

Status progression for modules:
- `not-started` → `pre-flight passed` → `in-design` (during phases 1–10) → `designed` (phase 11 complete) → `implemented` (after code ships per BUILD-ORDER.md)

### Action 2 — Propagate to composites

If this module's phase 9 created or updated composite VARIANTS.md sections, surface that:

```markdown
## Cross-composite updates triggered

In this module design, the following composite artifacts were updated:
- `composites/<name>/VARIANTS.md` — added/updated `<this module>` variant
- `composites/<name>/VARIANTS.md` — added/updated `<this module>` variant

If `saas-ui-audit` runs against composites, expect those VARIANTS sections to be validated.
```

### Action 3 — Unblock downstream modules

Read MODULES.md → Module dependency notes. Any module that depended on this one being designed is now unblocked:

```markdown
## Unblocked modules

- <module> — depended on this module's <flow / contract / hand-off>; now ready for `saas-ui-module`.
```

### Action 4 — Surface SPEC gaps discovered

If any phase discovered a composite SPEC gap or CONSTITUTION ambiguity:

```markdown
## Discovered gaps

- **Composite gap:** `<composite>` SPEC needs <change> because <module-level reason>. Recommend re-running `saas-ui-composite <name>` after this module ships, or update the SPEC directly if the change is minor.
- **CONSTITUTION ambiguity:** <e.g., "CONSTITUTION → Motion → Modal canonical pattern doesn't specify behavior for nested drawers; we used <decision> here. Recommend updating CONSTITUTION to make this canonical or argue the deviation in this module.">
- **INVENTORY atom gap:** <e.g., "Empty-state illustration variant for filter-empty isn't in INVENTORY. Recommend adding to INVENTORY.md → Gaps observed.">
```

These gaps are *not* fixed here — they're surfaced for the next bootstrap revisit or composite SPEC update.

---

## What to do if a gap blocks ship

If a discovered gap blocks the module from being implementable (per BUILD-ORDER.md → Critical path):

> "Heads-up: this module has a critical-path gap that blocks ship:
> - <gap>
>
> Recommend one of:
> 1. Fix the gap first (run `saas-ui-composite <name>` or revisit bootstrap phase X)
> 2. Down-scope this module to bypass the gap (update BUILD-ORDER.md Stub strategy)
> 3. Accept the gap and stub the affected surface (per BUILD-ORDER.md Stub strategy)"

Don't proceed to "next module" until the gap is handled.

---

## Suggest the next module

From MODULES.md → Build order suggestion, identify the next module to design:

```markdown
## Next

Next module by build order: **<next module name>** (<tier>).

To design it, run:

    /saas-ui-module <next module name>

Or run `saas-ui-status` to see the full project state.

Alternative if a composite SPEC was discovered as missing:
- Run `/saas-ui-composite <composite name>` first to spec the missing composite.
```

---

## Final checkpoint

Show the user a summary:

```markdown
## Module design complete: <module name>

**Artifacts produced:**
- `.saas-ui/modules/<name>/OVERVIEW.md`
- `.saas-ui/modules/<name>/FLOWS.md`
- `.saas-ui/modules/<name>/LAWS.md`
- `.saas-ui/modules/<name>/EMPTY-STATES.md`
- `.saas-ui/modules/<name>/COMPOSITES.md`
- `.saas-ui/modules/<name>/BUILD-ORDER.md`

**Cross-artifacts updated:**
- `.saas-ui/STATE.md`
- <composite VARIANTS.md entries>

**Posture applied:** <SPRINT/BALANCED/CRAFT>
**In-scope flows:** <list>
**Stubbed/deferred:** <list>

**Critical-path estimate (rough):** <range>

**Discovered gaps surfaced:** <count> (see hand-off section in OVERVIEW.md)

**Next module:** <name> (or address gaps first)
```

---

## Hand-off principle

The module artifacts should be self-sufficient. A future Claude session (or human engineer) reading just these files plus the project's CONSTITUTION / PRINCIPLES / INVENTORY / composite SPECs should be able to:

1. Understand what this module is for
2. Implement the in-scope flows in the right order
3. Recognize and respect the deferred/stubbed surfaces
4. Recover the design rationale for any decision (Peak-End choice, posture cut, variant)

If any of these would require a chat with the original designer, the artifacts aren't yet hand-off-ready. Iterate before locking.

---

## Phase 11 complete

The module ships when BUILD-ORDER.md is followed and the resulting code passes `saas-ui-audit`. Designing additional modules is the next module-skill invocation.

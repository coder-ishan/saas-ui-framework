# Composite Catalog — <PRODUCT NAME>

> Composites are reusable, multi-atom UI building blocks (DataTable, CommandPalette, FilterBuilder, DetailDrawer, etc).
> This catalog is derived from cross-module pattern extraction in phase 3 — not from a template.
> Each composite will get its own detailed SPEC via `saas-ui-composite` later.

**Last updated:** <YYYY-MM-DD>

---

## Composite tier definitions

- **Canonical (multi-module):** used in 2+ modules; one SPEC, per-module variants
- **Single-module:** complex enough to spec, used in only one module
- **Bespoke:** unique pattern, often domain-specific (Kanban, Timeline, Pipeline, etc)

---

## Composites

<For each composite discovered:>

### <Composite name>

- **Type:** canonical | single-module | bespoke
- **Used by modules:** <list — empty if single-module>
- **Tier (usage breadth):** anchor (everywhere) | important (2-3 modules) | localized (single)

**One-line purpose:** <what this composite does>

**Behaviors (rough — detailed in `composites/<name>/SPEC.md` by `saas-ui-composite`):**
- <e.g., sortable, filterable, multi-select, inline-edit>

**Atoms required (from INVENTORY.md):**
- <Atom name>: ✓ present | ✗ gap
- ...

**Blocked until:** <list of unbuilt atoms or "not blocked">

**Priority:** P0 (anchor module is broken without it) | P1 (important) | P2 (nice-to-have)

**Cross-module variants expected:**
- <module>: <how it differs in this module>
- <module>: <…>

---

## Build order suggestion

Composites sorted by:
1. P0 first
2. Within P0, the one used in the most anchor modules first
3. Blockers (atom gaps) noted — unbuilt atoms must come first

| Order | Composite | Priority | Blockers |
|---|---|---|---|
| 1 | <composite> | P0 | <atoms> |
| 2 | <composite> | P0 | none |
| 3 | <composite> | P1 | <atoms> |
| ... | ... | ... | ... |

---

## Atom-gap summary

Atoms that block composite work, with which composites they block:

| Missing atom | Blocks composites | Priority |
|---|---|---|
| <atom> | <list> | P0 |
| <atom> | <list> | P0 |

These belong to the design-system track and must be built before the composite SPECs reach implementation phase.

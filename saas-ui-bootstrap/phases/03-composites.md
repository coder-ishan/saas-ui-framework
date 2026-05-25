# Phase 3 — Composites

**Purpose:** Discover the composite UI building blocks this product needs, by extracting cross-module patterns from the module catalog. Composites are the reusable, multi-atom UI patterns (DataTable, CommandPalette, FilterBuilder, DetailDrawer, FormSystem, BulkActionBar, ToastSystem, EmptyState, NavigationShell, etc).

**Produces:** `.saas-ui/COMPOSITES.md`

**Estimated time:** 20–30 min

## Why fully-discovered (no defaults)

Same reason as phase 2. We've all seen the canonical SaaS-composite list — it's tempting to just paste it. But each product needs a *different subset* with *different prominence*, and may need bespoke composites (e.g., a Kanban variant, a Timeline view) that a generic list would miss. This phase forces the user to derive composites from *their* modules.

## Interview protocol

### Primary method: cross-module pattern extraction

For each module from phase 2, ask:
> In <module>, what UI patterns appear? Lists, forms, dialogs, navigation, search, anything specific?

After listing patterns per module, **cross-reference**:
> Looking across modules, what patterns recur in 2+ modules? Those are the composites we'll need to make canonical.

Examples of what to look for (do not name these to the user until they describe them — let them name first):
- Lists with sort/filter/select → **DataTable** composite
- Quick search across entities, action launcher → **CommandPalette** composite
- Filter UIs (multi-predicate, save/restore) → **FilterBuilder** composite
- Side panels with detail views → **DetailDrawer** / **Sheet** composite
- Forms with autosave + validation → **FormSystem** composite
- Multi-select with contextual actions → **BulkActionBar** composite
- App-wide notifications → **ToastSystem** composite
- "Nothing here" moments → **EmptyState** composite (yes, treat this as composite-tier, not atom-tier — empty states are where flow continues)
- Top nav + side nav + breadcrumb → **NavigationShell** composite
- Tabs, Steps/Wizard, Pagination → may be atoms or composites depending on complexity
- Domain-specific patterns user names: Kanban board, Calendar, Timeline, Gantt, Mind-map, Graph view, Diff viewer, Inbox triage → bespoke composites

### Secondary method: module-specific bespoke composites

> Are there UI patterns unique to a single module that are still complex enough to warrant treating as a composite? (For example, a Kanban that only Reports uses, or a Pipeline view only Sales uses.)

These get listed as **single-module composites** in COMPOSITES.md — same depth of SPEC but no cross-module reuse.

### Tertiary method: catch-all probe

> Is there a UI pattern you've seen elsewhere (Linear, ClickUp, Notion, Figma, Airtable, etc.) that you want here but haven't named yet?

Important because users sometimes have a "feature debt" of patterns they've seen and want but haven't surfaced.

## Cross-reference with INVENTORY.md

For each composite candidate, check INVENTORY.md for the atoms it would need. Flag in COMPOSITES.md any composite whose atoms are not yet built — this becomes design-system work that must precede the composite's SPEC.

Example:
> DataTable needs: Table primitive (✓), Checkbox (✓), DropdownMenu (✓), Skeleton (✗ — gap), Popover (✓), virtualization utility (✗ — gap).
> **Blocked until:** Skeleton + virtualization utility exist.

## Cross-reference with MODULES.md

For each composite, list the modules that use it. This becomes the foundation for the future `saas-ui-module`'s VARIANTS.md generation.

## What to write

### COMPOSITES.md

Use `templates/COMPOSITES.md`. For each composite:

```markdown
## <Composite name>

**Type:** canonical (multi-module) | single-module | bespoke
**Used by modules:** <list>
**Tier:** anchor (used everywhere) | important (used in 2-3 modules) | localized (single module)

**One-line purpose:** <what this composite does>

**Behaviors (rough — to be detailed in saas-ui-composite):**
- <e.g., sortable, filterable, multi-select, inline-edit>

**Atoms required (from INVENTORY.md):**
- <Atom name>: ✓ present | ✗ gap
- ...

**Blocked until:** <list of unbuilt atoms or "not blocked">

**Priority:** P0 (without it, anchor module is broken) | P1 (important) | P2 (nice-to-have)

**Cross-module variants expected:**
- <module>: <how it differs in this module>
```

At the bottom of COMPOSITES.md, write a **build order suggestion** — composites sorted by:
1. P0 first
2. Within P0, the one used in the most anchor modules first
3. Blockers (atom gaps) noted

This becomes the natural sequence for invoking `saas-ui-composite` later.

## Success criteria

Phase 3 is complete when:
1. COMPOSITES.md lists every composite the product needs.
2. Each composite has type, used-by-modules, atom requirements, blockers (if any), and priority.
3. The build-order suggestion at the bottom is generated.
4. STATE.md updated: `phase_3_complete: true`, `current_phase: 4`.

## What NOT to do

- Don't paste a canonical composite list. Derive from modules.
- Don't list a composite without naming the modules that use it. Reuse-of-1 is not a composite; it's a module-specific build.
- Don't say "DataTable" without specifying what atoms it needs and what's missing.
- Don't proceed without a build order. The order is the most actionable output of this phase.

## Checkpoint

When phase 3 completes, ask: "Composite catalog is set with build order. Continue to phase 4 (motion), or pause here?"

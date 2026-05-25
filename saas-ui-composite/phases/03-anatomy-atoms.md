# Phase 3 — Anatomy & atoms

**Purpose:** Decompose the composite into its visual anatomy and bind each part to an exact atom in INVENTORY.md. Catch primitive gaps before any state or motion work.

**Produces:** `SPEC.md` sections: Anatomy, Atoms in use, Primitive Gaps.

**Estimated time:** 15 minutes.

---

## The three-step decomposition

### Step 1 — Anatomy tree

Walk the composite top-down. Name every visual zone. Use indentation to show nesting.

Example for DataTable:

```
DataTable
├── Toolbar
│   ├── Left cluster
│   │   ├── Search input
│   │   └── Filter chip group
│   ├── Right cluster
│   │   ├── View switcher
│   │   ├── Columns menu trigger
│   │   ├── Refresh button
│   │   └── Add-row button
│   └── Selection-mode strip (conditional, when selection > 0)
│       ├── Selection count
│       ├── Bulk delete
│       ├── Bulk export
│       └── Clear selection
├── Header row
│   ├── Select-all checkbox cell
│   └── Column header cells
│       ├── Label
│       ├── Sort indicator
│       └── Filter chevron (opens facet)
├── Body
│   ├── Row (idle)
│   │   ├── Select checkbox cell
│   │   └── Data cells
│   ├── Row (hover state — shows row-context menu trigger)
│   ├── Row (selected)
│   ├── Row (focused)
│   └── Empty-state slot (replaces body when no data)
└── Footer
    ├── Row count summary
    └── Pagination controls (only if >50 rows or paginated mode)
```

**Rules for the tree:**
- Every leaf is a *visual thing*. Not a behavior.
- Conditional zones (only-when-X) are noted in the node label.
- States of a single zone (row idle/hover/selected/focused) are written as siblings here because their visual treatment differs; the *behavioral* state machine lives in STATES.md.

### Step 2 — Bind every leaf to an atom

For each leaf, name the atom from INVENTORY.md that implements it. Use exact names.

```markdown
| Anatomy node | Atom (INVENTORY.md) | Notes |
|---|---|---|
| Search input | `Input` (size=md, leading icon: Search) | Inline x to clear is required state |
| Filter chip | `Chip` (variant=filter, removable=true) | — |
| View switcher | `DropdownMenu` + `Button` (ghost) | — |
| Columns menu trigger | `Button` (ghost, icon=Columns) + `DropdownMenu` | — |
| Refresh button | `IconButton` (icon=Refresh, tooltip=true) | — |
| Add-row button | `Button` (primary) | — |
| Selection count | `Text` (variant=label) | — |
| Bulk delete | `Button` (destructive) | — |
| Bulk export | `Button` (secondary) | — |
| Clear selection | `Button` (ghost) | — |
| Select-all checkbox | `Checkbox` (tri-state) | — |
| Column header cell | (composite — built from `Text` + `IconButton` + `DropdownMenu`) | Not its own atom — composed inline |
| Sort indicator | `Icon` (ArrowUp / ArrowDown / inactive) | — |
| Filter chevron | `IconButton` (icon=ChevronDown) | — |
| Row checkbox | `Checkbox` | — |
| Data cell | `Text` (default) OR variant atom per column type | Per-column rendering is part of column config, not this SPEC |
| Empty state | `EmptyState` molecule | Already in INVENTORY |
| Row count summary | `Text` (variant=label) | — |
| Pagination controls | `Pagination` (gap!) | — |
```

### Step 3 — Surface gaps

Any binding marked `(gap!)` is a primitive missing from the design system. List all gaps explicitly with a resolution:

```markdown
## Primitive Gaps

| Missing atom | Needed for | Resolution |
|---|---|---|
| `Pagination` | DataTable footer | **Add to design system** — see DS ticket DS-42 |
| `Skeleton (table-row)` | DataTable loading state | **Add to design system** — needed for STATES.md phase 4 |
| `Virtualization wrapper` | DataTable body for >1000 rows | **Defer** — current contract caps at 1000 rows for first paint, can add virtualization later without breaking SPEC |
```

Each gap MUST be resolved one of three ways:
- **Add to design system: <ticket / PR / next-up>** — primitive is built first, SPEC ships after
- **Defer: <until when, what we lose>** — SPEC ships incomplete; this section captures the deficit
- **Substitute: <atom from inventory> — Argue: <why substitute works>** — atom from INVENTORY stands in; argue must explain why the substitute doesn't degrade the composite

No gap may be left as "TBD."

If gaps were resolved via Substitute, also append the substitution to INVENTORY.md's `Constraints for downstream SPECs` so future composites know it's an accepted pattern.

---

## What to write

In `SPEC.md`:

```markdown
## Anatomy

<the tree, fenced as ``` text ```>

## Atoms in use

<the binding table>

## Primitive Gaps

<the gaps table>

## Composition rules

- <e.g., "Toolbar and Footer use the same horizontal padding (CONSTITUTION → Density → Page outer)">
- <e.g., "Selection-mode strip replaces the right cluster of the Toolbar; do not stack them">
```

---

## Success criteria for phase 3

- Every visual leaf in the Anatomy tree has a bound atom or appears in Primitive Gaps
- Every bound atom name matches INVENTORY.md exactly (no near-name drift like `Button` vs `BaseButton`)
- Every gap has a resolution path
- Composition rules cross-reference CONSTITUTION Density section by name, not by restated values

---

## What NOT to do in phase 3

- **Don't invent atoms inline.** If you find yourself writing "...uses a custom toggle pill that..." — stop. That's a gap.
- **Don't restate atom signatures.** If `Button` already has variants in INVENTORY, don't redescribe them. Reference by name + variant.
- **Don't bind to molecules unless they exist in INVENTORY → Molecules.** If a needed molecule doesn't exist, treat it as a gap or compose inline from atoms (and note the composition).
- **Don't put state-handling here.** "Row hover shows context menu trigger" goes in STATES.md. Anatomy is the *structure*, not the behavior.

---

## Checkpoint

Show the user the tree, the binding table, and the gaps. Ask:

> "Three checks:
> 1. Is the tree complete? Walk through every state and edge from the Action Inventory — every visible thing should map to a node.
> 2. Are any bindings 'close-enough' rather than exact? Those drift over time. Either rename the atom in INVENTORY or change the binding.
> 3. For each gap: are you OK with the resolution? Adding to design system means delaying this SPEC. Deferring means shipping a known deficit. Substituting commits us to that pattern across the product."

Iterate. Lock. Move to phase 4.

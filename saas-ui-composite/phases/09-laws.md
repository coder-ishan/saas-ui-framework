# Phase 9 — Laws of UX application

**Purpose:** Apply composite-level Laws of UX as an objective rubric. The laws aren't decorations — each forces a specific design check that catches a class of UI failure.

**Produces:** `LAWS.md` for this composite.

**Estimated time:** 15 minutes.

---

## The 11 composite-level laws

Reference: `references/composite-laws.md` for full definitions and example applications.

1. **Fitts's Law** — target size + distance
2. **Miller's Law** — 7±2 chunks in working memory
3. **Hick's Law** — choice count slows decision
4. **Von Restorff Effect** — the different one is remembered
5. **Working Memory** — don't ask user to hold state across interactions
6. **Chunking** — group related items
7. **Common Region (Gestalt)** — bounded region = group
8. **Proximity (Gestalt)** — closer = related
9. **Similarity (Gestalt)** — same style = same kind
10. **Uniform Connectedness (Gestalt)** — connected = unified
11. **Prägnanz** — the simplest interpretation wins

---

## Per-law application template

For each law, write:

```markdown
### <law name>

- **Applies to:** <which anatomy nodes / interactions / states this law constrains>
- **Rule for this composite:** <what the law forces here>
- **Anti-pattern this prevents:** <what would happen without the rule>
- **Verification:** <how `saas-ui-audit` can check this is upheld>
```

---

## Example applications (DataTable)

```markdown
### Fitts's Law

- **Applies to:** Toolbar buttons, header sort triggers, row checkboxes, row context-menu trigger, pagination controls.
- **Rule for this composite:**
  - All toolbar buttons ≥ 32px tap target on mouse; ≥ 44px on touch (responsive).
  - Header sort affordance covers the entire header cell, not just the label — increases target area without visual change.
  - Row checkbox cell is full-height clickable, not just the checkbox glyph.
  - Row context-menu trigger appears on hover at consistent position (rightmost cell, 8px from edge) to support flick-to-corner muscle memory.
- **Anti-pattern this prevents:** Tiny chevron-only targets in row right edge that miss-click; selection requiring pixel-perfect aim on the checkbox graphic.
- **Verification:** Audit checks computed pixel sizes against thresholds.

### Miller's Law (7±2)

- **Applies to:** Number of visible columns by default, number of filters in active state, items in row context menu, items in the Columns show/hide menu.
- **Rule for this composite:**
  - Default visible columns: ≤ 7. If schema has more, hide the rest behind "Columns" menu.
  - Active filter chips before "+ <N> more" collapse: ≤ 5.
  - Row context menu items: ≤ 7 first-level; nested actions ("More...") for the rest.
- **Anti-pattern this prevents:** 15-column tables where user can't see anything; chip-bars that wrap to two lines; context menus that scroll.
- **Verification:** Audit reads the column config and counts active toolbar/context menu items.

### Hick's Law

- **Applies to:** Toolbar action count, view switcher menu, column header dropdown menu, bulk-action strip.
- **Rule for this composite:**
  - Toolbar primary actions in idle state: ≤ 5 (Search, Filter, View, Columns, Add).
  - Bulk-action strip: ≤ 4 actions (per CONSTITUTION → Action Affordances → Forbidden: "Same action exposed twice in toolbar / header / footer").
  - View switcher: ≤ 7 saved views before grouping or search-within-views.
- **Anti-pattern this prevents:** Cluttered toolbars that take 3s to scan.
- **Verification:** Audit counts.

### Von Restorff Effect

- **Applies to:** Primary action ("Add row"), destructive action ("Delete" in bulk strip), warning indicators.
- **Rule for this composite:**
  - Exactly one button in the Toolbar uses the `primary` variant — "+ Add row".
  - Bulk delete uses `destructive` variant; nothing else in the bulk strip does.
  - Status anomalies (overdue, blocked, errored rows) use both color AND icon AND weight (per CONSTITUTION → Forbidden Patterns #8).
- **Anti-pattern this prevents:** Toolbars with three "primary" buttons (none feel primary); destructive-styled actions that aren't actually destructive (boy-who-cried-wolf).
- **Verification:** Audit reads variant usage by region.

### Working Memory

- **Applies to:** Multi-step flows (filter builder, bulk-edit modal), edit drawer with multiple fields, save-view flow.
- **Rule for this composite:**
  - When opening row detail / edit drawer, persist row identifier in the drawer header — user shouldn't lose track of which row they're editing.
  - During bulk action, the strip shows the selection count persistently — user shouldn't have to remember "I had 12 selected."
  - Filter builder shows current filter expression in plain text below the controls — user shouldn't have to mentally compile filter logic.
- **Anti-pattern this prevents:** "Which row am I editing?", "How many did I select?", "What does this filter actually do?"
- **Verification:** Audit checks for persistent context indicators in identified flows.

### Chunking

- **Applies to:** Toolbar grouping (Left/Right clusters), header row vs body, footer vs body.
- **Rule for this composite:**
  - Toolbar has explicit Left cluster (search + filter — scoping) and Right cluster (view + columns + refresh + add — control). Visual gap (CONSTITUTION → Density → Section spacing) separates clusters.
  - Header row is visually distinct from body via different background tint (CONSTITUTION → Density → Hierarchy mechanism: tints).
  - Footer is separated by a divider (CONSTITUTION → Density → Hierarchy mechanism: dividers).
- **Anti-pattern this prevents:** Toolbar items in a single uniform row where the user can't tell what's a scoping action vs a control action.
- **Verification:** Audit checks for gap/divider per Density rules.

### Common Region (Gestalt)

- **Applies to:** Toolbar bounding, Body bounding, Footer bounding, Selection-mode strip.
- **Rule for this composite:**
  - Composite has a single bounding container — bordered or shadowed per CONSTITUTION.
  - Selection-mode strip, when active, has its own region treatment to signal "this region behaves differently now."
  - Filter chip group has its own region (rounded surround or tint) so chips read as related.
- **Anti-pattern this prevents:** Floating chips that look like top-level actions; row detail drawer that visually merges with the table.
- **Verification:** Audit checks for explicit region treatment on identified nodes.

### Proximity (Gestalt)

- **Applies to:** Toolbar cluster spacing, label-input pairs in edit drawer, row inner-cell spacing.
- **Rule for this composite:**
  - Within-cluster spacing < between-cluster spacing. Specifically: items within Left cluster: 8px; gap between Left and Right clusters: 24px (per CONSTITUTION → Density → Form group spacing).
  - Inline-edit cell: input is flush within cell bounds, not padded as if separate.
- **Anti-pattern this prevents:** Toolbar where everything is equidistant — user can't see grouping.
- **Verification:** Audit reads spacing values against CONSTITUTION scale.

### Similarity (Gestalt)

- **Applies to:** Button styles in toolbar, status badges, row treatments.
- **Rule for this composite:**
  - All ghost buttons (icon-only or text) in the Right cluster share the exact same `IconButton` / `Button (ghost)` atom.
  - All status badges across rows use the same `Badge` atom with semantic variants.
- **Anti-pattern this prevents:** Three different "refresh" looking icons across the toolbar from different atom families.
- **Verification:** Audit checks atom-name consistency.

### Uniform Connectedness (Gestalt)

- **Applies to:** Row hover affordance (context menu trigger appears on hover, attached to the row), selection mode (toolbar + strip + checkboxes co-active).
- **Rule for this composite:**
  - Context menu trigger appears within the row on hover, visually connected (same row background), not floating above.
  - In selection mode, the selection-strip color/tint matches the selected-row tint — signals "these things go together."
- **Anti-pattern this prevents:** Floating action menu that obscures which row it targets.
- **Verification:** Manual visual check; audit can verify component structure.

### Prägnanz

- **Applies to:** Overall composite layout, empty state, error state.
- **Rule for this composite:**
  - The simplest mental model of this composite should be true: "a list of records with a toolbar of controls and a footer of summary." If a user has to construct a more complex model to use it, simplify.
  - Empty states do one thing — explain absence + recovery. No nested CTAs, no upsells, no tips.
- **Anti-pattern this prevents:** Composites that look like they're three different things bolted together; empty states with five competing CTAs.
- **Verification:** Subjective; reviewed at audit.
```

---

## What to write

`LAWS.md`:

```markdown
# <Composite name> — Laws of UX applied

## Composite-level laws

<one subsection per law from the 11, following the template>

## Cross-cutting laws

<For each from CONSTITUTION cross-cutting reminders: Choice Overload, Cognitive Bias, Paradox of the Active User, Selective Attention, Working Memory — note any composite-specific consideration. Most will be brief.>

## Conflicts with CONSTITUTION

If applying a law required deviating from CONSTITUTION:

- **Law:** ...
- **CONSTITUTION rule it stretches:** ...
- **Reason for the stretch:** ...
```

---

## Success criteria for phase 9

- All 11 composite-level laws have entries (or "N/A: <reason>" for genuinely inapplicable)
- Each rule is verifiable — either by audit or by manual review with a clear check
- Conflicts with CONSTITUTION are surfaced (rare but valuable to capture)
- The Verification line of each law tells `saas-ui-audit` what to check

---

## What NOT to do in phase 9

- **Don't apply laws decoratively.** "Fitts's Law: targets should be big" with no specifics is noise. Specify sizes, anatomy nodes, and verification.
- **Don't restate CONSTITUTION.** If a law concern is fully handled by CONSTITUTION (e.g., Density rules cover Fitts), reference and move on.
- **Don't skip Gestalt.** Composites are where Gestalt earns its keep — most LLM-generated UI fails Common Region, Proximity, and Similarity all at once.
- **Don't apply laws without anti-patterns.** The anti-pattern names what would happen without the rule — that's the test of whether the rule is real.

---

## Checkpoint

> "Three checks:
> 1. Pick the three laws you're most likely to violate (be honest — every team has favorites it ignores). Are the Verification lines specific enough to catch a violation?
> 2. For Gestalt laws specifically — open the composite mentally and ask: do the groupings 'fall out' visually, or do they require thought to perceive? If thought, tighten the rules.
> 3. Any law that felt forced? Mark N/A with a reason — it's healthier than padding."

Iterate. Lock. Move to phase 10.

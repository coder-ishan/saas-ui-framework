# Phase 2 — Action Inventory

**Purpose:** Enumerate every user-meaningful action this composite supports, and prove each has exactly ONE primary affordance per region. This is the duplicate-button gate — the most important phase in the entire framework.

**Produces:** `SPEC.md` → Action Inventory section.

**Estimated time:** 20–30 minutes for a rich composite (DataTable). 5–10 minutes for a simple one (Toast, Tooltip).

---

## Why this phase exists

The single most common LLM UI failure is **duplicate-purpose affordances**: two buttons in the same region that do the same thing, or a button + menu item that overlap, or an action exposed three times in different toolbars "for discoverability."

It happens because LLMs optimize for surface completeness rather than for semantic uniqueness. Without this phase, the SPEC will read coherent but the implementation will ship duplicates.

This phase makes the duplicates appear *in the spec*, where they can be argued or deleted before any code is written.

---

## The four-step inventory

### Step 1 — List every user-meaningful action

> "Walk through the composite mentally. Every time the user *does* something that has a semantic meaning, name it. Use verbs. Don't worry about how it's surfaced yet."

For DataTable, candidate list:
- Sort by column
- Filter rows (cross-column)
- Filter rows (single-column facet)
- Search rows (free-text)
- Select one row
- Select multiple rows (range)
- Select all visible rows
- Open row detail
- Edit cell inline
- Edit row (full detail)
- Add row
- Duplicate row
- Delete row
- Bulk delete
- Bulk edit
- Bulk export
- Reorder columns
- Show/hide columns
- Resize column
- Pin column
- Group by column
- Save view (sort + filter + columns)
- Switch view
- Refresh data

That's ~24 actions. They're not all going to make the cut — that's the next step.

### Step 2 — Drop, merge, or argue

For each action, ask:

1. **Drop:** Is this action in our Contract or required by a Non-goals exclusion? If neither, why does it exist?
2. **Merge:** Does this action share a verb with another? If yes, are they semantically distinct or are they the same action surfaced twice?

Apply the **Two-Verb Rule:** If two actions share a verb, write the user-visible semantic. If the semantics match → merge. If they differ → both stay, with the semantic difference recorded.

Example:
- "Filter rows (cross-column)" + "Filter rows (single-column facet)" share the verb. Semantic difference: the second is scoped to one column via its header dropdown; the first is the toolbar filter chip and crosses columns. → Both stay, with argument.

- "Edit cell inline" + "Edit row (full detail)" share the verb. Semantic difference: inline is fast, single-field; detail is comprehensive, multi-field. → Both stay, with argument.

- "Select one row" + "Open row detail" — different verbs (select vs open). Selection is for batch-acting; opening is for inspecting. → Both stay; no overlap.

### Step 3 — Pick the single primary affordance per region

A *region* is a coherent visual zone: toolbar, table header row, row, row hover area, modal, footer.

For each surviving action, decide: **What is its ONE primary affordance, and in which region?**

Rules:
1. One primary affordance per region per action.
2. Secondary modalities (keyboard, command palette, context menu, drag-drop) are not duplicates.
3. If an action is reachable from two regions, that's allowed *only* if the regions serve different mental contexts (e.g., "Add row" from toolbar = "I'm starting fresh"; "Add row" from empty-state body = "the list is empty, here's the way in"). Both must point at the same handler.

For DataTable example:
| Action | Primary affordance | Region | Secondary modalities |
|---|---|---|---|
| Sort by column | Click column header | Header row | Cmd+click adds secondary sort |
| Filter (cross-column) | Filter chip "+ Filter" | Toolbar | Cmd+F focuses filter, Palette: "Filter rows" |
| Filter (single-column) | Column header chevron → facet | Header cell | none (scoped to column UI) |
| Search | Search input | Toolbar | Cmd+K (global), `/` (in-table) |
| Select one row | Click checkbox | Row leading cell | Space (on focused row) |
| Select multiple | Shift+click checkbox | Row leading cell | Shift+arrow extends |
| Select all visible | Header checkbox | Header row | Cmd+A |
| Open row detail | Click row body (not checkbox cell) | Row | Enter (on focused row) |
| Edit cell | Double-click cell, or focus + Enter | Cell | E key on focused cell |
| Edit row (detail) | "Edit" in row detail drawer | Detail drawer toolbar | Cmd+E in drawer |
| Add row | "+ Add row" button | Toolbar | Cmd+N, Palette: "Add row", empty-state CTA |
| Duplicate row | "Duplicate" in row context menu | Row context menu | Cmd+D on focused row |
| Delete row | "Delete" in row context menu | Row context menu | Del key on focused row |
| Bulk delete | "Delete (N)" appears in toolbar **only when selection > 0** | Toolbar (selection mode) | Del key with selection |
| Bulk export | "Export (N)" appears in toolbar **only when selection > 0** | Toolbar (selection mode) | none |
| Show/hide columns | "Columns" button → menu | Toolbar | Palette: "Show/hide columns" |
| Resize column | Drag column edge | Header cell | none (drag-only by design) |
| Save view | "Save view" in view switcher menu | View switcher | Cmd+S in table |
| Switch view | View switcher dropdown | Toolbar | Cmd+Shift+V opens picker |
| Refresh | "Refresh" icon button | Toolbar | Cmd+R (in table scope) |

Notice:
- "Delete row" (single, via context menu) and "Bulk delete" (multi, via toolbar) — same verb, different scope. Both stay. **Argument:** scope distinction is mental-model-distinct; conflating them would force user to enter selection mode for single-row deletes, which is bad UX.
- Bulk actions only appear when selection > 0 — they don't compete with normal toolbar actions in idle state.
- "Edit cell" and "Edit row (detail)" both kept — semantic-distinct (granularity).

### Step 4 — Detect remaining duplicates and resolve

For each pair of actions sharing a verb or semantic, write one of:

- **Argue:** `<verb> via <affordance A> is for <context A>; via <affordance B> is for <context B>. Distinct.`
- **Delete:** remove one (and update the actions list).
- **Merge:** combine into one action (rename to reflect unified semantic).

**Hard rule:** No action may appear with two primary affordances in the *same* region with overlapping context. If two seem to fit that pattern, one must go.

---

## What to write

In `SPEC.md`, fill this section:

```markdown
## Action Inventory

### Actions

| # | Action | Primary affordance | Region | Secondary modalities | Notes |
|---|---|---|---|---|---|
| 1 | <action> | <affordance> | <region> | <list> | <if any> |
| 2 | ... | | | | |

### Argued duplicates

For each pair of actions that share a verb:

#### <verb>: <action A> vs <action B>
- A's context: <when user reaches for it>
- B's context: <when user reaches for it>
- **Argue:** <why both must exist; what would be lost by collapsing>

### Deleted candidates (audit trail)

For each action considered but dropped:
- <action> — dropped because <reason>

### Cross-region duplication (allowed)

If an action's primary affordance appears in two regions, justify:
- <action>: surfaces in <region A> for <mental context A>, and in <region B> for <mental context B>. Both invoke the same handler. <why this is not a duplicate>
```

---

## Success criteria for phase 2

- Every kept action has exactly one row in the inventory
- No two actions share a primary affordance + region combination unless an Argued duplicates entry exists
- Every action that appears in two regions has a cross-region justification
- The audit trail captures dropped candidates so future SPEC reviewers can verify decisions

---

## What NOT to do in phase 2

- **Don't add affordances for discoverability.** "But users might not find the keyboard shortcut" is solved by tooltips and the command palette, not by sprinkling buttons.
- **Don't keep both a toolbar button and a menu item for the same action in the same region.** Pick one. The audit (`saas-ui-audit`) will catch this anyway, but better here.
- **Don't conflate keyboard + button as primary affordances.** Keyboard is always secondary. The "primary affordance" is the visible UI thing.
- **Don't treat the command palette as a duplicate.** It is by design a secondary modality for every action. Always allowed.
- **Don't use icons-only on the primary affordance unless the icon is universal (search, close, settings).** Otherwise the icon is the label and the action fails the discoverability test from another direction.

---

## Checkpoint

Show the user the full table and the Argued duplicates section. Ask:

> "Three checks before we lock this:
> 1. Is any action missing? (Walk through the user flows you imagined.)
> 2. Does any 'Argue:' line feel weak? If you can't defend the distinction in one sentence, collapse the actions.
> 3. Are there actions you *want* to add for parity with another product but that don't belong here? Those should go on a 'considered, not adopted' list."

Iterate. Lock the section. Move to phase 3.

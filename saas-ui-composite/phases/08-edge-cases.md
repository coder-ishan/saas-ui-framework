# Phase 8 — Edge cases

**Purpose:** Catalog the failure modes that ship as bugs. Each edge case gets a handling rule. The framework's stance: an edge case without a rule is a future incident.

**Produces:** `EDGE-CASES.md` for this composite.

**Estimated time:** 20 minutes.

---

## The canonical edge-case checklist

Walk through every category. For each, decide: applies / doesn't apply / partially applies. For each that applies, write the handling rule.

### Input edge cases

- **Empty input** in a search/filter that requires a value
- **Extremely long input** (10k+ characters in a search, very long row content)
- **Whitespace-only input** treated as empty
- **Unicode/emoji** in searchable fields
- **Right-to-left text** rendering in mixed-direction content
- **Pasted content with formatting** in plain-text fields
- **Pasted CSV / tabular content** in a single field (auto-split? reject? accept?)

### Interaction edge cases

- **Rapid clicks / double submission** on Add row, Delete, Save
- **Click during inflight mutation** on the same row
- **Drag interrupted** (mouseup outside window, ESC during drag)
- **Focus loss during inline edit** (click outside, tab away, alt-tab)
- **Hover on element that disappears** (mutation removes the row mid-hover)
- **Selection during sort** (does selection survive? Cross-state rules say yes; what if a selected row drops out of view?)
- **Selection during filter** (selection clears per Cross-state rules — confirm)

### Concurrency edge cases

- **Stale data on return to tab** (user comes back after 10 minutes; the table data may be stale)
- **Concurrent edit** (two users edit the same row; whose write wins?)
- **Optimistic mutation race** (user clicks delete on row that's mid-add by another user)
- **Server error after optimistic apply** (CONSTITUTION → State Model rollback toast)
- **Network drop mid-mutation** (per CONSTITUTION → State Model → Offline)
- **Page reload during mutation** (Server Actions are crash-tolerant if idempotent; otherwise lost)

### Permission edge cases

- **Permission revoked mid-session** (user loses write access between page load and edit attempt)
- **Row-level permission** (user can see row but not edit it; partial-permission state per phase 4)
- **Tenant switched mid-action** (multi-tenant SaaS)

### Browser / device edge cases

- **Browser back after mutation** (does the table reflect post-mutation state? URL state should restore it)
- **Browser forward into a deleted state** (URL points at filter that excluded a row that's now gone)
- **Page reload mid-edit** (dirty state lost unless autosaved per CONSTITUTION)
- **Mobile / narrow viewport** (Toolbar reflows? Columns hidden? Drawer behavior?)
- **Tablet touch + keyboard combo** (hover-only affordances don't exist; long-press for context menu?)
- **Print** (does the table print? With or without selection / hover decorations?)
- **Zoom level** (200% browser zoom — does layout hold?)
- **High-contrast / forced-colors mode** (does the focus ring survive?)

### Accessibility edge cases

- **Screen reader on a virtualized list** (announce visible vs total; jump-to-row)
- **Keyboard-only on a long table** (Page Up/Down? Home/End?)
- **Reduced motion** (`prefers-reduced-motion`: motion overrides go to instant; toast still appears, just without slide)
- **Color-only signaling** (selection state, status indicators — must pair color with icon/weight per Forbidden Patterns #8)

### Data shape edge cases

- **Zero rows** (true-empty per STATES.md)
- **One row** (singular pluralization in copy: "1 row" vs "N rows")
- **Exactly the page boundary** (50 rows when threshold is 50)
- **All rows match filter** (filter-empty is the inverse; what if everything matches?)
- **Missing required fields** in a row (display N/A? Skip? Error?)
- **Extremely long cell content** (truncate with ellipsis + tooltip on hover; long-content state from STATES.md)
- **HTML/script in cell content** (escape always; never `dangerouslySetInnerHTML` on data fields)

### Time-zone edge cases

- **Server vs client time zones** (display in user's TZ; store in UTC)
- **DST transitions** in saved views with relative time filters
- **Locale-sensitive formatting** (numbers, currency, dates, plurals)

---

## Per-edge-case handling rule

For each edge case that applies, write:

```markdown
### <category> — <edge case>

- **Scenario:** <one-line scenario>
- **Handling:** <what the composite does>
- **State affected:** <which STATES.md state, if any>
- **Copy (if user-visible):** <follow CONSTITUTION Copy structure>
```

Example:

```markdown
### Concurrency — Server error after optimistic delete

- **Scenario:** User deletes a row. Optimistic UI removes it instantly. Server action fails (network drop, permission revoked, row already deleted by another user).
- **Handling:** Optimistic state rolls back — row reappears in its prior position. Rollback toast appears at bottom-right with: "<row identifier> couldn't be deleted." + secondary text naming the reason if known + "Retry" button. Toast persists 10s (per CONSTITUTION → State Model → Recovery → Bulk destructive).
- **State affected:** loading-mutation → error (recoverable).
- **Copy:** "Couldn't delete '<row name>': <reason>. Try again." Button: "Retry".
```

---

## What to write

`EDGE-CASES.md` has:

```markdown
# <Composite name> — Edge cases

## Applicable edge cases

<per-edge-case entries grouped by category from the checklist>

## Non-applicable edge cases

For each item from the canonical checklist that doesn't apply, list it with a one-line reason:

- **<edge case>:** N/A — <reason>

## Composite-specific edge cases

Edge cases unique to this composite, not on the canonical checklist:

### <name>

- **Scenario:** ...
- **Handling:** ...
```

---

## Success criteria for phase 8

- Every item in the canonical checklist is addressed (handling rule OR N/A with reason)
- Every applicable edge case references the State (from phase 4) it affects
- Every applicable edge case with user-visible copy uses the CONSTITUTION Copy structure
- Composite-specific edge cases are listed separately so they don't get lost in the canonical checklist

---

## What NOT to do in phase 8

- **Don't say "shouldn't happen."** Edge cases by definition shouldn't happen — that's why they need rules.
- **Don't handle errors with "Something went wrong."** Forbidden Pattern #15. Name the failure.
- **Don't put double-submit prevention in UI alone.** UI dedup is a guardrail; the Server Action must also be idempotent.
- **Don't ignore reduced-motion.** It's a CONSTITUTION → Motion concern too; just confirm here that motion overrides degrade gracefully.

---

## Checkpoint

> "Three checks:
> 1. Walk through a 'bad day' scenario — slow network, partial permissions, intermittent failures. Does the composite degrade gracefully through the rules here?
> 2. Did any category in the checklist surprise you (e.g., 'print' often skipped, but tables that ship to enterprises get printed)?
> 3. Composite-specific edge cases — what did you encounter in similar products that's not on the canonical list?"

Iterate. Lock. Move to phase 9.

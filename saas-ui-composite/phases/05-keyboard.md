# Phase 5 — Keyboard map

**Purpose:** Specify every keyboard shortcut for this composite, detect conflicts with app-global shortcuts, and ensure the composite is fully operable from the keyboard.

**Produces:** `KEYBOARD.md` for this composite.

**Estimated time:** 15 minutes.

---

## The three-step keyboard map

### Step 1 — Pull keyboard secondaries from Action Inventory

Every entry in phase 2's Action Inventory that listed keyboard as a secondary modality goes here. Re-list them with full context.

For DataTable:
| Key | Action | Scope | When active |
|---|---|---|---|
| `↑` `↓` | Move row focus | Body | when focus is inside body |
| `←` `→` | Move cell focus (within a row, if cell-focus mode) | Body | when row is focused and cell mode |
| `Space` | Toggle row selection | Body | when row focused |
| `Shift+↑/↓` | Extend selection | Body | when row focused |
| `Cmd+A` | Select all visible rows | Composite | when focus inside composite |
| `Enter` | Open row detail | Body | when row focused |
| `Esc` | Clear selection → close detail → exit composite | Composite | nested behavior |
| `E` | Edit focused cell | Body | when cell focused |
| `Cmd+E` | Edit focused row in detail drawer | Body | when row focused |
| `Cmd+N` | Add row | Composite | when focus inside composite |
| `Cmd+D` | Duplicate focused row | Body | when row focused |
| `Del` | Delete focused row OR delete selection | Body | when row focused or selection > 0 |
| `Cmd+F` | Focus filter chip | Composite | when focus inside composite |
| `/` | Focus search input | Composite | when focus inside composite, no input focused |
| `Cmd+S` | Save current view | Composite | when focus inside composite |
| `Cmd+R` | Refresh data | Composite | when focus inside composite |
| `Cmd+Shift+V` | Open view switcher | Composite | when focus inside composite |
| `Cmd+click` (header) | Add secondary sort | Header | — |

### Step 2 — Conflict check

For each shortcut, verify it doesn't collide with:

1. **App-global shortcuts.** These come from CONSTITUTION → Action Affordances and the app's global keyboard map (typically Cmd+K for palette, Cmd+/ for help, Cmd+Shift+P for command palette in some apps).
2. **Browser-native shortcuts.** Avoid overriding Cmd+R (refresh page), Cmd+W (close tab), Cmd+T (new tab), Cmd+L (focus URL bar), Cmd+F (find in page).
3. **Other composite shortcuts** that may be active simultaneously (e.g., if a CommandPalette can be open over a DataTable, their shortcuts must not collide).

For each collision, resolve:
- **Override with reason:** "Cmd+R in our table scope refreshes the table data, not the page. The browser default is replaced when focus is inside the composite. Reason: in a SaaS dashboard, table refresh is the dominant interpretation; users can Cmd+Shift+R to bypass."
- **Reassign:** pick a different key.
- **Scope:** make the shortcut only active when focus is inside the composite (most common solution).

Add a `Conflict resolutions` table:

```markdown
| Shortcut | Conflicts with | Resolution | Reason |
|---|---|---|---|
| `Cmd+R` | Browser page refresh | Scope to composite; preventDefault when focus inside | Table refresh dominates in this context |
| `/` | None (forward slash unbound globally) | — | — |
| `E` | None | — | — |
```

### Step 3 — Focus & traversal contract

Specify how focus enters, moves, and exits:

```markdown
## Focus contract

**Entry:**
- Tab from outside composite → focus lands on first interactive element in toolbar (search input by default)
- Click anywhere inside composite → focus lands on clicked element

**Movement (Tab order):**
- Toolbar (left to right, top to bottom)
- Header (first column header → last)
- Body (first row, leading cell → last row, trailing cell)
- Footer

**Body navigation:**
- Once focus enters body, arrow keys take over (NOT Tab)
- Tab from inside body exits to footer
- Shift+Tab from inside body exits to header

**Exit:**
- Tab from last footer element → exits composite to next page tabstop
- Esc — see Escape ladder below

## Escape ladder

Esc has a nested behavior: each press steps OUT one level:
1. If a cell is in active-edit → Esc exits edit, focus returns to cell
2. Else if selection > 0 → Esc clears selection
3. Else if row detail is open → Esc closes detail, focus returns to row
4. Else → Esc passes through (parent component handles)

Reason: predictable layered dismissal, no surprise navigation.
```

---

## What to write

`KEYBOARD.md` has:

```markdown
# <Composite name> — Keyboard map

## Shortcuts table

<the table from Step 1>

## Conflict resolutions

<from Step 2>

## Focus contract

<from Step 3>

## Escape ladder

<from Step 3>

## Accessibility notes

- ARIA roles: <list per anatomy node>
- Live region announcements: <when selection count changes, when filter applies, when row is deleted>
- Focus visible: yes — every focusable element uses CONSTITUTION-defined focus ring
```

---

## Success criteria for phase 5

- Every keyboard secondary from Action Inventory is in the table
- Every shortcut has a Scope (Composite | Body | Header | Toolbar | Detail drawer)
- Every collision is resolved with a reason
- Focus contract describes entry, movement, and exit explicitly
- Escape ladder is unambiguous
- ARIA roles are listed for every anatomy node from phase 3 that's interactive

---

## What NOT to do in phase 5

- **Don't invent shortcuts.** Only shortcuts from Action Inventory's secondary modalities. New shortcuts require adding them there first.
- **Don't overload single letters globally.** `E` for edit is fine when scoped to body; making `E` work app-wide creates collisions across modules.
- **Don't omit Esc.** Every composite has an Escape behavior even if it's "pass through to parent." Declaring it explicitly prevents parent-level surprises.
- **Don't claim accessibility without naming ARIA roles.** "Accessible" is a posture, not a deliverable. The ARIA roles per anatomy node are the deliverable.

---

## Checkpoint

Show the user the table, conflicts, and focus contract. Ask:

> "Three checks:
> 1. Walk through the experience of a power user landing on this composite with keyboard only. Can they do every Action Inventory item without ever touching the mouse?
> 2. Does any shortcut break a habit from a familiar product? If yes, is the break worth the friction? (Jakob's Law — CONSTITUTION → Product → Mental model has the list of conventions you inherit.)
> 3. The Escape ladder — is the order of unwinding what you'd want? Common variation: some products clear selection before closing detail; this SPEC closes detail first if both are present."

Iterate. Lock. Move to phase 6.

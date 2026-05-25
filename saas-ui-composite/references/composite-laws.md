# Composite-level Laws of UX — cheat sheet

> Quick reference. Full per-law application lives in `composites/<name>/LAWS.md`.

These 11 laws have the highest leverage at composite level. Project-level laws (Doherty, Jakob's, Tesler's, Pareto, Postel, Aesthetic-Usability, Cognitive Load, Mental Model) live in `.saas-ui/PRINCIPLES.md` and aren't restated here.

---

## 1. Fitts's Law

**Law:** Time to acquire a target is proportional to the distance / target-size ratio.

**At composite level:** Larger targets and closer-to-cursor targets are faster. Edge and corner targets are effectively infinite-sized (the cursor can't go past the screen edge).

**Checks:**
- Toolbar buttons ≥ 32px (mouse), 44px (touch)
- Row-level click targets fill the row, not just the icon
- Primary actions are positioned near where the user's attention already is (top-right for "Add", inline with row for row-level)
- Avoid tiny icon-only triggers that aren't on edges

---

## 2. Miller's Law

**Law:** Working memory holds ~7±2 chunks.

**At composite level:** Cap visible options. When a list exceeds the limit, group or hide behind progressive disclosure.

**Checks:**
- ≤ 7 visible columns by default
- ≤ 5 active filter chips before "+ N more"
- ≤ 7 first-level context menu items
- ≤ 7 saved views before grouping or search

---

## 3. Hick's Law

**Law:** Decision time grows logarithmically with the number of choices.

**At composite level:** Reduce visible choices, especially in toolbars and primary action surfaces.

**Checks:**
- ≤ 5 toolbar actions in idle state
- ≤ 4 actions in bulk-mode strip
- Dropdown menus chunked into sections of ≤ 5

---

## 4. Von Restorff Effect

**Law:** The thing that's different is remembered.

**At composite level:** Make the most-important action visually distinct. Everything else should be quieter so the primary action wins.

**Checks:**
- Exactly ONE `primary` variant button per region (CONSTITUTION → Density → Visual weight order)
- Destructive actions use `destructive` variant — only used where actually destructive (boy-who-cried-wolf)
- Status anomalies (overdue, blocked, errored) use color + icon + weight, not color alone (Forbidden Pattern #8)

---

## 5. Working Memory

**Law:** Users can't hold multiple pieces of state across interactions reliably.

**At composite level:** Persist context. Don't make the user remember what they were doing.

**Checks:**
- Edit drawer shows row identifier persistently
- Bulk-mode strip shows selection count persistently
- Filter builder shows compiled filter in plain text
- Multi-step flows show progress + reversibility per step

---

## 6. Chunking

**Law:** Grouping related items reduces cognitive load.

**At composite level:** Cluster related actions visually. Within-cluster spacing < between-cluster spacing.

**Checks:**
- Toolbar has explicit clusters (Left: scoping; Right: control)
- Header row visually distinct from body
- Footer separated by divider
- Form groups distinct from each other

---

## 7. Common Region (Gestalt)

**Law:** Elements in a bounded region are perceived as related.

**At composite level:** Use bounded regions to signal grouping; remove boundaries that signal false grouping.

**Checks:**
- Composite has a single outer container (bordered, shadowed, or tinted)
- Selection-mode strip has its own region treatment when active
- Filter chips are visually contained (chip-group has its own region)

---

## 8. Proximity (Gestalt)

**Law:** Closer elements are perceived as more related.

**At composite level:** Calibrate spacing so groupings emerge from spacing alone.

**Checks:**
- Within-group spacing strictly less than between-group spacing
- Toolbar cluster gaps follow CONSTITUTION → Density → Section spacing
- Inline-edit input flush within cell (signals "this is the cell," not "input over cell")

---

## 9. Similarity (Gestalt)

**Law:** Elements that look similar are perceived as the same kind.

**At composite level:** Same atom, same visual treatment, everywhere.

**Checks:**
- All ghost buttons in the same region use the exact same atom + variant
- All status badges use the same `Badge` atom with semantic variants
- All inputs use the same `Input` atom (no mixing native input and custom)

---

## 10. Uniform Connectedness (Gestalt)

**Law:** Elements visually connected (lines, surfaces, color) are perceived as unified.

**At composite level:** Make groupings unmistakable through connection, not just proximity.

**Checks:**
- Context menu trigger appears within the row's background on hover (connected to row)
- Selection-strip color matches selected-row tint (connected via color)
- Drawer attached to row visually (sliding from row's right edge, not floating over)

---

## 11. Prägnanz (Good Form)

**Law:** People perceive the simplest possible interpretation of what they see.

**At composite level:** The simplest mental model of the composite should be the true one. If the composite requires assembling multiple metaphors mentally, simplify.

**Checks:**
- One-line purpose from phase 1 is the *actual* model — no hidden behaviors that violate it
- Empty states do ONE thing — explain + recover (no upsells, no tips, no nested CTAs)
- Error states do ONE thing — name + recover

---

## How `saas-ui-audit` uses this

Each law's Verification line tells the audit what to check programmatically vs manually:

- **Programmatic:** count-based laws (Miller, Hick), atom-name laws (Similarity), variant-usage laws (Von Restorff)
- **Manual:** subjective laws (Prägnanz, partial Common Region)
- **Mixed:** Fitts (pixel sizes are programmatic; positioning judgement is manual)

The audit emits a per-composite scorecard with pass/fail per law.

# Phase 4 — State enumeration

**Purpose:** Force every canonical state to be designed, not left implicit. The 13 canonical states are the most common shipping holes — empty states that say "No data," errors that say "Something went wrong," loading states that flash a spinner. This phase makes them all explicit.

**Produces:** `STATES.md` for this composite.

**Estimated time:** 25 minutes.

---

## The 13 canonical states

Reference: `references/state-catalog.md` for definitions.

Required entries (every composite SPEC must address each, even if to say "N/A because <reason>"):

1. **idle** — at rest, no user attention
2. **hover** — pointer over interactive element
3. **focus** — keyboard focus
4. **selected** — explicit selection (distinct from focus)
5. **active-edit** — inline editing mode
6. **loading-initial** — first paint, no data yet
7. **loading-mutation** — data shown, write pending
8. **empty (true-empty)** — no data ever existed
9. **empty (filter-empty)** — data exists, filter excludes all
10. **error (recoverable)** — operation failed, user can retry
11. **error (terminal)** — operation failed permanently (permission denied, deleted, 404)
12. **partial-permission** — user can see, but not all actions allowed
13. **offline / stale** — connection lost, or data stale
14. **long-content** — content overflows (long text, deep nesting, wide table)

(14 entries — the catalog uses "13 canonical" because hover/focus often share treatment, but they remain distinct entries in this phase.)

---

## Per-state template

For each state, fill four cells:

```markdown
### <state name>

- **When it occurs:** <conditions, in user terms>
- **Visual treatment:** <reference INVENTORY atoms + CONSTITUTION density rules; no hex codes>
- **Allowed actions:** <which Action Inventory items are reachable in this state; rest are disabled/hidden>
- **Copy (if any):** <follow CONSTITUTION Copy section structure; for empty/error states use the [What] · [Why] · [Recovery] pattern>
- **Transitions out:** <what user action or system event moves to which next state>
- **N/A:** <only if state genuinely doesn't apply — explain why>
```

Example (filter-empty for DataTable):

```markdown
### empty (filter-empty)

- **When it occurs:** Filters are applied; zero rows match.
- **Visual treatment:** Body region replaced with centered `EmptyState` molecule (variant=secondary), illustration omitted (this isn't a true-empty), with a "Clear filters" button using `Button` (ghost). Toolbar stays visible and unchanged. Footer hides row-count summary; pagination collapses.
- **Allowed actions:** Clear filters, Adjust filters (toolbar filter chip stays interactive), Search (toolbar search stays), Switch view, Refresh.
- **Copy:** Header: "No rows match these filters." Body: "Adjust filters or clear them to see all <N> rows." Action label: "Clear filters". (Follows CONSTITUTION Copy → Empty state structure: What absent · Why · Recovery.)
- **Transitions out:** "Clear filters" → idle (with all rows). Adjusting a filter that produces matches → idle (with filtered rows). Adding more data while filtered → still filter-empty until match exists.
- **N/A:** No.
```

---

## Cross-state contract rules

After filling all 13/14 entries, write a `Cross-state rules` section that captures invariants spanning states:

```markdown
## Cross-state rules

1. **Selection persistence:** Selection survives sort and column reorder. Selection clears on filter change. Reason: filtering is a scope change; sort is presentation.
2. **Focus retention:** Keyboard focus on a row survives optimistic mutation; if the row is removed by the mutation, focus moves to the next sibling row.
3. **No simultaneous loading + error:** If loading-mutation fails, transition is loading-mutation → error (recoverable, rollback toast), not loading-mutation + error.
4. **Skeleton matches final layout:** loading-initial skeleton uses the same row height and column widths as the final render — no visible reflow on data arrival. Reason: prevents layout shift, holds Doherty.
5. **Empty (true) and empty (filter):** never both — true-empty means zero rows exist; filter-empty means rows exist but filtered out. If both could apply, true-empty wins.
```

---

## State machine (optional, only for composites with non-trivial transitions)

For composites like CommandPalette, FilterBuilder, or anything with multi-step flows, append a pseudocode state machine.

```markdown
## State machine

```text
idle
  ├─ user types → loading-initial (debounced search)
  │   ├─ results → idle (with results)
  │   └─ no results → empty (filter-empty)
  ├─ user presses ↓ → focus (first item)
  └─ user presses Esc → close (transition out of composite)

focus
  ├─ user presses Enter → execute (transition to action's handler)
  ├─ user presses Esc → idle
  └─ user types → loading-initial
```
```

This is the only place pseudocode is allowed in the framework.

---

## What to write

The full `STATES.md` is the 13/14 state entries + Cross-state rules + (optional) state machine. Use the template at `templates/STATES.md`.

---

## Success criteria for phase 4

- All 13 canonical states have entries (with N/A justified if genuinely inapplicable)
- Every Allowed-actions list references actions by name from the Action Inventory in phase 2
- Every Copy entry follows the CONSTITUTION Copy section structure
- Cross-state rules captures at least 3 invariants
- No state is left as "TBD"

---

## What NOT to do in phase 4

- **Don't write generic empty states.** "No data" is forbidden by CONSTITUTION → Forbidden Patterns #5. Name what's absent, why, and the recovery action.
- **Don't conflate hover and focus.** Mouse hover and keyboard focus are distinct mental contexts. They may share visual treatment, but the entries must be separate so accessibility behavior is explicit.
- **Don't show full-screen spinners.** loading-initial uses skeleton matching layout; loading-mutation uses inline indicators. CONSTITUTION → State Model is the source.
- **Don't error-mask.** "Error (terminal)" must name the failure ("Account not found", "Access denied"), not display "Something went wrong" (Forbidden Patterns #15).
- **Don't design partial-permission as "everything's disabled."** Show what the user CAN do; remove or fade what they can't, with a tooltip or inline reason where ambiguity exists.

---

## Checkpoint

Show the user the full STATES.md draft. Ask:

> "Three checks:
> 1. Walk through the loading-mutation row. If the mutation fails, where does the rollback toast appear, what does it say, and how long is it visible? (CONSTITUTION → State Model → Recovery is the source.)
> 2. Is there any state I marked N/A that you actually want? (Common surprise: `long-content` is often skipped — but tables with long cell text, wide columns, or deep nesting need it.)
> 3. Are the Cross-state rules tight enough that an implementer couldn't accidentally violate them?"

Iterate. Lock. Move to phase 5.

# Phase 8 — Forbidden Patterns + Action Affordances

**Purpose:** Explicitly forbid the common LLM UI pathologies so downstream composite SPECs (and the future audit skill) have clear rules to enforce. This phase consumes `references/common-llm-ui-mistakes.md`.

**Produces:** `.saas-ui/CONSTITUTION.md` (Forbidden section + Action Affordances section)

**Estimated time:** 10–15 min

## Why this phase is critical

Most LLM-generated SaaS UI is ruined by a small set of recurring pathologies — duplicate buttons, confirmation modal addiction, decorative icons, spinner overuse, generic copy. Without explicit rules, these creep back in even when other phases are polished. This phase makes the rules first-class.

## Protocol

Load `references/common-llm-ui-mistakes.md`. For each of the 17 pathologies, walk the user through:

1. **State the pathology** (one sentence).
2. **Show the prevention rule** as proposed in the reference.
3. **Ask:** "Accept this rule, modify it, or override (with rationale)?"

Some users will breeze through accepting most rules. Don't speed-run — for each one ask if there's a known exception in their product.

## Pathologies to walk through

(Detail in `references/common-llm-ui-mistakes.md`. The bootstrap should reference each by name.)

1. Duplicate-purpose affordances
2. Confirmation modal addiction
3. Decorative icons in dense UI
4. Spinner overuse
5. Generic empty states
6. Toast spam
7. Tooltip duplication
8. Color-only signaling
9. Inconsistent destructive UI
10. Disabled buttons without explanation
11. Placeholder-as-label
12. Forms without dirty state
13. Modal stacking
14. Animation overuse
15. Generic copy
16. Pagination on short lists
17. Search without affordances

## Special focus: Action Affordances (anchored on pathology #1)

This deserves its own constitution section because it's the most common LLM mistake and the hardest to lint.

Ask:
> For every user-meaningful action in your product, the rule is:
> **Exactly ONE primary affordance per screen.** Secondary modalities (keyboard shortcut, command palette, right-click menu, drag-drop) are NOT duplicates because they're different modalities.
>
> Are there cases in this product where two distinct UI buttons for the same semantic action *should* coexist? For example:
> - "Save draft" in toolbar AND "Save and continue" in flow footer (semantically different actions, both named save)
> - "Quick add" in header AND "+ New" in body (legitimate if header is always-available shortcut, body is context-bound)
>
> Define the *exception rule* if any. The exception is always: "two semantically distinct actions that happen to share a verb."

## What to write

### CONSTITUTION.md — Forbidden section

```markdown
## Forbidden Patterns

The following patterns are forbidden by default. Each rule has its rationale from `references/common-llm-ui-mistakes.md`. Overrides require explicit `Argue:` line per case.

### 1. Duplicate-purpose affordances
**Rule:** Every user-meaningful action has exactly ONE primary affordance per screen. Secondary modalities (keyboard, command palette, context menu, drag-drop) don't count as duplicates — they're different modalities.
**Exception logic:** Two actions that share a verb but are semantically distinct may coexist (e.g., "Save draft" vs "Save and continue"); each must be named with its distinct semantic.
**Override:** <user override or "none">

### 2. Confirmation modal addiction
**Rule:** Confirmation modals forbidden for undoable actions. Use undo toasts. Confirmations only for actions in the Irreversible list (see State Model).
**Override:** <user override or "none">

### 3. Decorative icons in dense UI
**Rule:** Icons must carry meaning. Forbidden: decorative leading icons on labeled buttons, icons that duplicate the label.
**Override:** <user override or "none">

### 4. Spinner overuse
**Rule:** No full-screen spinners. Spinners only inside submit buttons during inflight. Loading by duration (see State Model).
**Override:** <user override or "none">

### 5. Generic empty states
**Rule:** Every empty state names what's absent, why, and the single recommended action. No "No data" or "Nothing here."
**Override:** <user override or "none">

### 6. Toast spam
**Rule:** Toasts reserved for: actions whose source has left viewport, long-delayed successes, undo affordances. Inline feedback preferred for autosave and field-level events.
**Override:** <user override or "none">

### 7. Tooltip duplication
**Rule:** Tooltips only for: unlabeled icon-only buttons, non-obvious side effects, keyboard shortcut hints. Tooltips that restate a visible label are forbidden.
**Override:** <user override or "none">

### 8. Color-only signaling
**Rule:** Every meaningful state difference uses at least two cues from: color, icon, weight, position, label.
**Override:** <user override or "none">

### 9. Inconsistent destructive UI
**Rule:** Destructive actions follow one consistent pattern across the app. Defined location, visual signal, recovery model.
**Override:** <user override or "none">

### 10. Disabled buttons without explanation
**Rule:** A disabled button must communicate why — co-located reason or tooltip.
**Override:** <user override or "none">

### 11. Placeholder-as-label
**Rule:** Every input has a persistent label. Placeholders are examples/hints only.
**Override:** <user override or "none">

### 12. Forms without dirty state
**Rule:** Explicit-save forms show a dirty indicator. Autosave forms show passive sync status.
**Override:** <user override or "none">

### 13. Modal stacking
**Rule:** Modals do not stack. Further interaction happens inline or transitions to a new modal state. Side panels can layer over a parent screen but never over each other.
**Override:** <user override or "none">

### 14. Animation overuse
**Rule:** Motion only clarifies cause-effect. Durations 150–250ms. No spring/bounce in functional UI. >300ms must be argued.
**Override:** <user override or "none">

### 15. Generic copy
**Rule:** Voice rules from Copy section enforced. No "Oops!", no exclamation marks, no "Something went wrong."
**Override:** <user override or "none">

### 16. Pagination on short lists
**Rule:** Pagination for lists >50 items typical. Below that, render all (virtualize if needed). Always show total + page range when paginating.
**Override:** <user override or "none">

### 17. Search without affordances
**Rule:** Search inputs include: recent searches (where applicable), clear button, filter chips (where filters exist), context-relevant placeholder, keyboard shortcut hint if global.
**Override:** <user override or "none">
```

### CONSTITUTION.md — Action Affordances section

```markdown
## Action Affordances

**Core rule:** Every user-meaningful action has exactly ONE primary affordance per screen.

**Secondary modalities are not duplicates.** A button is an affordance. A keyboard shortcut, a command-palette entry, a context-menu item, and a drag-drop gesture are *different modalities* of access to the same action. The same action MAY have:
- 1 primary affordance (button, link, icon button) per screen region where the action is contextual
- 1 keyboard shortcut (global or contextual)
- 1 command-palette entry (if app has command palette)
- 1 context-menu item (right-click, if applicable)
- Drag-drop gestures (where appropriate)

**What counts as a duplicate (forbidden):**
- Two buttons in the same screen region performing the same semantic action
- A button + a menu item performing the same action in the same region (one or the other, not both)
- The same action exposed twice in the same toolbar / header / footer

**Exception logic — argued duplicates:**
Two actions that share a verb but are semantically distinct may coexist. Each must be named with its distinct semantic:
- "Save draft" (in toolbar) vs "Save and continue" (in flow footer) — different actions
- "Add task" (header always-on) vs "+ New" (per-list contextual) — semantically distinct if header creates global-context task and "+ New" creates list-bound task
- ...

**Action Inventory contract (for composite SPECs):**
Every composite SPEC must include an Action Inventory section that lists each semantic action with its single primary affordance and allowed secondary modalities. This is enforced by the future audit skill.
```

## Success criteria

Phase 8 is complete when:
1. CONSTITUTION.md Forbidden section has all 17 rules with explicit accept/modify/override per rule.
2. CONSTITUTION.md Action Affordances section is fully populated with the rule, exception logic, and any product-specific argued duplicates.
3. The Irreversible list from phase 7 is cross-referenced (only those allow confirmation modals).
4. STATE.md updated: `phase_8_complete: true`, `current_phase: 9`.

## What NOT to do

- Don't blanket-accept all 17 rules without walking through each — the user must consciously confirm each so future overrides have rationale.
- Don't let "we'll handle that case-by-case" stand for any forbidden rule. Either there's an exception now, or there isn't.
- Don't allow argued duplicates without distinct semantic naming. "Two save buttons for different vibes" is not an exception.

## Checkpoint

When phase 8 completes, ask: "Forbidden patterns and action rules set. Continue to phase 9 (references), or pause here?"

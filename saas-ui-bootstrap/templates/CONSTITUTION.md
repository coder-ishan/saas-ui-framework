# Design Constitution

> This is the design constitution for <PRODUCT NAME>.
> It encodes the team's taste and decisions about how this product should look, feel, and behave.
> Every UI artifact (composite SPEC, module SPEC, implementation) must comply with this constitution.
> Every rule has a `Reason:` line — when judging edge cases, return to the reason, not the letter.

**Stack:** Next.js (App Router, SSR+CSR)
**Last updated:** <YYYY-MM-DD>
**Owner:** <user>

---

## Product

**Pitch:** <one-sentence pitch>
**Primary user:** <role, context>
**Job-to-be-done:** <core job>

**Usage pattern:** <primary daily app | visited briefly>
**Polish budget implication:** <derived: heavy investment | targeted investment | minimal>

**Wedge:** <unique value>
**80/20 — flows getting 80% of polish:**
1. <flow>
2. <flow>
3. <flow>

**Mental model:**
- Metaphor: <inbox | canvas | board | spreadsheet | other>
- Inherits from: <familiar products>
- Breaks: <models broken>
  Reason: <why the break is worth the cost>

**Cognitive-load posture:** <Linear-style opinionated | ClickUp-style configurable>
  Reason: <user's rationale>

**Tesler's Law — complexity absorption:**
- <flow>: absorbed by <user|app|admin|support>
- <flow>: absorbed by <user|app|admin|support>

**Polish floor reference:**
- Polished: <product name>
- Mediocre: <product name>
- What makes it mediocre: <specifics>

---

## Motion

**Philosophy:** <functional only | functional + restrained delight | expressive>
  Reason: <rationale>

**Latency budget (Doherty):**
- Interactions (sort/filter/search/status): <ms>
- Navigation: <ms>
- Mutations: <optimistic with rollback | wait-and-confirm>
  Reason: <rationale>

**Durations:**
- State changes: <ms>
- Dismissal / appearance: <ms>
- Layered transitions: <ms>
- Above-default rule: <e.g., "anything above 300ms must be argued for in the SPEC">

**Easings:**
- Default appearance: <cubic-bezier(…) or named>
- Default dismissal: <cubic-bezier(…) or named>
- Spring: <config or "not used">

**Canonical motion patterns:**
- Modal open: <pattern>
- Sheet open: <pattern>
- Popover/dropdown: <pattern>
- Toast: <pattern>
- Row expand: <pattern>
- Status change: <pattern>
- <other product-specific patterns>

**Forbidden:**
- No bounce / overshoot in functional UI
- No fade-only transitions (always pair with transform)
- No animations >300ms by default
- No simultaneous animations on >3 elements
- <user-specific additions>

---

## Density

**Posture:** <comfortable | balanced | dense>
  Reason: <rationale>

**Grid:**
- Base unit: <4px | 8px>
- Scale: <full scale>

**Padding rules:**
- Page outer: <px>
- Card: <px>
- Dialog: <px>
- Form group spacing: <px>
- List item: <vert>px / <horiz>px
- Section: <px>

**Typography:**
- Scale: <name: px, name: px, ...>
- Line-height: body <1.x>, headings <1.x>
- Letter-spacing: <values>

**Hierarchy mechanism:**
- Primary: <whitespace | dividers | tints>
- Secondary: <whitespace | dividers | tints>
- Tertiary: <whitespace | dividers | tints | "not used">

**Visual weight order (Von Restorff):**
1. Primary action
2. Active selection
3. Anomaly / urgent indicator
4. Hover/focus
5. Everything else
  Reason: <if reordered>

**Polish floor (non-negotiable):**
- All spacing multiples of <base unit>; ad-hoc values forbidden
- All text uses scale values; ad-hoc font-sizes forbidden
- Every interactive element has defined hover, focus, active states
- <user additions>

---

## Copy

**Voice:** <direct/terse/professional | direct/warm/conversational | playful/branded>
  Reason: <rationale>

**Tense and grammar rules:**
- Confirmations: past tense
- Empty states: present, action-oriented
- Errors: [what failed] · [why] · [recovery]
- Buttons: verb-first imperative
- Labels: sentence case, noun
- Overrides: <list>

**Forbidden phrasings:**
- Exclamation marks (except: <exceptions>)
- "Oops!", apology-as-feedback
- Gamification ("Great job!", "Awesome!")
- "Are you sure?" → undo instead
- "Something went wrong" → name the failure
- Filler ("Please", "Kindly", "Just")
- <additional>

**Error message structure:**
[What failed] · [Why, if relevant] · [Recovery action].
Example: <product-specific>

**Empty state structure:**
[What's absent] · [Why / context] · [Single recommended action].
Example: <product-specific>

**Confirmation structure:**
[Action past tense] · [Subject] · [Undo if applicable].
Example: <product-specific>

**Helper text rule:**
Only when behavior or constraint is non-obvious. Generic helper text forbidden.

**Glossary (product-specific terms):**
- <Term>: <definition + usage>
- <Term>: <definition + usage>

---

## State Model

**Loading strategy:**
- <300ms: nothing
- 300ms–2s: skeleton matching final layout
- >2s: skeleton + "Taking longer than usual" affordance at 5s
- Submit-button inflight: spinner inside button
- Full-screen spinners: forbidden
- Overrides: <list>

**Mutation strategy:** <optimistic-everything | optimistic-for-safe | wait-and-confirm>
  Reason: <rationale>
- Rollback affordance: <toast | inline error | both>
- Conflict handling: <server-wins | client-wins | prompt>
- Pending visual: <subtle | explicit | invisible>

**Autosave:**
- Applies to: <scope>
- Sync indicator: <description>
- Explicit save reserved for: <list>

**Dirty state (for explicit-save forms):**
- Indicator: <description>
- Navigation guard: <yes | no | context>

**Error handling by scope:**
- Field-level: inline below field
- Section-level: banner at top of section
- Page-level: error placeholder with retry
- App-level: toast or persistent banner
- Mutation post-optimistic: rollback toast with retry

**Offline / stale:**
- Support: <none | full local-first | partial>
- Affordance: <description>
- Queue/replay: <description>

**Recovery (undo over confirm):**
- Inline destructive: 5s undo toast
- Bulk destructive: 10s undo toast
- Irreversible (require confirmation): <list>

**Postel's Law — input forgiveness:**
- Date parsing: <accepted formats>
- Currency: <accepted formats>
- Email paste: <multi-email behavior>
- Phone: <normalization>
- URL: <scheme handling>
- Search: <tolerance>
- File drop: <accepted MIME types>
- <other categories>

---

## Forbidden Patterns

> Each rule has its rationale in `references/common-llm-ui-mistakes.md`.
> Overrides require explicit `Argue:` line per case.

### 1. Duplicate-purpose affordances
**Rule:** Every user-meaningful action has exactly ONE primary affordance per screen. Secondary modalities (keyboard, command palette, context menu, drag-drop) don't count as duplicates.
**Exception logic:** Two actions sharing a verb but semantically distinct may coexist; each must be named with its distinct semantic.
**Override:** <override or "none">

### 2. Confirmation modal addiction
**Rule:** Confirmation modals forbidden for undoable actions. Use undo toasts. Confirmations only for the Irreversible list (see State Model).
**Override:** <override or "none">

### 3. Decorative icons in dense UI
**Rule:** Icons must carry meaning. Forbidden: decorative leading icons on labeled buttons, icons that duplicate the label.
**Override:** <override or "none">

### 4. Spinner overuse
**Rule:** No full-screen spinners. Spinners only inside submit buttons during inflight.
**Override:** <override or "none">

### 5. Generic empty states
**Rule:** Every empty state names what's absent, why, and the single recommended action.
**Override:** <override or "none">

### 6. Toast spam
**Rule:** Toasts reserved for: actions whose source has left viewport, long-delayed successes, undo affordances. Inline feedback preferred for autosave.
**Override:** <override or "none">

### 7. Tooltip duplication
**Rule:** Tooltips only for: unlabeled icon-only buttons, non-obvious side effects, keyboard shortcut hints.
**Override:** <override or "none">

### 8. Color-only signaling
**Rule:** Every meaningful state difference uses at least two cues from: color, icon, weight, position, label.
**Override:** <override or "none">

### 9. Inconsistent destructive UI
**Rule:** Destructive actions follow one consistent pattern across the app.
**Override:** <override or "none">

### 10. Disabled buttons without explanation
**Rule:** A disabled button must communicate why — co-located reason or tooltip.
**Override:** <override or "none">

### 11. Placeholder-as-label
**Rule:** Every input has a persistent label.
**Override:** <override or "none">

### 12. Forms without dirty state
**Rule:** Explicit-save forms show a dirty indicator. Autosave forms show passive sync status.
**Override:** <override or "none">

### 13. Modal stacking
**Rule:** Modals do not stack.
**Override:** <override or "none">

### 14. Animation overuse
**Rule:** Motion only clarifies cause-effect. Durations 150–250ms. No spring/bounce in functional UI.
**Override:** <override or "none">

### 15. Generic copy
**Rule:** Voice rules from Copy section enforced.
**Override:** <override or "none">

### 16. Pagination on short lists
**Rule:** Pagination for lists >50 items typical. Below that, render all (virtualize if needed).
**Override:** <override or "none">

### 17. Search without affordances
**Rule:** Search inputs include: recent searches, clear button, filter chips, context-relevant placeholder, keyboard shortcut hint if global.
**Override:** <override or "none">

---

## Action Affordances

**Core rule:** Every user-meaningful action has exactly ONE primary affordance per screen.

**Secondary modalities are not duplicates:**
- 1 primary affordance per screen region
- 1 keyboard shortcut
- 1 command-palette entry (if app has one)
- 1 context-menu item
- Drag-drop gestures where appropriate

**Forbidden:**
- Two buttons in same region performing same semantic action
- Button + menu item performing same action in same region
- Same action exposed twice in toolbar / header / footer

**Argued duplicates (this product):**
- <e.g., "Save draft" toolbar vs "Save and continue" flow footer — semantically distinct>
- <list>

**Action Inventory contract:**
Every composite SPEC must list each semantic action with its single primary affordance + allowed secondary modalities. Enforced by saas-ui-audit.

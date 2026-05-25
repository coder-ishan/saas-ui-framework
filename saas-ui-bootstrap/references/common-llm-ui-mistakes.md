# Common LLM UI Pathologies — what the framework must prevent

Recurring failure modes observed when LLMs generate enterprise SaaS UIs. Each entry describes the pathology, why it happens, and how the framework prevents it at the constitution / SPEC / audit layers.

Use this file when:
- Writing CONSTITUTION.md **Forbidden** section (phase 8) — these become explicit rules
- Writing **Action Affordances** section of CONSTITUTION.md (phase 8) — anchored on pathology #1
- Future `saas-ui-composite` SPEC generation — the "Action Inventory" section anchors on these
- Future `saas-ui-audit` lints

---

## 1. Duplicate-purpose affordances

**Pathology:** Two or more UI elements in the same screen/modal/component perform the same user-meaningful action. "Save" + "Update" buttons. "Cancel" + "Close" + X-icon. "Add task" link in header + "+ New" button below. "Submit" twice in the same form.

**Why LLMs ship this:** Pattern-matching learned from "modal needs buttons" + "form needs submit" + "list needs add affordance" without noticing the action is already represented elsewhere on the same screen.

**Prevention:**
- CONSTITUTION.md rule: "Every user-meaningful action has exactly ONE primary affordance per screen. Secondary modalities (keyboard shortcut, command palette, right-click menu, drag-drop) don't count as duplicates — they are different *modalities*, not different *buttons*."
- Every composite SPEC includes an **Action Inventory** section listing each semantic action with its single canonical surface and allowed secondary modalities.
- Audit lints: group all clickable/keyable elements by semantic action verb; flag any group with >1 same-modality surface in the same screen region.

**Exception (must be argued):** When an action has *two distinct user contexts* (e.g., "Save draft" in toolbar vs "Save and continue" in flow footer), the actions are not semantically identical and may both appear — but each must be named and reasoned for.

---

## 2. Confirmation modal addiction

**Pathology:** "Are you sure?" prompts on every destructive action. Modal-to-confirm-modal chains. Confirmation prompts that interrupt flow for actions that should just be undoable.

**Why LLMs ship this:** Defensive coding instinct ("user might click by accident"). Trained on countless examples that use confirmation modals as the default safety mechanism.

**Prevention:**
- CONSTITUTION.md rule: "Confirmation modals are forbidden for actions that can be undone. Use undo toasts. Confirmations are reserved for actions that cannot be reversed (permanent deletion of irrecoverable data, irreversible exports, bulk actions on >50 items)."
- Map: optimistic action + undo toast > confirmation modal > destructive action without recovery.

---

## 3. Decorative icons in dense UI

**Pathology:** Every button gets an icon. Every menu item has a leading icon. Icon-label-tooltip triples on everything. Visual noise that signals nothing.

**Why LLMs ship this:** "Polish" pattern-matched as "icons everywhere." Icons feel like effort.

**Prevention:**
- CONSTITUTION.md rule: "Icons must carry meaning or disappear. Forbidden uses: decorative leading icons on labeled buttons; icons that duplicate the label semantically; icons that don't serve as scanability anchors."
- Allowed uses: status indicators, type indicators in heterogeneous lists, action shorthand when label cannot fit, brand glyphs.

---

## 4. Spinner overuse

**Pathology:** Spinner appears on every fetch, every state change, every interaction. Spinner takes over the screen for a 200ms operation. Spinner replaces content that was already visible.

**Why LLMs ship this:** "Loading state = spinner" learned association. Doesn't distinguish duration tiers or layout-preserving alternatives.

**Prevention:**
- CONSTITUTION.md rule: "Loading affordances by duration: <300ms → nothing; 300ms–2s → skeleton matching final layout; >2s → skeleton + 'taking longer than usual' affordance at 5s. Spinners only appear inside submit buttons during request inflight. No full-screen spinners."

---

## 5. Generic empty states

**Pathology:** "No data" or "Nothing to show" with no action. Empty state with sad illustration but no guidance on what to do next.

**Why LLMs ship this:** Empty state treated as a string, not a designed moment.

**Prevention:**
- CONSTITUTION.md rule: "Empty states are designed moments. Each empty state must answer: what is missing, why it's missing (no data ever vs no data this filter), and the single most likely action the user should take. No generic 'No items' strings."

---

## 6. Toast spam

**Pathology:** Toast for every successful action including small ones ("Saved!" after every keystroke autosave). Toast stacks that block UI. Toasts that disappear before they can be read.

**Why LLMs ship this:** "Action feedback = toast" learned association. Doesn't reason about whether feedback is already inline.

**Prevention:**
- CONSTITUTION.md rule: "Toasts are reserved for actions whose source has left the viewport, for actions that succeed unexpectedly long after they were initiated, or for actions with undo affordances. Inline feedback (sync indicator, saved checkmark next to field) is preferred for autosave and field-level events."

---

## 7. Tooltip duplication

**Pathology:** Tooltip on a button whose label already says what it does. Tooltip that says "Click to delete" on a button labeled "Delete." Tooltip on an icon whose meaning is otherwise clear from context.

**Why LLMs ship this:** "Add tooltips for accessibility" learned without nuance.

**Prevention:**
- CONSTITUTION.md rule: "Tooltips are for: (a) unlabeled icon-only buttons where the meaning isn't established by context, (b) actions that have non-obvious side effects, (c) keyboard shortcut hints. Tooltips that restate a visible label are forbidden."

---

## 8. Color-only signaling

**Pathology:** Status indicated only by color (red = error, green = success). Required fields marked only by a red asterisk that's invisible to color-blind users. Selected state shown only by a tint.

**Why LLMs ship this:** Color is the easiest delta to add.

**Prevention:**
- CONSTITUTION.md rule: "Every meaningful state difference uses at least two cues from: color, icon, weight, position, label. Color alone is forbidden for status, selection, error, success, required-field marking."

---

## 9. Inconsistent destructive UI

**Pathology:** Sometimes "Delete" is a red button, sometimes a red link, sometimes lives in a "More" menu, sometimes triggers a confirmation. No consistent pattern.

**Why LLMs ship this:** Each screen generated in isolation; no shared pattern.

**Prevention:**
- CONSTITUTION.md rule: "Destructive actions have one consistent pattern across the app. Define once: where they live (in overflow menu, never primary), how they visually signal (color + icon + position), and their recovery model (undo toast, never confirmation modal except for irreversible)."

---

## 10. Disabled buttons without explanation

**Pathology:** Button is disabled. User has no idea why.

**Why LLMs ship this:** "Disable when invalid" learned without the corollary "and explain why."

**Prevention:**
- CONSTITUTION.md rule: "A disabled button must communicate why it's disabled — either through a co-located reason (inline error, helper text, missing-field count) or a tooltip on hover. Never a silent disabled state."

---

## 11. Placeholder-as-label

**Pathology:** Input fields use placeholder text as the label. Once user types, the label disappears.

**Why LLMs ship this:** Saves vertical space; looks clean.

**Prevention:**
- CONSTITUTION.md rule: "Every input has a persistent label visible during and after typing. Placeholders are for examples/hints only, never the field's identity."

---

## 12. Forms without dirty state

**Pathology:** User edits a field. Switches tabs. Returns. No indication that there are unsaved changes. Or: no warning when navigating away.

**Why LLMs ship this:** Dirty-state tracking is invisible plumbing; happy-path UI skips it.

**Prevention:**
- CONSTITUTION.md rule: "Forms with discrete save points show a dirty indicator (sync status, unsaved-changes badge) when state diverges from saved. Autosave forms show sync status passively (Saved · 2s ago)."

---

## 13. Modal stacking

**Pathology:** Opening a modal from inside a modal. User can't see what they're modifying. Esc closes the wrong one.

**Why LLMs ship this:** Modal is the default "show more content" pattern. Doesn't reason about layering.

**Prevention:**
- CONSTITUTION.md rule: "Modals do not stack. If a modal needs further interaction, the further interaction happens inline within the modal, or the original modal is replaced/transitioned to a new state. Side panels (sheets) can layer over a parent screen but never over each other."

---

## 14. Animation overuse

**Pathology:** Every state change has a 500ms animation. Bouncy springs. Long fade transitions. Animation as decoration.

**Why LLMs ship this:** "Polish" pattern-matched as "more animation."

**Prevention:**
- CONSTITUTION.md rule: "Motion is allowed only when it clarifies cause-effect or maintains spatial continuity. Durations 150–250ms. Ease-out for state changes. No spring/bounce/overshoot in functional UI. Animations longer than 300ms must be argued for."

---

## 15. Generic copy

**Pathology:** "Welcome to Dashboard!", "Great job!", "Oops!", "Something went wrong", "Are you sure you want to delete this item?"

**Why LLMs ship this:** Default copy patterns from generic SaaS templates.

**Prevention:**
- CONSTITUTION.md rule: "Copy voice: direct, past tense for confirmations ('Task created' not 'Task has been created'), no exclamation marks, no apologies ('Oops!'), no filler ('Are you sure you want to delete this task?' → 'Delete task?' or use undo instead). Error messages name the specific failure and the recovery action."

---

## 16. Pagination on short lists

**Pathology:** Paginating 12 items across 2 pages of 10. Or paginating with no count, so users can't tell if "next" has anything.

**Why LLMs ship this:** "Lists need pagination" without reasoning about list size.

**Prevention:**
- CONSTITUTION.md rule: "Pagination is for lists >50 items typical. Below that: render all, virtualize if needed for performance. When paginating, always show total count and current page range."

---

## 17. Search without affordances

**Pathology:** A search input with no recent searches, no filter suggestions, no example queries, no clear-button. Just an empty box.

**Why LLMs ship this:** Search treated as a primitive input, not a composite.

**Prevention:**
- CONSTITUTION.md rule: "Search inputs include: recent searches (when applicable), clear-affordance, contextual filter chips (when filters are available), example/placeholder hint relevant to the data being searched, keyboard shortcut hint if globally accessible."

---

## How the framework uses this file

- **`saas-ui-bootstrap` phase 8 (Forbidden)** loads this file and walks the user through each pathology, asking for their stance (full ban / case-by-case / acceptable). User answers become CONSTITUTION.md Forbidden rules with `Reason:` lines.
- **`saas-ui-composite`** (future) loads this file when generating composite SPECs; the Action Inventory and States sections are explicitly designed to prevent #1, #2, #4, #5, #10, #12.
- **`saas-ui-audit`** (future) lints implementations against these specifically.

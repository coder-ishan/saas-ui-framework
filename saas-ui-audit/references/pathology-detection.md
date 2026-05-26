# Pathology detection — how to detect each of the 17 LLM UI pathologies

> Reference for phase 2 (programmatic checks). For each pathology, how the audit detects it in artifacts and/or code.

The 17 pathologies are defined in `~/.claude/skills/saas-ui-bootstrap/references/common-llm-ui-mistakes.md`. This document covers detection only.

---

## #1 — Duplicate-purpose affordances

**Detection (artifact):**
Parse SPEC.md → Action Inventory. For each pair of actions:
- Share a verb? Check `Argued duplicates` for entry.
- Share a region with overlapping context? FAIL.

**Detection (code):**
Grep handler bindings. If two affordances (button + menu item, two buttons) in the same region wire to the same action handler without an `Argue:` annotation: FAIL.

**Severity:** CRITICAL

---

## #2 — Confirmation modal addiction

**Detection (artifact):**
Parse STATES.md and FLOWS.md. For each "Are you sure?" or confirmation modal:
- Is the action in CONSTITUTION → State Model → Recovery → Irreversible list?
- If not: FAIL (should be undo toast).

**Detection (code):**
Grep for `<ConfirmDialog>` or `<AlertDialog>` for actions that aren't in the Irreversible list. FAIL.

**Severity:** HIGH

---

## #3 — Decorative icons in dense UI

**Detection (artifact):**
Parse SPEC.md → Anatomy. For each labeled button:
- Does it have a leading icon that duplicates the label's meaning? (e.g., "+" icon + "Add row" label is OK; ambiguous icon + label is suspect.)
- For icon-only buttons: ensure tooltip provides label (Pathology #7 check overlaps).

**Severity:** MEDIUM

---

## #4 — Spinner overuse

**Detection (artifact):**
Parse STATES.md → loading-initial:
- Visual treatment should reference "skeleton matching final layout"
- If it says "spinner" or "loading indicator" generically: FAIL.

**Detection (code):**
Grep for `<Spinner />` or similar that's NOT inside a submit button. FAIL.

**Severity:** HIGH

---

## #5 — Generic empty states

**Detection (artifact):**
Parse STATES.md and EMPTY-STATES.md. For each empty state:
- Does the copy match `[What's absent] · [Why] · [Recovery]` structure?
- Grep for forbidden strings: "No data", "Nothing here", "Empty", "No results" (without context).

**Detection (code):**
Grep empty state JSX for "No data" string literals.

**Severity:** HIGH

---

## #6 — Toast spam

**Detection (artifact):**
Parse FLOWS.md. For each step that triggers a toast:
- Is the toast for: action whose source left viewport, long-delayed success, or undo? If yes: OK.
- Otherwise: FAIL (inline feedback preferred).

**Detection (code):**
Grep `toast(...)` calls in handlers where the source element is still visible at handler invocation time. FAIL.

**Severity:** MEDIUM

---

## #7 — Tooltip duplication

**Detection (artifact):**
Parse SPEC.md. For each tooltip:
- Is it on an unlabeled icon-only button? OK.
- Is it explaining a non-obvious side effect? OK.
- Is it a keyboard shortcut hint? OK.
- Otherwise (tooltip duplicates visible label)? FAIL.

**Severity:** LOW

---

## #8 — Color-only signaling

**Detection (artifact):**
Parse STATES.md → state visual treatments. For each state difference (selected, error, status):
- Does the visual treatment use ≥2 cues from: color, icon, weight, position, label?
- If only color: FAIL.

**Detection (code):**
Grep CSS classes / inline styles. For status indicators: check class names include both color-related AND icon-related or weight-related styling.

**Severity:** HIGH

---

## #9 — Inconsistent destructive UI

**Detection (artifact, project-wide):**
Across all composites/modules:
- Collect every destructive action (CONSTITUTION → Forbidden Patterns #9 references the consistent pattern).
- Compare visual variants. If different `Button` variants used for the same destructive semantic: FAIL.

**Detection (code, project-wide):**
Grep all destructive action handlers. Compare the component class names/variants used. If inconsistent: FAIL.

**Severity:** HIGH

---

## #10 — Disabled buttons without explanation

**Detection (artifact):**
Parse STATES.md → states with disabled affordances:
- Each disabled state must have a tooltip or co-located reason.
- If just `disabled={true}` without tooltip: FAIL.

**Detection (code):**
Grep `disabled` props on Button. Check for accompanying `title` or tooltip wrapper. If absent: FAIL.

**Severity:** MEDIUM

---

## #11 — Placeholder-as-label

**Detection (artifact):**
Parse SPEC.md → Anatomy → Input nodes:
- Each input must have a persistent label distinct from placeholder.

**Detection (code):**
Grep `<input placeholder="...">` or `<Input placeholder="...">`. Check for accompanying `<label>` or `aria-label`. If absent: FAIL.

**Severity:** HIGH

---

## #12 — Forms without dirty state

**Detection (artifact):**
Parse SPEC.md and FLOWS.md → explicit-save forms:
- Each must declare dirty indicator (Modified, Unsaved changes pill, etc.).
- Autosave forms must declare sync indicator.

**Detection (code):**
Grep for form components. Check for dirty-state tracking (`isDirty`, `hasChanges`, similar). If absent for explicit-save forms: FAIL.

**Severity:** MEDIUM

---

## #13 — Modal stacking

**Detection (artifact):**
Parse FLOWS.md for nested modal opens. If a modal opens another modal: FAIL.

**Detection (code):**
Grep for `<Modal>` or `<Dialog>` nested in another modal's children. FAIL.

**Severity:** HIGH

---

## #14 — Animation overuse

**Detection (artifact):**
Parse MOTION.md and STATES.md visual treatments:
- Durations > 300ms without `Reason:` line: FAIL.
- Spring/bounce in functional UI: FAIL.
- Fade-only transitions (no transform): FAIL.

**Detection (code):**
Grep CSS animation durations and framer-motion configs. Check against CONSTITUTION canonical.

**Severity:** MEDIUM

---

## #15 — Generic copy

**Detection (artifact):**
Parse EMPTY-STATES.md, STATES.md, FLOWS.md copy strings. Forbidden phrasings:
- "Oops"
- "Sorry"
- "Something went wrong"
- "Please"
- "Awesome!"
- "Great job!"
- Exclamation marks (except brand voice allowed in CONSTITUTION)

Each occurrence: FAIL.

**Detection (code):**
Grep string literals in components.

**Severity:** MEDIUM

---

## #16 — Pagination on short lists

**Detection (artifact):**
Parse FLOWS.md and composite SPECs. For each list/table:
- If pagination is present for lists <50 items typical: FAIL (virtualize or render all).

**Detection (code):**
Grep for Pagination component usage. If used on a route/composite that has <50 items typical: FAIL.

**Severity:** LOW

---

## #17 — Search without affordances

**Detection (artifact):**
Parse SPEC.md → Search component nodes:
- Search input must declare: recent searches presence (or absent with reason), clear button, filter chips (if relevant), context-relevant placeholder, keyboard shortcut hint (if global).
- If any required affordance missing: FAIL.

**Detection (code):**
Grep search inputs. Check for clear button, recent searches state, kbd shortcut binding.

**Severity:** MEDIUM

---

## Pathology severity scaling

Under SPRINT posture:
- All pathologies still STRICT (no softening)
- This is the audit's commitment: pathologies are always-on regardless of speed

Under CRAFT posture:
- Severity baselines apply
- Additional sub-checks may trigger (e.g., for Pathology #14, animation overuse, CRAFT also checks reduced-motion handling)

---

## Detection limitations

The audit's grep-based detection has false-positive risks:
- "No data" might appear in non-empty-state contexts (e.g., a banner). Audit surfaces with file:line; user can mark ARGUED-DEVIATION if the context is legitimate.
- Disabled buttons sometimes use a custom tooltip wrapper the audit doesn't know about. User-extensible: project's `.saas-ui/audit-config.md` can declare known tooltip wrappers.

When in doubt, the audit FLAGS rather than FAILs. The reviewer's judgment is the final gate.

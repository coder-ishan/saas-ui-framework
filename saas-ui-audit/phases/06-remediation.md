# Phase 6 — Remediation suggestions

**Purpose:** For each FAIL or REVIEW-NEEDED finding, suggest the specific action to fix it. Actionable, ordered by severity + ease.

**Produces:** The "Remediation summary" section of the scorecard.

---

## Per-finding remediation pattern

Every FAIL finding from phase 2 already has a `remediation` field. This phase compiles and orders them.

Remediations fall into categories:

### Category 1 — Skill rerun

The fix lives in re-running a saas-ui-* skill phase.

| Finding type | Skill to rerun |
|---|---|
| Atom invented in SPEC | `/saas-ui-composite <name>` phase 3 (anatomy/atoms) |
| Action Inventory duplicate without argument | `/saas-ui-composite <name>` phase 2 |
| Missing state in STATES.md | `/saas-ui-composite <name>` phase 4 |
| Generic empty state copy in module | `/saas-ui-module <name>` phase 8 |
| Missing Peak-End identification | `/saas-ui-module <name>` phase 4 |
| Posture mismatch in IMPLEMENTATION.md | `/saas-ui-composite <name>` phase 11 |
| Posture mismatch in BUILD-ORDER.md | `/saas-ui-module <name>` phase 10 |
| Stale posture | `/saas-ui-bootstrap` phase 10 |
| Atom gap in INVENTORY blocks composite | `/saas-ui-bootstrap` phase 0 (revisit design system) |

### Category 2 — Direct edit

The fix is a specific edit to a file.

Format: "Edit `<file>:<line range>` → <what to change>"

Examples:
- "Edit `composites/data-table/MOTION.md` → add `Reason:` line under each override"
- "Edit `modules/inbox/EMPTY-STATES.md` → replace 'No data' with `[What's absent] · [Why] · [Recovery]` structure (line 14)"

### Category 3 — Code edit (post-implementation mode)

For code-mode findings:

Format: "In `<source file>:<line>` — <change>"

Examples:
- "In `app/(dashboard)/customers/page.tsx:42` — wrap `<CustomerTable>` in `<Suspense fallback={<TableSkeleton />}>` per SPEC.md → Rendering"
- "In `app/actions/delete-customer.ts:8` — call `revalidateTag('table:customers')` after the delete completes"

### Category 4 — Design-system addition

When the fix requires adding a primitive to the design system.

Format: "Add `<atom name>` to INVENTORY.md → Atoms; build the component in `<expected path>` per <reference SPEC>"

This is design-system-track work, separate from the saas-ui-* framework but surfaced here so the team knows.

### Category 5 — User decision

For REVIEW-REQUIRED items, the remediation is a question:

Format: "Decide: <question>. After deciding, rerun audit with `--review-completed`."

---

## Ordering

The remediation list orders findings:

1. **By severity (CRITICAL > HIGH > MEDIUM > LOW)**
2. **Within severity, by ease (5 min > 15 min > 1 hour > multi-hour)**
3. **Skill reruns before direct edits** (skill reruns are atomic; direct edits can introduce new drift)

The first item is "what to fix now." The remaining items are "what to fix this session" or "what to track."

---

## "What to ship" guidance

After the remediation list, the scorecard ends with a recommendation:

```markdown
### Ship guidance

<Verdict-specific recommendation>
```

For verdict `SHIP`:
> "No blockers. Findings noted are LOW or PASS-WITH-NOTE under current posture. Cleared to ship at <POSTURE> quality."

For verdict `NEEDS-REVIEW`:
> "No CRITICAL or HIGH findings, but <N> items require manual review before clearing. Complete the Review-required section and rerun audit."

For verdict `NEEDS-FIXES`:
> "<N> CRITICAL and <N> HIGH findings. Address in order:
> 1. <#1 remediation>
> 2. <#2 remediation>
> 3. <#3 remediation>
>
> Rerun audit after fixes."

---

## Console summary

In addition to the scorecard file, print a brief console summary so the user doesn't need to open the file:

```text
Audit complete: <target> @ <posture>

Verdict: NEEDS-FIXES
- 2 CRITICAL: atom invention (DataTable), duplicate-button (Toolbar)
- 1 HIGH: missing state-coverage in STATES.md

Top remediations:
1. /saas-ui-composite data-table  (phase 3 — atoms)
2. /saas-ui-composite data-table  (phase 2 — action inventory)
3. Edit composites/data-table/STATES.md → add long-content entry

Full scorecard: .saas-ui/audits/composite/data-table/2026-05-26-1430.md
```

---

## What NOT to do in phase 6

- **Don't auto-execute remediations.** Even when the fix is "rerun /saas-ui-composite <name>", the user decides when.
- **Don't bundle unrelated fixes.** Each finding gets its own remediation entry.
- **Don't suggest fixes that the audit can't verify after.** If a fix can't be re-audited, the audit can't track its resolution. Surface that limitation.

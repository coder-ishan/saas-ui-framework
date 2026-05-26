# Phase 2 — Programmatic checks

**Purpose:** Run every check whose verification can be done by parsing artifacts or scanning code. Produces a findings list with PASS / FAIL / SKIP.

**Produces:** In-memory findings list.

---

## Per-check execution pattern

For each check in phase 1's list:

```text
{
  check_id: "atom-invention",
  target: "composite/data-table/SPEC.md",
  result: PASS | FAIL | SKIP,
  severity: CRITICAL | HIGH | MEDIUM | LOW,
  rule: "INVENTORY.md → Constraints: all composite SPECs must reference atoms by exact name",
  finding: "If FAIL: <one-line description of violation, with file:line if known>",
  remediation: "If FAIL: <how to fix — typically a skill rerun or edit>",
  evidence: "<the actual quote/match that triggered the result>",
}
```

---

## Core programmatic checks

### Atom invention sweep

For target composite or module artifacts, extract every component name referenced. Compare against INVENTORY.md → Atoms (available) + Molecules (available).

- **PASS:** every reference matches an inventory entry exactly.
- **FAIL (CRITICAL):** reference uses a component not in inventory.
  - Remediation: "Add `<component>` to INVENTORY.md → Atoms or Molecules; OR substitute with `<inventory atom>` and document the substitution."

Implementation: grep for `\b[A-Z][a-zA-Z]+\b` in SPEC.md (matching React-style component names), filter against INVENTORY names, surface anything that doesn't match.

### Action Inventory contract

Parse SPEC.md → Action Inventory section. For each action:

1. Confirm exactly one primary affordance + region.
2. If two actions share a verb, confirm `Argued duplicates` entry exists.
3. If an action appears in two regions, confirm cross-region justification.

- **PASS:** all constraints met.
- **FAIL (CRITICAL):** duplicate detected without argument.
  - Severity is CRITICAL because this is Pathology #1, the most common LLM UI failure.

### Primitive Gaps resolution

Parse SPEC.md → Primitive Gaps section. Each gap row must have a Resolution column with `Add to design system: …`, `Defer: …`, or `Substitute: …`.

- **PASS:** all gaps have resolutions.
- **FAIL (HIGH):** gap has no resolution OR resolution is "TBD".

### State enumeration completeness

Parse STATES.md. Confirm all 13 canonical states have entries (idle, hover, focus, selected, active-edit, loading-initial, loading-mutation, empty true-empty, empty filter-empty, error recoverable, error terminal, partial-permission, offline, long-content).

- **PASS:** all states present with required fields (When, Visual, Allowed actions, Transitions).
- **FAIL (HIGH):** state missing or marked TBD.
- **SKIP-EXPECTED:** state genuinely N/A with reason (e.g., "active-edit: N/A — composite is read-only").

### Reason: line presence

For every override in CONSTITUTION-shaped artifacts (MOTION.md overrides, copy overrides, etc.), confirm a `Reason:` line.

- **FAIL (HIGH):** override without Reason: line.

### Pathology sweeps

Run all 17 per `references/pathology-detection.md`. Each gets its own check ID. Most are programmatic; a few require manual review (surfaced in phase 3).

Examples:
- **Pathology #5 (Generic empty states):** grep empty-state copy for "No data", "Nothing here", "Empty". → FAIL with severity HIGH.
- **Pathology #1 (Duplicate-purpose affordances):** covered by Action Inventory contract above.
- **Pathology #11 (Placeholder-as-label):** for code mode, grep for `<input placeholder=` without an associated `<label>`.

### Drift detection

Cross-check artifact references against source artifacts:

- SPEC.md references `Button (primary)` → INVENTORY.md → Atoms → Button has variant `primary`? If not, FAIL.
- MOTION.md says "modal: per CONSTITUTION canonical pattern" → CONSTITUTION → Motion → Canonical patterns has a "modal" entry? If not, FAIL.
- VARIANTS.md references a module → that module exists in MODULES.md? If not, FAIL.

### Posture-consistency check

If IMPLEMENTATION.md exists for a composite, confirm its declared posture matches EXECUTION-POSTURE.md.

- **WARN (MEDIUM):** posture mismatch — IMPLEMENTATION computed under a different posture than currently committed.
  - Remediation: "Re-run `saas-ui-composite` phase 11 to recompute IMPLEMENTATION.md."

---

## Code-mode programmatic checks (post-implementation)

Additional checks when target is `code:<path>`:

### Imports from inventory

Parse import statements. For each component imported:
- If it's a UI component (CamelCase, looks like a React component) AND not from a node_modules path AND not in INVENTORY: FAIL CRITICAL (potential atom invention).

### useOptimistic + rollback presence

For each composite implementation file, grep for `useOptimistic`. If present:
- Confirm an error path renders a toast (search for toast invocation in catch / error branch).
- If no rollback toast: FAIL HIGH — silent failure pattern (per `pr-review-toolkit:silent-failure-hunter` aligned rule).

### 'use client' boundary

Scan for files with `'use client'`. Confirm boundary placement matches SPEC.md → Rendering.
- If SPEC says a node is Server but the file is Client: FAIL HIGH.
- If 'use client' is at page top instead of leaf: FAIL MEDIUM (suboptimal boundary).

### Suspense boundary

Confirm Suspense boundary exists per SPEC.md → Rendering → Suspense topology.
- Grep for `<Suspense fallback=` in expected files. If absent: FAIL HIGH.

### Server Action conventions

For mutations, confirm:
- Server Action declared with `'use server'`
- `revalidateTag` called per SPEC.md → Caching tag namespace
- If composite SPEC says optimistic, confirm `useOptimistic` call site exists.

---

## Output format per finding

```markdown
### [CHECK-ID] <Check name>

**Result:** PASS | FAIL | SKIP
**Severity:** CRITICAL | HIGH | MEDIUM | LOW
**Rule:** <source rule reference — CONSTITUTION § X / INVENTORY constraint Y / Pathology #N>

<If FAIL:>

**Finding:** <one-line description>
**Evidence:** `<quote / file:line>`
**Remediation:** <actionable next step — usually a skill rerun or specific edit>
```

Findings are accumulated into the in-memory list; phase 5 composes the scorecard from them.

---

## What NOT to do in phase 2

- **Don't run checks that require subjective judgment.** Those go in phase 3 (manual-check surfacing).
- **Don't auto-fix.** Even if a fix is obvious, the audit only reports — the user's fix path is via the appropriate skill rerun.
- **Don't suppress findings.** Every check produces a result (PASS / FAIL / SKIP). Silent skips defeat the audit.
- **Don't echo passing findings in detail.** PASS findings collapse to count in the scorecard; FAIL and SKIP findings get full detail.

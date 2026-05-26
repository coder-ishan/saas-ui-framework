# Audit rule catalog

> The canonical list of every rule the audit checks. Each rule has a stable `check_id`, a category, a default severity, a strictness flag, and pseudo-code for verification.

---

## Rule structure

```text
- check_id: <stable identifier>
- category: functional | constitution | principle | composite-law | module-law | pathology | drift
- default_severity: CRITICAL | HIGH | MEDIUM | LOW
- strictness: always | posture-scaled
- target_types: [<list of target types this rule applies to>]
- description: <one-line>
- verification: <pseudo-code or description>
- remediation: <what to do if FAIL>
- references: <CONSTITUTION § X | INVENTORY constraint | Pathology #N>
```

---

## Functional category (always strict)

### atom-invention

- **Category:** functional
- **Severity:** CRITICAL
- **Strictness:** always
- **Targets:** composite/*, module/*, code:*
- **Description:** Every component reference must match an INVENTORY.md atom or molecule by exact name.
- **Verification:**
  ```text
  For each component name extracted from target:
    if not in INVENTORY.atoms + INVENTORY.molecules:
      FAIL
  ```
- **Remediation:** Add to INVENTORY (via bootstrap phase 0) OR substitute with existing atom and document.
- **References:** INVENTORY.md → Constraints for downstream SPECs (hard rule)

### action-inventory-contract

- **Category:** functional
- **Severity:** CRITICAL
- **Strictness:** always
- **Targets:** composite/*
- **Description:** Every user-meaningful action has exactly ONE primary affordance per region. Duplicates require `Argue:` line.
- **Verification:**
  ```text
  Parse SPEC.md → Action Inventory.
  For each pair of actions in the table:
    if same verb AND same region:
      if no Argued duplicates entry:
        FAIL
  ```
- **Remediation:** `/saas-ui-composite <name>` phase 2.
- **References:** CONSTITUTION → Action Affordances; Pathology #1.

### primitive-gaps-resolution

- **Category:** functional
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** composite/*
- **Description:** Every gap in Primitive Gaps must have a Resolution: Add / Defer / Substitute.
- **Verification:**
  ```text
  Parse SPEC.md → Primitive Gaps.
  For each gap:
    if Resolution is empty or "TBD":
      FAIL
  ```

### state-enumeration-complete

- **Category:** functional
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** composite/*
- **Description:** All 13 canonical states present in STATES.md.
- **Verification:**
  ```text
  Parse STATES.md sections.
  Required states: [idle, hover, focus, selected, active-edit, loading-initial,
                    loading-mutation, empty-true-empty, empty-filter-empty,
                    error-recoverable, error-terminal, partial-permission,
                    offline, long-content]
  For each required state:
    if not present OR marked TBD:
      FAIL
  ```

### reason-line-presence

- **Category:** functional
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** composite/*, module/*
- **Description:** Every override has a Reason: line.
- **Verification:**
  ```text
  Grep for "Override:" without nearby "Reason:" in MOTION.md, COPY-deltas, etc.
  ```

---

## CONSTITUTION category (always strict)

### motion-canonical-reference

- **Category:** constitution
- **Severity:** MEDIUM
- **Strictness:** always
- **Targets:** composite/*
- **Description:** Composite MOTION.md must reference CONSTITUTION canonical patterns, not restate values.
- **Verification:**
  ```text
  In composite MOTION.md:
    if duration value (e.g., "200ms") appears for a canonical pattern (modal, drawer, popover, toast):
      FAIL (should reference, not restate)
  ```

### copy-structure

- **Category:** constitution
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** composite/*, module/*
- **Description:** All user-visible copy follows CONSTITUTION → Copy structure.
- **Verification:**
  ```text
  For each copy string in EMPTY-STATES.md and STATES.md:
    if it's an error: check [What] · [Why] · [Recovery] structure
    if it's an empty state: check [Absent] · [Why] · [Recovery] structure
    if it's a confirmation: check [Action past tense] · [Subject] · [Undo if applicable]
  ```

---

## Pathology category (always strict)

All 17 pathologies. See `references/pathology-detection.md` for verification details. Severities:

| # | Pathology | Severity |
|---|---|---|
| 1 | Duplicate-purpose affordances | CRITICAL |
| 2 | Confirmation modal addiction | HIGH |
| 3 | Decorative icons in dense UI | MEDIUM |
| 4 | Spinner overuse | HIGH |
| 5 | Generic empty states | HIGH |
| 6 | Toast spam | MEDIUM |
| 7 | Tooltip duplication | LOW |
| 8 | Color-only signaling | HIGH |
| 9 | Inconsistent destructive UI | HIGH |
| 10 | Disabled buttons without explanation | MEDIUM |
| 11 | Placeholder-as-label | HIGH |
| 12 | Forms without dirty state | MEDIUM |
| 13 | Modal stacking | HIGH |
| 14 | Animation overuse | MEDIUM |
| 15 | Generic copy | MEDIUM |
| 16 | Pagination on short lists | LOW |
| 17 | Search without affordances | MEDIUM |

---

## Composite-level Laws category (posture-scaled)

For each of the 11 composite-level laws (Fitts, Miller, Hick, Von Restorff, Working Memory, Chunking, 5 Gestalt, Prägnanz):

- **Category:** composite-law
- **Severity:** MEDIUM (default)
- **Strictness:** posture-scaled
- **Targets:** composite/*
- **Verification:** parse LAWS.md → look for entry per law; surface manual review questions per `references/posture-strictness.md`.

---

## Module-level Laws category (posture-scaled)

For each of the 6 module-level laws (Peak-End, Goal-Gradient, Zeigarnik, Serial Position, Flow, Selective Attention):

- **Category:** module-law
- **Severity:** MEDIUM (default)
- **Strictness:** posture-scaled
- **Targets:** module/*
- **Verification:** parse LAWS.md → look for entry per law.

---

## Drift category (always reported)

### atom-reference-drift

- **Category:** drift
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** composite/*, module/*
- **Description:** SPEC references atom variant that doesn't exist in INVENTORY.
- **Verification:**
  ```text
  For each "Button (primary)" style reference:
    look up Button in INVENTORY
    if "primary" not in Button.variants:
      FAIL
  ```

### posture-mismatch

- **Category:** drift
- **Severity:** MEDIUM
- **Strictness:** always
- **Targets:** composite/*, module/*
- **Description:** IMPLEMENTATION.md or BUILD-ORDER.md was computed under a different posture than currently committed.
- **Verification:**
  ```text
  Compare artifact's posture field to EXECUTION-POSTURE.md.
  If different: FAIL.
  ```
- **Remediation:** Re-run the posture-consuming phase.

### state-claimed-but-missing

- **Category:** drift
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** project-wide
- **Description:** STATE.md claims a composite/module is spec'd/designed but the file is missing.
- **Verification:**
  ```text
  For each entry in STATE.md showing "spec'd" or "designed":
    check filesystem for corresponding file.
    if missing: FAIL (data loss risk).
  ```

---

## Code-mode-only checks (post-implementation)

### import-from-inventory

- **Category:** functional
- **Severity:** CRITICAL
- **Strictness:** always
- **Targets:** code:*
- **Description:** UI components imported must come from INVENTORY paths.

### useoptimistic-rollback

- **Category:** functional
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** code:*
- **Description:** `useOptimistic` must pair with a rollback toast in the error path.

### use-client-boundary

- **Category:** constitution
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** code:*
- **Description:** 'use client' is on the smallest leaf, not the whole component.

### suspense-presence

- **Category:** constitution
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** code:*
- **Description:** Suspense boundary present where SPEC.md → Rendering says it should be.

### server-action-revalidate

- **Category:** functional
- **Severity:** HIGH
- **Strictness:** always
- **Targets:** code:*
- **Description:** Server Actions that mutate must call `revalidateTag` with the namespace from SPEC.

---

## Total rule count

- Functional: 5 + 5 code-mode = 10
- CONSTITUTION: 2
- Pathologies: 17
- Composite-laws: 11
- Module-laws: 6
- Drift: 3
- **Total: ~49 rules**

Per audit run, typical count is ~25 (only rules applicable to target). Project-wide audits run more.

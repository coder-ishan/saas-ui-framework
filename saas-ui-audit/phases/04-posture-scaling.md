# Phase 4 — Posture scaling

**Purpose:** Apply posture-specific strictness to findings. Same artifact, different gate thresholds depending on whether the team is shipping SPRINT, BALANCED, or CRAFT.

**Produces:** Findings list adjusted with posture-weighted severity.

---

## The rule

Reference: `references/posture-strictness.md`.

Findings have two strictness flags:
- `always` — passes/fails are absolute. Pathologies, atom invention, duplicate buttons, generic empty states.
- `posture-scaled` — passes/fails depend on posture. Polish-tier rules (Peak-End polish, composite-level Gestalt depth, motion calibration).

This phase walks every `posture-scaled` finding and adjusts:
- Under SPRINT: many MEDIUM findings downgrade to LOW or become PASS-WITH-NOTE
- Under BALANCED: findings retain their default severity
- Under CRAFT: many MEDIUM findings upgrade to HIGH; LOW findings stay but get flagged for review

---

## The scaling matrix

```text
                  SPRINT       BALANCED      CRAFT
─────────────────────────────────────────────────────
CRITICAL          CRITICAL     CRITICAL      CRITICAL    (functional always)
HIGH              HIGH         HIGH          CRITICAL    (constitution always)
MEDIUM            LOW          MEDIUM        HIGH        (polish-tier)
LOW               PASS-WITH-NOTE  LOW         MEDIUM     (suggestion-tier)
```

CRITICAL never softens. HIGH never softens to less than HIGH under any non-CRAFT posture.

---

## Per-finding scaling

For each finding produced in phases 2 + 3:

1. Read `category` field.
2. If category is `functional` or `pathology`: severity unchanged.
3. If category is `constitution`: severity unchanged (CONSTITUTION rules are explicit team commitments).
4. If category is `composite-law`, `module-law`, or `principle`:
   - Look up scaling for current posture
   - Adjust severity
5. If category is `drift`: severity unchanged (drift is always reported).

After scaling, finding includes:
- `original_severity` (what phase 2/3 emitted)
- `scaled_severity` (after posture adjustment)
- `posture` (the posture used for scaling)

---

## Posture-specific overrides

### SPRINT-specific

Under SPRINT, additional findings auto-downgrade:

- Motion polish findings (composite-specific patterns not yet promoted to CONSTITUTION): MEDIUM → PASS-WITH-NOTE
- Per-module Peak-End polish quality (subjective): REVIEW-REQUIRED but doesn't gate
- Cross-variant consistency depth: MEDIUM → LOW (variants are usually deferred under SPRINT)
- Edge case completeness (categories from EDGE-CASES.md): MEDIUM unless category is data-shape (which stays HIGH; data-shape is functional)

### CRAFT-specific

Under CRAFT, additional findings auto-upgrade:

- Cross-variant consistency: any MEDIUM finding upgrades to HIGH
- Voice consistency: any subjective inconsistency triggers manual review even if individual files pass
- Performance: bundle size, render performance, virtualization thresholds enter the check set (these are no-ops under SPRINT/BALANCED)
- Accessibility: ARIA + screen reader walkthrough required (REVIEW-REQUIRED, ship-blocking under CRAFT)

### BALANCED — the default

Phase 2/3 severity passes through unchanged.

---

## Posture mismatch findings

If the audit detects posture mismatch between artifacts:
- A composite's IMPLEMENTATION.md says "BALANCED" but EXECUTION-POSTURE.md says "CRAFT"
- A module's BUILD-ORDER.md says "SPRINT" but EXECUTION-POSTURE.md says "BALANCED"

Severity: MEDIUM (drift category)
Remediation: re-run the affected skill's posture-consuming phase (`/saas-ui-composite <name>` for IMPLEMENTATION; `/saas-ui-module <name>` for BUILD-ORDER).

---

## Output format

After scaling, the findings list has:

```text
{
  check_id: "composite-law-fitts",
  original_severity: "MEDIUM",
  scaled_severity: "LOW",  // SPRINT downgrade
  posture: "SPRINT",
  posture_note: "Polish-tier rule; gate eased under SPRINT.",
  result: ...,
  rule: ...,
  finding: ...,
  remediation: ...,
}
```

The `posture_note` field is shown in the scorecard so the reader understands why severity differs from default.

---

## What NOT to do in phase 4

- **Don't scale CRITICAL findings.** Functional rules always gate at CRITICAL.
- **Don't scale CONSTITUTION rules.** The CONSTITUTION is the team's commitment — its rules don't soften by posture.
- **Don't hide ARGUED-DEVIATION findings.** Even if argued, they're recorded. The audit doesn't hide deviations; it ensures they have rationale.
- **Don't double-scale.** Each finding is scaled once.

---

## Special case: project-wide audits

For project-wide audits, posture scaling applies per-artifact (each artifact may have been computed at a different posture if posture has shifted over time). The scorecard groups findings by artifact and shows the posture used for each.

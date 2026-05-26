# Posture strictness — how each posture scales the audit gate

> Reference for phase 4 (posture scaling). The audit reads EXECUTION-POSTURE.md and scales findings.

---

## The principle

The same artifact under SPRINT posture and CRAFT posture should ship at *the same functional correctness* but at *different polish quality*. The audit reflects this:

- **Functional rules** — atom invention, duplicate buttons, generic empty states, pathologies — are always-on. Posture cannot soften these.
- **Polish-tier rules** — Peak-End polish quality, composite-law verification, motion calibration depth, cross-variant consistency — scale with posture.

This document specifies which rules scale and how.

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

---

## SPRINT scaling

### Always-strict (no softening)

- All Pathology checks (#1–#17)
- Atom invention
- Action Inventory contract
- Primitive Gaps resolution
- State enumeration completeness (the 5 MVP states: idle, loading-initial, error-recoverable, empty-true-empty, success exit)
- Reason: line presence
- Copy structure (CONSTITUTION → Copy)
- Atom reference drift
- Posture mismatch
- All code-mode functional checks (import-from-inventory, useoptimistic-rollback, suspense-presence, server-action-revalidate)

### Softened to LOW or PASS-WITH-NOTE

- State enumeration completeness for the 8 deferred states (hover, focus, selected, active-edit, loading-mutation, empty-filter-empty, partial-permission, offline, long-content) → PASS-WITH-NOTE if missing
- Composite-level Laws checks (LAWS.md verification depth) → MEDIUM → LOW
- Module-level Peak-End polish quality → MEDIUM → LOW (still surfaces as REVIEW-NEEDED but doesn't gate)
- Motion override calibration depth → MEDIUM → LOW
- Variants section completeness → MEDIUM → PASS-WITH-NOTE (variants typically deferred under SPRINT)
- Cross-variant consistency → MEDIUM → PASS-WITH-NOTE

### Disabled under SPRINT

- Performance checks (bundle size, render performance) — not in scope until BALANCED+
- Accessibility deep-dive (ARIA + screen reader walkthrough) — basic ARIA must pass (HIGH), but deep walkthrough is CRAFT-only
- Reduced motion handling — not gated under SPRINT (still warned)
- High-contrast / forced-colors mode — not gated under SPRINT

### Ship gate under SPRINT

Verdict `SHIP` when:
- 0 CRITICAL findings
- 0 HIGH findings (excluding posture-softened ones)
- 0 always-strict drift findings
- 0 always-strict pathology findings

Argued deviations don't count as findings.

---

## BALANCED scaling

### Default — phase 2/3 severities pass through

All checks run at their default severity. No softening, no upgrade.

### Always-strict

- All SPRINT always-strict rules
- PLUS:
  - State enumeration completeness for ALL 13 states (no softening)
  - Composite-level Laws checks (HIGH severity, LAWS.md must have all 11 entries with verification)
  - Module-level Laws checks (HIGH severity, all 6 laws with entries)
  - Variants section presence for canonical composites
  - Cross-variant consistency check (MEDIUM severity)
  - Module-specific copy in EMPTY-STATES.md (HIGH severity if generic)

### Surfaced as REVIEW-NEEDED

- Peak-End polish proportionality
- Voice consistency
- Gestalt subjective checks

### Ship gate under BALANCED

Verdict `SHIP` when:
- 0 CRITICAL
- 0 HIGH
- ≤ 3 MEDIUM (each requires argument)
- All REVIEW-NEEDED items resolved with PASS or ARGUED-DEVIATION

---

## CRAFT scaling

### Strictest — many upgrades

- All BALANCED always-strict, but:
  - MEDIUM severity upgrades to HIGH where polish matters
  - Composite-level Laws: ALL 11 must pass verification (HIGH severity)
  - Module-level Laws: ALL 6 with concrete entries (HIGH severity)
  - Cross-variant consistency: HIGH severity (was MEDIUM under BALANCED)
  - Voice consistency: HIGH severity (REVIEW-NEEDED still, but ship-blocking)

### Additional checks enabled

- **Performance checks** — bundle size budget per composite; render performance under load; virtualization presence where row counts justify
- **Accessibility deep-dive** — full ARIA + screen reader walkthrough; keyboard-only flow verification; reduced-motion + high-contrast + RTL + print all addressed
- **Motion calibration depth** — per-state durations tuned (e.g., selection toggle faster than CONSTITUTION default); composite-specific patterns either promoted to CONSTITUTION or argued
- **Edge case coverage** — every category from EDGE-CASES.md addressed (no N/A without reason)

### Ship gate under CRAFT

Verdict `SHIP` when:
- 0 CRITICAL
- 0 HIGH
- 0 MEDIUM (every finding either resolved or ARGUED-DEVIATION)
- All REVIEW-NEEDED items resolved
- All accessibility + performance checks pass

CRAFT is the strictest gate. Argued deviations require strong justification.

---

## Edge cases and exceptions

### When posture is not committed

Default to BALANCED strictness. The scorecard notes: "Posture not committed; using BALANCED defaults. Run /saas-ui-bootstrap phase 10 to commit a posture."

### When posture is stale

Apply the committed posture's strictness AND surface a posture-staleness warning. The audit doesn't change strictness just because posture is old — that's the user's call to re-commit.

### Mixed-posture artifacts

If an artifact's IMPLEMENTATION.md or BUILD-ORDER.md was computed under a different posture than currently committed:
- The audit flags this (posture-mismatch drift finding, MEDIUM severity)
- The audit uses *the currently committed posture* for strictness, not the artifact's posture
- Recommendation: re-run the artifact's posture-consuming phase

### Project-wide audits

Each artifact's findings are scaled per its own context (composite vs module), but the overall verdict uses the currently committed posture.

---

## Strictness summary cheat

| Concern | SPRINT | BALANCED | CRAFT |
|---|---|---|---|
| Pathologies | strict | strict | strict |
| Atom invention | strict | strict | strict |
| Duplicate buttons | strict | strict | strict |
| Required states (5 MVP) | strict | strict | strict |
| Required states (8 V1) | softened | strict | strict |
| CONSTITUTION rules | strict | strict | strict (HIGH→CRITICAL) |
| Composite laws | LOW | MEDIUM | HIGH |
| Module laws | LOW | MEDIUM | HIGH |
| Cross-variant consistency | PASS-WITH-NOTE | MEDIUM | HIGH |
| Performance checks | disabled | basic | full |
| Accessibility deep-dive | basic | basic | full |
| Reduced motion | warn | warn | required |
| Voice consistency | LOW | REVIEW | HIGH-REVIEW |
| Verdict SHIP threshold | no CRITICAL + HIGH | no CRITICAL + HIGH + ≤3 MEDIUM | no CRITICAL + HIGH + 0 MEDIUM |

---

## Same audit, different verdicts — example

A composite with these findings:
- 0 CRITICAL
- 1 HIGH: motion override without Reason:
- 2 MEDIUM: Fitts verification missing, Cross-variant consistency suggestion
- 1 LOW: Pagination on a list that might have <50 items

Verdicts:
- **SPRINT:** SHIP (HIGH addressed; MEDIUM/LOW softened to LOW/PASS-WITH-NOTE)
- **BALANCED:** NEEDS-FIXES (1 HIGH blocks)
- **CRAFT:** NEEDS-FIXES (1 HIGH; the 2 MEDIUM upgrade to HIGH)

Same artifact, three different gate outcomes. This is the design intent: ship at the right quality for the current posture, not over- or under-shoot.

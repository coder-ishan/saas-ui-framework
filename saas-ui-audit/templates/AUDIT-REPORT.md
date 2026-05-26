# Audit — <target>

**Target:** <composite/<name> | module/<name> | code:<path> | project-wide>
**Mode:** pre-implementation | post-implementation | project-wide
**Posture:** <current posture>
**Run at:** <YYYY-MM-DD HH:MM>
**Audit version:** saas-ui-audit v<version>

---

## Summary

| Severity | Count |
|---|---|
| CRITICAL | <N> |
| HIGH | <N> |
| MEDIUM | <N> |
| LOW | <N> |
| REVIEW-NEEDED | <N> |
| PASS | <N> |
| SKIP | <N> |

**Verdict:** SHIP | NEEDS-FIXES | NEEDS-REVIEW

**Top blockers:**
1. <highest severity finding, one-line>
2. <next>
3. <next>

---

## Functional (always strict)

### [CHECK-ID] <Check name>

**Result:** FAIL
**Severity:** CRITICAL
**Rule:** <source>
**Finding:** <one-line description>
**Evidence:** `<file:line or quote>`
**Remediation:** <skill rerun or specific edit>

---

## CONSTITUTION

<repeat per finding>

---

## Composite-level Laws

<repeat per finding>

---

## Module-level Laws

<repeat per finding>

---

## Pathologies (17)

For each pathology check that triggered:

### [PATHOLOGY-N] <Name>

**Result:** FAIL
**Severity:** <per pathology — typically HIGH>
**Rule:** Pathology #N from common-llm-ui-mistakes.md
**Finding:** ...
**Evidence:** ...
**Remediation:** ...

---

## Drift

<cross-artifact inconsistencies>

---

## Review required

These checks require human judgment. Mark each as PASS / FAIL / ARGUED-DEVIATION after review.

| # | Check | Question | Where to look | Status |
|---|---|---|---|---|
| 1 | <name> | <question> | <file/section> | [ ] |
| ... | | | | |

To complete the audit:
1. Review each item above
2. Mark `[ ]` as `[PASS]`, `[FAIL]`, or `[ARGUED]`
3. Save this file
4. Rerun `/saas-ui-audit --review-completed <this-file-path>`

---

## Passing checks (<count>)

<collapsible list of check IDs that passed — no detail; just confirmation>

---

## Skipped checks (<count>)

| Check | Reason for skip |
|---|---|
| <id> | <reason — anchor missing, target file absent, etc.> |

---

## Remediation summary

Ordered by severity + ease of fix:

1. **<finding name>** — <action> (CRITICAL, ~<estimate>)
2. **<finding name>** — <action> (HIGH, ~<estimate>)
3. **<finding name>** — <action> (MEDIUM, ~<estimate>)

---

## Compared to previous audit (<date>, if exists)

- New findings: <N>
- Resolved findings: <N>
- Severity trend: <improving | worsening | stable>

---

## Ship guidance

<For SHIP: "No blockers. Cleared to ship at <POSTURE> quality.">
<For NEEDS-REVIEW: "Complete the Review-required section and rerun audit.">
<For NEEDS-FIXES: "Address CRITICAL and HIGH findings in order. Rerun audit after fixes.">

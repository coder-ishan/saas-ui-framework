# Phase 5 — Compose scorecard

**Purpose:** Write the findings to `.saas-ui/audits/<target>/<timestamp>.md` using `templates/AUDIT-REPORT.md` as the format guide.

**Produces:** `.saas-ui/audits/<target>/<YYYY-MM-DD-HHMM>.md` plus a symlink/copy `latest.md`.

---

## Output layout

```text
.saas-ui/
  audits/
    composite/data-table/
      2026-05-26-1430.md
      2026-05-26-1517.md
      latest.md → 2026-05-26-1517.md
    module/inbox/
      2026-05-25-0900.md
      latest.md → 2026-05-25-0900.md
    project-wide/
      2026-05-26-1600.md
      latest.md → ...
```

`latest.md` is a copy (not a true symlink — portability across Windows) of the most recent audit for the target.

---

## Scorecard structure

Use `templates/AUDIT-REPORT.md` as the format. The scorecard has these sections:

### Header

```markdown
# Audit — <target>

**Target:** <composite/<name> | module/<name> | code:<path> | project-wide>
**Mode:** pre-implementation | post-implementation | project-wide
**Posture:** <current posture from EXECUTION-POSTURE.md>
**Run at:** <YYYY-MM-DD HH:MM>
**Audit version:** saas-ui-audit v<version>
```

### Summary

```markdown
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

**Verdict:** <SHIP | NEEDS-FIXES | NEEDS-REVIEW>

- **SHIP** if: 0 CRITICAL, 0 HIGH, 0 REVIEW-NEEDED-CRITICAL
- **NEEDS-REVIEW** if: 0 CRITICAL, 0 HIGH, but REVIEW-NEEDED items present
- **NEEDS-FIXES** if: any CRITICAL or HIGH

**Top blockers:**
1. <highest severity finding with one-line description>
2. <next>
3. <next>
```

### Findings by category

For each category that has findings, a subsection:

```markdown
## Functional (always strict)

### [CHECK-ID] <Check name>

**Result:** FAIL
**Severity:** CRITICAL
**Rule:** <source>
**Finding:** ...
**Evidence:** ...
**Remediation:** ...
```

Repeat for all categories: Functional, CONSTITUTION, Composite-laws, Module-laws, Pathologies, Drift, Posture-scaling, Review-required.

### Passing checks summary

```markdown
## Passing checks (<count>)

<collapsible list of check IDs that passed — no detail; just confirmation>
```

### Skipped checks

```markdown
## Skipped checks (<count>)

For each skipped check, name and reason (anchor missing, target file absent, etc.)
```

### Review required

```markdown
## Review required

These checks require human judgment.

| # | Check | Question | Status |
|---|---|---|---|
| 1 | <name> | <one-line question> | [ ] |
| ... | | | |

To complete the audit: review each item, mark PASS / FAIL / ARGUED-DEVIATION inline, save the file, then rerun `/saas-ui-audit --review-completed audits/<target>/<file>.md`.
```

### Remediation summary

```markdown
## Remediation summary

Ordered by severity + ease of fix:

1. **<finding>** — run `<command>` (CRITICAL, ~5 min)
2. **<finding>** — edit `<file>:<line>` (HIGH, ~15 min)
3. **<finding>** — review and decide (REVIEW-NEEDED)
```

### Historical comparison (if previous audits exist)

```markdown
## Compared to previous audit (<date>)

- New findings: <N>
- Resolved findings: <N>
- Severity trend: <improving | worsening | stable>
```

---

## Writing the file

1. Create directory `.saas-ui/audits/<target>/` if absent.
2. Generate filename `<YYYY-MM-DD-HHMM>.md`.
3. Write the scorecard.
4. Copy (or rewrite) `latest.md` with the same content.
5. Optionally update STATE.md with an `audits_run` count or last-audit timestamp.

---

## What NOT to do in phase 5

- **Don't delete previous audits.** History is valuable for trend tracking.
- **Don't write the file if findings list is empty.** That shouldn't happen (PASS findings still produce summary entries); if it does, emit a "skipped — nothing to audit" output.
- **Don't include code snippets longer than 5 lines.** Findings should reference file:line, not embed code.
- **Don't write multiple audit files in one run.** One run = one scorecard.

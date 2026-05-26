---
name: saas-ui-audit
description: Use when the user asks to audit / lint / review a saas-ui artifact or implementation, runs /saas-ui-audit, says "check this composite for issues", "does my code match the SPEC", "audit the inbox module", or wants a pre-merge gate before shipping. Reads CONSTITUTION + PRINCIPLES + INVENTORY + common-llm-ui-mistakes.md + EXECUTION-POSTURE and lints any target artifact (SPEC.md, STATES.md, code in src/) against them. Two modes: pre-implementation (artifacts only) and post-implementation (code vs artifacts). Outputs a posture-scaled scorecard with pass / fail / argued-deviation per check. Strict on functional rules under all postures; lenient on polish-tier rules under SPRINT.
---

# saas-ui-audit

The critic. Lints any saas-ui artifact (or its implementation) against the project's constitution, principles, inventory, and the 17 known LLM UI pathologies.

This is the gate that catches what the design phases couldn't prevent — drift between SPEC and code, atom invention, duplicate buttons, generic empty states. It runs after artifacts ship and before code ships.

## When to invoke

- **Pre-implementation:** before writing code for a composite or module, audit its SPEC/FLOWS to surface design-level issues
- **Post-implementation:** before merging, audit the implementation against its SPEC
- **Project-wide:** `/saas-ui-audit` (no target) — run across all spec'd artifacts; useful before a release
- **Pathology hunt:** `/saas-ui-audit --pathology <N>` — focused check for a specific LLM pathology (1–17)
- **Drift detection:** when `saas-ui-status` flags drift between artifacts and filesystem

## Audit targets

| Target | Mode | What's checked |
|---|---|---|
| `composite/<name>/SPEC.md` and siblings | pre-impl | SPEC against CONSTITUTION, INVENTORY, pathologies, composite-level Laws |
| `module/<name>/` artifacts | pre-impl | FLOWS against CONSTITUTION, module-level Laws, EMPTY-STATES copy rules |
| `src/**/<composite-name>*` code files | post-impl | Code against composite SPEC (Action Inventory, States, Keyboard, Rendering boundary) |
| `src/**/<module-name>*` code files | post-impl | Code against module BUILD-ORDER, FLOWS, EMPTY-STATES |
| project-wide (no target) | both | Cross-artifact consistency, atom invention sweep, pathology sweep |

## What this skill does NOT do

- **No fixing.** Output is a scorecard with remediation guidance. Fixes happen in the appropriate skill (re-run composite phase 11 to update IMPLEMENTATION, re-run module phase 8 to update copy, hand-edit code, etc.).
- **No design judgments not encoded in CONSTITUTION/PRINCIPLES.** The audit only checks against what's written. If a rule isn't in CONSTITUTION, the audit won't enforce it.
- **No code style.** The audit checks correctness against design contracts, not formatting, linting, or React-idiomatic concerns. Use ESLint, Prettier, etc. for those.
- **No security review.** Different concern; use a security audit skill.

## Iron rules

1. **Posture-scaled strictness, but functional rules always strict.** The 17 pathologies are always-on, regardless of posture. Polish-tier rules (Peak-End polish quality, Gestalt consistency depth) scale to posture.
2. **Findings cite the source rule.** Every finding references CONSTITUTION rule N, INVENTORY constraint X, or Pathology #M. No bare findings.
3. **Argued deviations are first-class.** If a SPEC explicitly argues a deviation (with `Argue:` line or `Reason:` line), the audit records it as `ARGUED-DEVIATION`, not `FAIL`. The audit's job is to ensure deviations have rationale, not to disallow them.
4. **One scorecard per target.** Even project-wide audits produce per-artifact scorecards plus a roll-up summary.
5. **Findings are actionable.** Each `FAIL` includes the rule violated AND a remediation hint (re-run skill X, edit field Y).
6. **No silent skips.** If a check can't run (e.g., target file missing), the scorecard says so explicitly.

## Phase outline

| # | Phase | Output |
|---|---|---|
| 0 | Pre-flight & anchor load | (validation only) |
| 1 | Target identification & check enumeration | (in-memory check list) |
| 2 | Programmatic checks | findings list (PASS / FAIL / SKIP) |
| 3 | Manual-review surfacing | findings list (REVIEW-REQUIRED) |
| 4 | Posture scaling | findings adjusted with posture weight |
| 5 | Scorecard composition | `audits/<target>/<date>.md` |
| 6 | Remediation suggestions | end of scorecard |
| 7 | Hand-off | console summary + STATE.md note |

## On-disk layout

```
.saas-ui/
  audits/
    <target>/
      <YYYY-MM-DD-HHMM>.md   ← scorecard for this audit run
      latest.md              ← symlink or copy of most recent
```

Audits accumulate. Previous audits become history — useful for tracking improvement over time.

## Output discipline

- **Scorecard format:** structured Markdown per `templates/AUDIT-REPORT.md`. Easy to scan, easy to diff against previous audits.
- **One section per check category:** functional rules (always strict), composite/module/project laws (posture-scaled), 17 pathologies, atom invention sweep, drift detection.
- **Severity:** CRITICAL (functional rule violated; ship-blocker) > HIGH (CONSTITUTION rule violated without argument) > MEDIUM (polish-tier rule under non-CRAFT) > LOW (suggestion).
- **Counts on top:** scorecard opens with summary counts.

## Files in this skill

- `phases/00-preflight.md` — directory + anchor + target validation
- `phases/01-identify-checks.md` — which checks apply to this target
- `phases/02-programmatic-checks.md` — count-based, name-match, presence rules
- `phases/03-manual-checks.md` — subjective rules surfaced for human review
- `phases/04-posture-scaling.md` — apply posture-specific strictness
- `phases/05-compose-scorecard.md` — write `audits/<target>/<date>.md`
- `phases/06-remediation.md` — actionable next steps per finding
- `templates/AUDIT-REPORT.md` — scorecard format
- `references/audit-rules.md` — the canonical rule catalog
- `references/pathology-detection.md` — how to detect each of the 17 LLM pathologies
- `references/posture-strictness.md` — gate thresholds per posture

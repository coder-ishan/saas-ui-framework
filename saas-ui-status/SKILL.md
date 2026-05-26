---
name: saas-ui-status
description: Use when the user asks "where am I in the saas-ui workflow", "what's next", "saas-ui status", runs /saas-ui-status, or wants a project-state snapshot before deciding what to do. Reads .saas-ui/STATE.md + EXECUTION-POSTURE.md + MODULES.md + COMPOSITES.md + INVENTORY.md and produces a single-screen status report covering bootstrap completion, posture commitment + freshness, composite SPEC progress, module design progress, atom gaps, and a prioritized next-action list. Read-only — does not interview or produce new design artifacts. Optionally heals minor STATE.md drift.
---

# saas-ui-status

Lightweight orchestrator. Reads project state and tells you where you are.

This skill is the friction-free entry point to a saas-ui project. Instead of remembering which composites are spec'd, which modules need design, or whether the execution posture is stale, run `/saas-ui-status` for a single-screen snapshot with a prioritized next-action list.

## When to invoke

- User explicitly asks for status, progress, or "where am I"
- User starts a session and isn't sure what to do next
- After completing a composite SPEC or module design, before picking the next one
- Before re-committing execution posture (status surfaces what posture-shift would affect)
- After a long break — re-orient before continuing work

## What this skill does NOT do

- **No interviews.** Read-only against the project state.
- **No new design artifacts.** Doesn't write CONSTITUTION, MODULES, COMPOSITES, etc.
- **No code.** It's a planning-layer status check.
- **No posture changes.** If posture is stale, it surfaces that — the user re-commits via `/saas-ui-bootstrap` phase 10.
- **No catalog edits.** If a module or composite is missing, it surfaces that — the user adds it via the appropriate bootstrap phase.

## What this skill MAY do

- **Heal minor STATE.md drift.** Examples:
  - If a `composites/<name>/SPEC.md` exists but STATE.md says `unspec'd`, update STATE.md to `spec'd`.
  - If `modules/<name>/BUILD-ORDER.md` exists but STATE.md says `not-started`, update STATE.md to `designed`.
  - Only safe drift corrections — never the reverse direction. If STATE.md says `spec'd` but the file is missing, surface as a discrepancy, don't auto-edit.

## Entry protocol

1. **Check for `.saas-ui/` directory.**
   - If absent: tell user "No saas-ui project here. Run `/saas-ui-bootstrap` to start." Halt.

2. **Load anchors.**
   - `.saas-ui/STATE.md` (required)
   - `.saas-ui/EXECUTION-POSTURE.md` (warn if absent — bootstrap incomplete)
   - `.saas-ui/MODULES.md` (warn if absent)
   - `.saas-ui/COMPOSITES.md` (warn if absent)
   - `.saas-ui/INVENTORY.md` (warn if absent)
   - Glob `.saas-ui/composites/*/SPEC.md` to detect what's actually spec'd
   - Glob `.saas-ui/modules/*/BUILD-ORDER.md` to detect what's actually designed

3. **Compute derived status.** See phase 1.

4. **Format report.** See phase 2.

5. **Suggest next actions.** See phase 3.

## Phase outline

| # | Phase | Output |
|---|---|---|
| 0 | Pre-flight & anchor load | (validation only) |
| 1 | Compute status | (in-memory) |
| 2 | Format report | console output (using `templates/STATUS-REPORT.md`) |
| 3 | Suggest next actions | console output |

Unlike other saas-ui-* skills, this one is single-invocation — no checkpoint dialogs, no resumability. Output is the report itself.

## Output discipline

- **Single screen.** The report fits in one terminal screen (~50 lines).
- **No emoji-as-data.** Use ASCII symbols (✓ ✗ ⚠ →) only for status pills; never as decoration.
- **Posture-aware.** Suggestions match the current posture (SPRINT recommends shipping not polishing; CRAFT recommends polishing the next anchor module).
- **Hard truths.** If posture is stale (>30 days), say so. If a composite is blocking 5 modules, say so. The report does not hide signals.

## Files in this skill

- `phases/00-preflight.md` — directory + anchor validation
- `phases/01-compute.md` — derive status from anchors
- `phases/02-report.md` — format the report
- `phases/03-suggest-next.md` — prioritized next-action list
- `templates/STATUS-REPORT.md` — output format with placeholders
- `references/status-checks.md` — what each check measures + interpretation
- `references/next-action-rules.md` — how to prioritize next actions per posture and state

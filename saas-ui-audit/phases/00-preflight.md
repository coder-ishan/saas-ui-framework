# Phase 0 — Pre-flight & anchor load

**Purpose:** Validate target exists, anchors are available, posture is committed.

**Produces:** No artifact. Validation only.

---

## The four checks

### Check 1 — `.saas-ui/` exists

If not:
> "No saas-ui project here. Run `/saas-ui-bootstrap` first."

Halt.

### Check 2 — Target identification

Targets:
- `composite/<name>` → look for `.saas-ui/composites/<name>/`
- `module/<name>` → look for `.saas-ui/modules/<name>/`
- `code:<path>` → look for `<path>` in repo (post-impl mode)
- No target → project-wide audit

If a named target doesn't exist:
> "Target `<name>` not found. Either it's not spec'd yet, or the path is wrong. Run `/saas-ui-status` to see what's available."

Halt.

### Check 3 — Anchor load

Required:
- `.saas-ui/STATE.md`
- `.saas-ui/CONSTITUTION.md`
- `.saas-ui/PRINCIPLES.md`
- `.saas-ui/INVENTORY.md`
- `.saas-ui/EXECUTION-POSTURE.md` (warn if missing; default to BALANCED strictness)
- `.saas-ui/MODULES.md`
- `.saas-ui/COMPOSITES.md`

Skill-internal references:
- `~/.claude/skills/saas-ui-bootstrap/references/common-llm-ui-mistakes.md` (the 17 pathologies)
- `~/.claude/skills/saas-ui-audit/references/audit-rules.md`
- `~/.claude/skills/saas-ui-audit/references/pathology-detection.md`
- `~/.claude/skills/saas-ui-audit/references/posture-strictness.md`

For target-specific audits:
- The target's own artifacts (SPEC, STATES, KEYBOARD, etc. for composite; OVERVIEW, FLOWS, etc. for module)
- For code audits: the source files matching the target

If any required anchor is missing: warn but proceed where possible. Note in scorecard's "skipped checks" section.

### Check 4 — Determine audit mode

- **Pre-implementation:** target is `composite/<name>` or `module/<name>` (artifact mode)
- **Post-implementation:** target is `code:<path>` — requires the corresponding artifact to exist for comparison
- **Project-wide:** no target — runs reduced check set across all artifacts

Announce the mode to the user before running:
> "Auditing `<target>` in `<mode>` mode with posture `<posture>`. Estimated <N> checks. Starting…"

---

## Success criteria

- Target identified and exists (or project-wide mode confirmed)
- All required anchors loaded (or absent ones noted)
- Posture committed (or default BALANCED noted)
- Audit mode determined

---

## What NOT to do in phase 0

- **Don't run any checks.** This is validation only. Checks happen in phase 2.
- **Don't write to disk.** Pre-flight is read-only.
- **Don't halt on missing posture.** Default to BALANCED strictness if posture not committed; note in scorecard.

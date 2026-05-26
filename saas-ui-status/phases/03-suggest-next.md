# Phase 3 — Suggest next actions

**Purpose:** Compose the prioritized `Next` section from phase 1's `next_actions` list. Apply posture rules to pick the top recommendation.

**Produces:** The `Next` section content (already included in phase 2's report shape; this phase encodes the prioritization logic).

---

## Priority rules by posture

Reference: `references/next-action-rules.md` has the full decision matrix.

### When bootstrap is incomplete

P0 action is always: "Finish bootstrap at phase N: /saas-ui-bootstrap".

Everything else is gated on bootstrap completion.

### When posture is not committed (bootstrap phase 10 not done)

P0 action: "Commit execution posture: /saas-ui-bootstrap phase 10".

Reason: downstream skills (composite IMPLEMENTATION.md, module BUILD-ORDER.md) halt without it.

### When posture is stale

P0 or P1 (depending on severity): "Re-evaluate posture: /saas-ui-bootstrap phase 10".

Severity is high if:
- Stale by >2x the threshold (60+ days for SPRINT, 180+ for BALANCED, 360+ for CRAFT)
- Re-evaluation trigger explicitly cites a past event (e.g., "after demo on Mar 14" and that date has passed)

### When everything is green (posture fresh, bootstrap complete, no drift)

Apply posture-specific priority:

#### SPRINT — ship MVP

1. **Spec the highest-leverage unspec'd P0 composite.**
   - Highest-leverage = blocks the most anchor modules
   - No atom gaps (so it can ship cleanly)
   - Command: `/saas-ui-composite <name>`

2. **Design the highest-leverage ready-to-design anchor module.**
   - Ready = no blocking unspec'd composites
   - Anchor tier (highest polish budget)
   - Command: `/saas-ui-module <name>`

3. **If an atom gap blocks 2+ composites:** add the atom to the design system first.
   - Not a skill action — surfaces as: "Add atom <name> to design system; blocks <composite-list>"

4. **If all P0 composites are spec'd and ≥1 anchor module is designed:** "Begin implementation." Status doesn't drive implementation directly, but it's the natural next step.

#### BALANCED — V1 quality

1. **Spec the next composite in COMPOSITES.md build order.**
   - Build order respects Pareto + atom dependencies
   - Command: `/saas-ui-composite <name>`

2. **Design the next module in MODULES.md build order.**
   - Command: `/saas-ui-module <name>`

3. **Address drift warnings** (auto-heal where safe).

4. **Re-run IMPLEMENTATION.md** for any composite whose SPEC has changed since IMPLEMENTATION was generated.

5. **Re-run BUILD-ORDER.md** for modules whose dependent composite SPECs have changed.

#### CRAFT — polish

1. **Polish the most-used anchor composite.**
   - Re-run phase 11 to extend IMPLEMENTATION cuts
   - Audit composite against its LAWS.md verification points
   - Command: `/saas-ui-audit composite/<name>` (when audit ships)

2. **Polish the dominant anchor module's Peak-End.**
   - Open `modules/<name>/LAWS.md` → Peak-End section
   - Verify the peak's polish investment matches CRAFT standards

3. **Address every warning** — under CRAFT, warnings are higher-cost than under SPRINT.

4. **Run audit** across the project (when audit ships): `/saas-ui-audit`.

---

## Drift remediation offer

If `warnings` contain auto-healable drift items (file exists but STATE.md shows not-started), the report ends with:

```text
Auto-heal available:
- DataTable: STATE.md → mark spec'd (filesystem confirms)
- Inbox: STATE.md → mark designed (filesystem confirms)

Run /saas-ui-status --heal to apply these corrections.
```

The `--heal` invocation is the only mode where this skill writes — and only to STATE.md, never to design artifacts.

### Auto-heal rules

Safe to heal (write to STATE.md):
- File exists but STATE.md shows status earlier than the file implies. Forward-only.

NEVER heal (always surface, never write):
- File missing but STATE.md claims it exists (data loss risk; user must investigate).
- Conflicting state across files (e.g., COMPOSITES.md and SPEC.md disagree about composite type).
- Drift between EXECUTION-POSTURE.md and downstream IMPLEMENTATION.md cuts (posture changed without re-running downstream skills).

---

## Maximum 5 next-actions

If more than 5 priorities exist, show top 5 with "+ N more" note.

If fewer than 3 priorities exist (rare — usually means everything is done at current posture):
- Surface posture-upgrade question: "All work at current posture (<name>) is done. Consider upgrading to <next posture> or starting a new milestone."

---

## What NOT to do in phase 3

- **Don't suggest non-saas-ui actions.** Status is about the framework's state, not the user's broader work.
- **Don't auto-heal without asking.** `--heal` flag is required to write to STATE.md.
- **Don't recommend a skill that requires a missing prerequisite.** If `/saas-ui-module Inbox` is blocked by an unspec'd composite, recommend the composite instead.
- **Don't show the same action twice.** If "spec DetailDrawer" is #1 and also unblocks several modules, don't list each unblock as a separate action.

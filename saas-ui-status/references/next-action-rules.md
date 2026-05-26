# Next-action rules — decision matrix for prioritization

> Reference for phase 3. Encodes how `next_actions` is composed given the current status.

---

## The five-question decision tree

Each invocation runs through these questions in order. The first one that triggers determines P0 (top recommendation).

### Q1 — Is bootstrap incomplete?

If `bootstrap.complete == false`:

**P0:** "Finish bootstrap at phase N: `/saas-ui-bootstrap`"

Reason: nothing else is well-grounded without CONSTITUTION + INVENTORY + MODULES + COMPOSITES + POSTURE.

P1 onwards are skipped — the report focuses on completing bootstrap.

### Q2 — Is posture not committed?

If `posture.committed == false` (phase 10 hasn't run):

**P0:** "Commit execution posture: `/saas-ui-bootstrap` phase 10"

Reason: composite IMPLEMENTATION.md and module BUILD-ORDER.md halt without it.

### Q3 — Is posture severely stale?

If `posture.days_since_commit > 2 × threshold` OR `re_evaluation_trigger` references past date:

**P0:** "Re-evaluate posture: `/saas-ui-bootstrap` phase 10"

Reason: the posture has likely shifted from SPRINT → BALANCED or BALANCED → CRAFT, and downstream artifacts are computed on a now-wrong premise.

### Q4 — Are there unsafe drift errors?

If any item in `errors` is non-recoverable (e.g., "SPEC missing but STATE claims spec'd"):

**P0:** "Resolve drift: <error description>"

Reason: data loss risk takes precedence over forward work.

### Q5 — All green; apply posture-specific priority

Otherwise, apply the matrix below.

---

## Posture-specific priority matrix

### SPRINT — ship MVP

Goal: ship anchor flows functional and CONSTITUTION-compliant in weeks.

| Priority | Action | When this fires |
|---|---|---|
| 1 | Spec next unspec'd P0 composite (anchor-blocking, no atom gaps) | Unspec'd P0 composites with no gaps exist |
| 2 | Design next ready-to-design anchor module | Anchor modules with no blocking composites exist |
| 3 | Resolve P0 atom gap blocking ≥2 composites | Such a gap exists |
| 4 | Implement what's spec'd | All P0 composites spec'd + ≥1 anchor module designed |
| 5 | Defer P1 composites; rare modules | Always at SPRINT |

Skipped under SPRINT (not recommended as next-actions):
- Polishing existing composites (CRAFT mode)
- Spec'ing rare-tier composites (deferred per posture)
- Running audit (audit is light-touch under SPRINT)

### BALANCED — V1 quality

Goal: ship polished V1 for paying customers in months.

| Priority | Action | When this fires |
|---|---|---|
| 1 | Spec next composite in COMPOSITES.md build order | Unspec'd composites exist |
| 2 | Design next module in MODULES.md build order | Ready-to-design modules exist |
| 3 | Address drift warnings (auto-heal where safe) | Drift exists |
| 4 | Re-run IMPLEMENTATION.md for composites with changed SPEC | Such composites exist |
| 5 | Re-run BUILD-ORDER.md for modules with changed composite SPECs | Such modules exist |
| 6 | Run audit on spec'd composites | All P0 composites spec'd |

### CRAFT — polish to Linear-grade

Goal: best-in-class. All anchor modules and composites at peak polish.

| Priority | Action | When this fires |
|---|---|---|
| 1 | Polish most-used anchor composite | Always (if any composite is implemented) |
| 2 | Polish dominant anchor module's Peak-End | Always (if dominant module designed) |
| 3 | Address every warning | Any warnings exist |
| 4 | Run audit project-wide | Always |
| 5 | Spec the next composite | Unspec'd composites exist |
| 6 | Verify cross-variant consistency in canonical composites | Composites with variants exist |

Notably under CRAFT, *warnings* become higher-priority than new design work — polish before expansion.

---

## Cross-cutting rules

These apply at every posture:

### Rule 1 — Single primary recommendation

The report shows up to 5 next-actions, but P0 is the *single* primary recommendation. The other 4 are alternatives.

### Rule 2 — Commands are concrete

Every next-action includes the literal slash command to run. Not "consider designing the inbox" — but "`/saas-ui-module Inbox`".

### Rule 3 — Reasons cite the source

The reason for each recommendation cites the data source: "blocks 4 modules", "no atom gaps", "posture is 38 days stale". Not just "high priority."

### Rule 4 — Skip rare-tier under SPRINT

Under SPRINT, never recommend speccing a rare-tier composite or designing a rare-tier module. They're deferred per posture; surfacing them noise-up the report.

### Rule 5 — Posture transitions

If the user runs `/saas-ui-status` and all work at current posture is done, suggest:

```text
All <POSTURE>-scope work complete.

Consider:
- Upgrading posture (SPRINT → BALANCED → CRAFT) via /saas-ui-bootstrap phase 10
- Starting a new milestone (revisit MODULES.md / COMPOSITES.md for new entries)
- Implementing what's been designed (status is read-only; implementation happens elsewhere)
```

This prevents the loop where the user keeps running `/saas-ui-status` looking for the next thing when the next thing is actually outside the framework.

---

## Examples

### Example 1: fresh project

```text
Next  Recommended actions:
      1. /saas-ui-bootstrap
         — bootstrap not started; design constitution needs to be written
```

### Example 2: bootstrap done, SPRINT, no composites yet

```text
Next  Recommended actions, posture-tuned for SPRINT:
      1. /saas-ui-composite DataTable
         — anchor-blocking (4 modules); no atom gaps
      2. /saas-ui-composite CommandPalette
         — anchor-blocking (3 modules); no atom gaps
      3. Defer rare-tier composites until SPRINT goal met
```

### Example 3: BALANCED, mid-flight

```text
Next  Recommended actions, posture-tuned for BALANCED:
      1. /saas-ui-composite FilterBuilder
         — next in COMPOSITES.md build order; atoms resolved
      2. /saas-ui-module Customers
         — next in MODULES.md build order; no blocking composites
      3. /saas-ui-status --heal
         — auto-heal 2 safe drift items
```

### Example 4: CRAFT, polishing

```text
Next  Recommended actions, posture-tuned for CRAFT:
      1. Audit DataTable composite against its LAWS.md
         — most-used composite (in 4 anchor modules); ensures polish-tier rules upheld
      2. Polish Peak-End in Inbox module
         — dominant anchor module; review modules/inbox/LAWS.md → Peak-End
      3. /saas-ui-audit
         — run project-wide audit (when shipped)
```

### Example 5: everything done

```text
Next  All CRAFT-scope work complete.

      Consider:
      - Starting a new milestone (revisit MODULES.md / COMPOSITES.md)
      - Implementing what's designed (status is read-only)
```

# Phase 2 — Format report

**Purpose:** Render the in-memory status from phase 1 into a single-screen console report.

**Produces:** Console output using `templates/STATUS-REPORT.md` as the format guide.

---

## The report shape

The report has six sections, in order:

1. **Header** — project name (if available) + last updated
2. **Bootstrap** — progress bar, current phase, blockers
3. **Posture** — committed posture, age, staleness flag
4. **Inventory** — atom/molecule count, gap count, P0 gaps
5. **Composites** — table of composites with status pills
6. **Modules** — table of modules with status pills + blockers
7. **Drift & warnings** — only if non-empty
8. **Next** — prioritized action list (3–5 entries)

Total length target: ~40–60 lines.

---

## Section formats

### Header

```text
saas-ui status — <project name or path basename>
Last updated: <date from STATE.md>
```

### Bootstrap

If complete:
```text
Bootstrap     ████████████ complete (11/11 phases)
```

If in progress:
```text
Bootstrap     ████████░░░░ in progress (8/11 phases)
              Current phase: phase 8 — Forbidden patterns
              Resume:        /saas-ui-bootstrap
```

If not started:
```text
Bootstrap     ░░░░░░░░░░░░ not started
              Start:         /saas-ui-bootstrap
```

### Posture

If committed and fresh:
```text
Posture       BALANCED — committed 12 days ago
              Re-evaluate at: first 50 paying customers
```

If committed and stale:
```text
Posture       SPRINT — committed 38 days ago ⚠ STALE
              Re-evaluate at: investor demo (Mar 14)
              Consider re-running /saas-ui-bootstrap phase 10
```

If not committed (bootstrap incomplete):
```text
Posture       not yet committed
              Complete bootstrap phase 10 to commit
```

### Inventory

```text
Inventory     <N> atoms · <N> molecules · <N> gaps
              <N> P0 gaps blocking composites:
              - <atom>: blocks <composite-list>
              - <atom>: blocks <composite-list>
```

If no gaps:
```text
Inventory     <N> atoms · <N> molecules · 0 gaps ✓
```

### Composites table

One line per composite. Status pills:
- `[not started]` — not in build queue yet
- `[in progress: phase N]` — SPEC partly done
- `[spec'd]` — SPEC.md exists, IMPLEMENTATION.md not yet
- `[implementation-ready]` — both SPEC.md + IMPLEMENTATION.md exist
- `[blocked]` — has unresolved atom gap
- `[drift]` — STATE.md and filesystem disagree

```text
Composites    Name              Status                Blocks N modules
              ----------------- --------------------- ----------------
              DataTable         implementation-ready  4 modules
              CommandPalette    spec'd                3 modules
              FilterBuilder     blocked (atom: Chip)  2 modules
              DetailDrawer      not started           5 modules
              ...
```

If many composites: show top 8 by `blocks N modules` descending; note "... + N more (see COMPOSITES.md)".

### Modules table

One line per module. Status pills:
- `[not started]` — design hasn't begun
- `[in design: phase N]` — design in progress
- `[designed]` — full BUILD-ORDER.md exists
- `[blocked]` — has missing composite SPECs
- `[drift]` — filesystem disagreement

```text
Modules       Name (tier)       Status              Blocking composites
              ----------------- ------------------- -------------------
              Inbox (anchor)    designed            (none)
              Customers (anch)  in design: phase 4  (none)
              Reports (occ)     blocked             FilterBuilder, ExportDialog
              Billing (rare)    not started         (none)
              ...
```

### Drift & warnings

Only if `warnings` or `drift_flag` non-empty:

```text
Warnings      ⚠ Posture is stale (38 days; trigger said "demo by Mar 14")
              ⚠ DataTable: SPEC exists but STATE.md shows unspec'd (auto-heal available)
              ⚠ FilterBuilder: STATE.md claims spec'd but file missing
              ⚠ 2 anchor modules blocked by unspec'd composites
```

If auto-heal available, offer at the end (see phase 3).

### Next

```text
Next          Recommended actions, posture-tuned for BALANCED:
              1. /saas-ui-composite DetailDrawer
                 — blocks 5 modules (highest leverage), no atom gaps
              2. /saas-ui-module Inbox  (already designed; consider implementing)
              3. /saas-ui-composite FilterBuilder
                 — blocked by atom: Chip → add Chip to design system first
              4. /saas-ui-bootstrap phase 10
                 — re-commit posture (current is 38 days stale)
              5. Resolve drift: /saas-ui-status --heal
```

---

## Rendering rules

- ASCII boxes only; no Unicode box-drawing characters in case of fixed-width font issues
- Pills use square brackets `[spec'd]` not parentheses
- Warnings use `⚠` prefix (single Unicode char is acceptable)
- Checkmark for clean state: `✓`
- Section headers in fixed left column; content right-aligned via spaces
- Hard line width: 80 chars max so it doesn't wrap in standard terminals

---

## What NOT to do in phase 2

- **Don't paginate.** Single screen. If content overflows, summarize (e.g., "... + N more composites — see COMPOSITES.md").
- **Don't suggest more than 5 next-actions.** Decision paralysis.
- **Don't use emoji as decoration.** Only `⚠` for warnings and `✓` for clean state.
- **Don't restate every detail from CONSTITUTION/PRINCIPLES.** Status is a pointer to state, not a re-render of design.

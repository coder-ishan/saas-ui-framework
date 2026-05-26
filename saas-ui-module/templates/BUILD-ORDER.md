# <Module name> — Build order & implementation strategy

**Posture:** <SPRINT / BALANCED / CRAFT — from EXECUTION-POSTURE.md>
**Last updated:** <YYYY-MM-DD>

---

## Flow priority cut

| Flow | Priority (FLOWS.md) | SPRINT cut | BALANCED cut | CRAFT cut |
|---|---|---|---|---|
| | P0 | full | full | full |
| | P1 | stub or skip | full | full |
| | P2 | skip | stub | full |

### Posture applied

This project is **<posture>**.

**In scope:**
- ...

**Stubbed (visible but not functional):**
- ...

**Deferred (not in this iteration):**
- ...

---

## Per-flow cuts

### <Flow name>

**Full design:** <step count> steps with <pattern>.

**MVP cut (SPRINT):**
- Steps included: ...
- States required: ...
- States deferred: ...
- Copy: ...
- Keyboard: ...

**V1 cut (BALANCED):**
- ...

**CRAFT cut (full):**
- ...

---

## Critical path

```text
<dependency chain from raw input to ship>
```

**Estimated duration (rough order-of-magnitude):**
- SPRINT: <range>
- BALANCED: <range>
- CRAFT: <range>

---

## Parallel streams

| Stream | Owner pattern | What's in it | Depends on | Unblocked at |
|---|---|---|---|---|
| A — data + server actions | backend | | | day 0 |
| B — composite shell + states | frontend | | | day 0 |
| C — flow integration | full-stack | | A & B at 70% | day N |
| D — empty/error/copy | designer/PM | | | day 0 |
| E — polish (Peak-End, motion) | design-engineer | | A–D at 90% | day M |

**Recommended for this posture:**
- ...

---

## Stub strategy

### <Stubbed surface name>

- **Visible:** ...
- **Stub copy:** ...
- **Interactions:** disabled with tooltip
- **State:** empty (stub variant)
- **Reason for stub vs hide:** ...

### <Fully deferred surface>

- **Hidden until:** ...
- **Direct URL behavior:** 404 with module-aware copy

---

## Posture-shaped notes

<recommendations from references/execution-postures.md applicable to current posture>

---

## Re-evaluation triggers

Recompute when:
- EXECUTION-POSTURE.md changes
- A dependent composite SPEC changes
- A flow is added, removed, or re-prioritized

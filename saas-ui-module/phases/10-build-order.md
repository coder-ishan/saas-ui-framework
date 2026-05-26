# Phase 10 — Build order & implementation strategy

**Purpose:** Translate the module's design artifacts into an execution plan tuned to the project's EXECUTION-POSTURE. The same module ships differently under SPRINT, BALANCED, or CRAFT postures — this phase makes the difference explicit so implementation is fastest under the project's actual constraints.

**Produces:** `BUILD-ORDER.md`.

**Estimated time:** 15 minutes.

---

## Inputs

Required before this phase:
- `.saas-ui/EXECUTION-POSTURE.md` (from `saas-ui-bootstrap` phase 10) — names the posture
- This module's `OVERVIEW.md`, `FLOWS.md`, `LAWS.md`, `COMPOSITES.md`
- Each dependent composite's `IMPLEMENTATION.md` (from `saas-ui-composite` phase 11), if spec'd

If `EXECUTION-POSTURE.md` is missing:
> "EXECUTION-POSTURE.md is not present in .saas-ui/. Run `saas-ui-bootstrap` phase 10 first to commit a posture. Without it, build-order recommendations are ungrounded — anyone's guess."

Halt.

---

## The three postures (refresher)

From EXECUTION-POSTURE.md, the project is one of:

- **SPRINT** — ship-fast MVP. Anchor flows working end-to-end. Polish floor is CONSTITUTION minimum. Rare-tier modules deferred entirely. Internal users acceptable; investor demo or first-customer target.
- **BALANCED** — V1 quality. Anchor + occasional modules. Most flows polished; rare modules functional. Aimed at paying customer or product launch.
- **CRAFT** — production V2 or scale. All modules polished. Anchor flows hit Linear-grade. Multi-user team, established product, raising expectations.

This phase consults the posture and translates this module's design into the right scope and order.

---

## The five-step build order

### Step 1 — Flow priority cut

From FLOWS.md → Flow enumeration table, take the priority column:

```markdown
## Flow priority cut by posture

| Flow | Priority (FLOWS.md) | SPRINT cut | BALANCED cut | CRAFT cut |
|---|---|---|---|---|
| <P0 flow> | P0 | full | full | full |
| <P1 flow> | P1 | stub or skip | full | full |
| <P2 flow> | P2 | skip | stub | full |

### Posture applied

This project is **<posture from EXECUTION-POSTURE.md>**.

**In scope for this module:**
- <list of flows that ship>

**Stubbed (visible but not functional):**
- <list — empty state with "Coming soon" or disabled affordance>

**Deferred (not in this iteration):**
- <list — not even visible>
```

### Step 2 — Per-flow MVP-vs-V1 cut

For each in-scope flow, what's the minimum that makes it functional vs the full design from phase 3?

```markdown
## Per-flow cuts

### <Flow name>

**Full design (from FLOWS.md):** <step count> steps with <branching/save-resume/etc.> pattern.

**MVP cut (for SPRINT posture):**
- Steps included: <e.g., 1, 2, 3, 5 — skip step 4 optional confirmation>
- States required: <subset from composite STATES.md — typically: idle, loading-initial, error-recoverable, empty (true), success exit>
- States deferred: long-content, offline, partial-permission, error-terminal
- Copy: shared registry only; flow-specific copy uses placeholder until V1
- Keyboard: primary path only; advanced shortcuts deferred

**V1 cut (for BALANCED posture):**
- All steps from full design
- All canonical states from composite STATES.md
- Full keyboard map
- Module-specific copy per EMPTY-STATES.md
- Goal-Gradient indicators on multi-step flows

**CRAFT cut (full):**
- Everything above, plus:
- Peak-End polish per LAWS.md
- Zeigarnik resumability
- Animation polish per CONSTITUTION canonical patterns
- Edge cases per composite EDGE-CASES.md (every category addressed)
```

### Step 3 — Critical path

What's the longest dependency chain in this module?

```markdown
## Critical path

```text
<composite atom gap if any>
  ↓ blocks
<composite SPEC if unspec'd>
  ↓ blocks
<flow's primary composite>
  ↓ blocks
<flow>
  ↓ blocks
<dependent flow that hands off from above>
```

**Estimated critical-path duration (rough):**
- SPRINT: <days>
- BALANCED: <days>
- CRAFT: <days>

The critical path is the floor for "when can this module ship?" — parallelization can't beat it.
```

### Step 4 — Parallel streams

What can be built in parallel? Where are the natural seams?

```markdown
## Parallel streams

| Stream | Owner pattern | What's in it | Depends on | Unblocked at |
|---|---|---|---|---|
| Stream A — data + server actions | backend-leaning | DB schema, Server Actions, fetch endpoints | CONSTITUTION + composite SPEC rendering section | day 0 |
| Stream B — composite shell + states | frontend-leaning | Toolbar, body shell, idle/hover/focus states | INVENTORY atoms present | day 0 |
| Stream C — flow integration | full-stack | Wire Stream A + Stream B into the flows | Streams A & B at 70% | day N |
| Stream D — empty/error/copy | designer/PM-leaning | Copy registry, illustration commissioning, empty-state surfaces | EMPTY-STATES.md | day 0 |
| Stream E — polish (Peak-End, motion) | design-engineer | Peak moment polish, motion calibration | Streams A–D at 90% | day M |

**Recommended parallelization for posture:**
- SPRINT: Streams A + B in parallel; C and D combined with A/B; E deferred entirely
- BALANCED: Streams A + B + D in parallel; C after A/B at 70%; E in final week
- CRAFT: All five streams; E gets the longest tail
```

### Step 5 — Stub strategy

For deferred or stubbed surfaces, design the stub explicitly so it doesn't ship as broken:

```markdown
## Stub strategy

For each stubbed flow / surface:

### <Flow / surface name>

- **What's visible:** <e.g., the nav entry exists, but clicking shows a 'Coming soon' empty state>
- **Stub copy:** "<verb-first concrete description of what's coming>. Available in <next milestone | <approx date>>."
- **Stub interactions:** primary CTA disabled with tooltip; no fake interactivity
- **Stub state:** empty (true-empty) variant with explicit "stub" treatment, not the canonical empty state
- **Reason for stub vs hide:** <e.g., "Users expect this entry in nav from competitive products; hiding causes 'is this missing or broken?' confusion. Stub is honest.">

If a surface should be hidden entirely (no entry, no nav), say so:

### <Surface name> — fully deferred

- **Hidden until:** <milestone / posture upgrade>
- **No entry in nav or sidebar.**
- **If user reaches via direct URL:** route to 404 with module-aware copy ("That feature isn't available yet").
```

---

## Posture-shaped recommendations

The skill outputs different recommendations depending on the posture:

### SPRINT

- **Aggressive cuts.** Only P0 flows. Only minimum required states per composite. Module-specific copy minimal — use the shared registry's defaults.
- **Defer everything decorative.** No illustrations beyond CONSTITUTION's polish floor. No motion polish beyond defaults.
- **Stubs everywhere appropriate.** Better to ship 3 working anchor flows + 7 honest stubs than 5 half-working flows.
- **Critical path obsessed.** Calendar the critical path explicitly. Anything else is below-the-line.
- **Quality bar:** functional and CONSTITUTION-compliant. Not yet polished.

### BALANCED

- **P0 + P1 flows full; P2 stubbed.**
- **All canonical states present.** Edge cases addressed for P0; abbreviated for P1.
- **Module-specific copy per EMPTY-STATES.md.**
- **Motion + Peak-End for anchor flows; defaults elsewhere.**
- **Quality bar:** polished V1. Paying customer can rely on this.

### CRAFT

- **Everything in full.** All flows, all states, all edge cases, all polish.
- **Peak-End polished.** Motion calibrated. Empty illustrations commissioned.
- **All keyboard paths complete.**
- **Quality bar:** Linear-grade. The peak of what this module can be.

---

## What NOT to do in phase 10

- **Don't size by feature count.** Size by critical path. A module with 12 simple flows often ships faster than one with 3 deep flows.
- **Don't promise CRAFT polish at SPRINT cost.** If EXECUTION-POSTURE says SPRINT, write SPRINT cuts. Argue the posture in EXECUTION-POSTURE.md, not here.
- **Don't half-stub.** A stub is either honestly "coming soon" with disabled affordance, or it doesn't exist. Half-functional surfaces are bug magnets.
- **Don't skip the critical path.** It's the discipline that prevents "I'll just add one more parallel stream" from masking real blockers.
- **Don't generate fake estimates.** "Estimated critical path: <days>" should be a rough order-of-magnitude (1–3 days, 1–2 weeks, 1–2 months), not a false-precision daily count. Calendar mathematics is a different discipline.

---

## What to write

`BUILD-ORDER.md`:

```markdown
# <Module name> — Build order & implementation strategy

**Posture:** <from EXECUTION-POSTURE.md>
**Last updated:** <YYYY-MM-DD>

## Flow priority cut

<table + scope sections from Step 1>

## Per-flow cuts

<from Step 2 — one section per in-scope flow>

## Critical path

<diagram + estimate from Step 3>

## Parallel streams

<table + posture recommendation from Step 4>

## Stub strategy

<from Step 5>

## Posture-shaped notes

<the recommendations section applicable to this posture>

## Re-evaluation triggers

This BUILD-ORDER.md is recomputed when:
- EXECUTION-POSTURE.md changes
- A dependent composite SPEC changes
- A flow is added, removed, or re-prioritized in FLOWS.md
```

---

## Checkpoint

> "Three checks:
> 1. Does the posture-applied scope match what you'd actually commit to building in the next iteration? If the SPRINT cut still feels too big, cut more — SPRINT means MVP, not 'lite version of everything.'
> 2. Is the critical path the *actual* longest chain, or did you skip a dependency? Trace it forward from raw input (DB schema, atom availability) to user-facing flow ship.
> 3. Are the parallel streams genuinely independent? A stream that says 'Stream B unblocked at day 0' but actually depends on Stream A's data shape decision is not parallel — re-examine."

Iterate. Lock. Move to phase 11 (hand-off).

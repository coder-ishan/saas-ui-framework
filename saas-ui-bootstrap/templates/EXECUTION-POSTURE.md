# Execution Posture — <PRODUCT NAME>

> The project's ship-pace + fidelity commitment.
> Downstream skills (`saas-ui-composite` phase 11, `saas-ui-module` phase 10) read this to decide build order, MVP/V1 cuts, and parallel streams.

**Posture:** SPRINT | BALANCED | CRAFT
**Committed on:** <YYYY-MM-DD>
**Re-evaluation trigger:** <date or event when this should be reconsidered>

---

## Posture summary

<one paragraph stating the posture and what it means for this project>

---

## The seven commitments

| # | Question | Answer |
|---|---|---|
| 1 | Target ship horizon | <weeks / months / quarter+> |
| 2 | Validation target | <internal / first-customer / established> |
| 3 | Team shape | <solo / small / dedicated> |
| 4 | Quality bar | <functional / polished V1 / best-in-class> |
| 5 | Deferral tolerance | <high / medium / low> |
| 6 | Polish vs scope tradeoff | <reduce scope / hold scope accept floor / hold scope polish everything> |
| 7 | Known deferrals | <free-form list> |

---

## What this posture means for downstream skills

### saas-ui-composite (phase 11 — Implementation Strategy)

Each composite's `IMPLEMENTATION.md` is computed with this posture's cuts:
- MVP cut (states required for v0)
- V1 cut (states required for v1)
- Build order within composite
- Parallelization hints
- Defer list

### saas-ui-module (phase 10 — Build Order)

Each module's `BUILD-ORDER.md` is computed with this posture's cuts:
- Flow priority cut (P0/P1/P2 by posture)
- Per-flow MVP-vs-V1 cuts
- Critical path
- Parallel streams

### saas-ui-audit (when run)

The audit's strictness threshold scales with posture:
- SPRINT: strict on functional rules; lenient on polish-tier
- BALANCED: strict on canonical-state coverage and module copy
- CRAFT: strictest on cross-flow consistency, voice, motion

### saas-ui-status

Reads this file. Surfaces the current posture in every status check.

---

## Known deferrals

| Item | Reason | Re-consider when |
|---|---|---|
| | | |

---

## Re-commitment log

| Date | From | To | Reason |
|---|---|---|---|
| <YYYY-MM-DD> | (initial commit) | <posture> | <reason> |

# Phase 10 — Execution Posture

**Purpose:** Commit the project to one of three execution postures (SPRINT / BALANCED / CRAFT). This commitment governs every downstream build-order, scope cut, and parallel-stream decision. It's the bridge between the design layer (CONSTITUTION + PRINCIPLES + INVENTORY + MODULES + COMPOSITES) and the implementation layer (composite IMPLEMENTATION.md + module BUILD-ORDER.md).

**Produces:** `.saas-ui/EXECUTION-POSTURE.md`.

**Estimated time:** 10 minutes.

---

## Why this phase exists

The same product, designed identically, ships at very different speeds and quality depending on team posture. A 4-engineer team racing toward a demo ships SPRINT. The same team three months later, after first customers, ships BALANCED. A year in, with a design-engineering function, they ship CRAFT.

Without an explicit posture commitment:
- Engineers debate scope every standup ("should this state ship?")
- Designers polish surfaces the posture says to skip
- The product reads as half-polished — neither obviously MVP nor obviously crafted
- The implementation timeline drifts because every decision is re-litigated

This phase forces a single answer. It can be re-committed later (postures upgrade); but at any given time, the project has one posture.

---

## The seven questions

### Question 1 — Target ship horizon

> "When does this product (or the next milestone) ship to real users? Use a rough horizon, not a date."

Options:
- **Weeks (≤ 6 weeks)** — typically SPRINT
- **Months (2–6 months)** — typically BALANCED
- **Quarter+ (6 months+)** — could be BALANCED or CRAFT depending on quality target

### Question 2 — Validation target

> "Who is the next user audience for this product? What are they validating?"

Options:
- **Internal pilot / investor demo** — SPRINT-aligned
- **First paying customers** — BALANCED-aligned
- **Established product / competitive market** — CRAFT-aligned

If validation target conflicts with horizon (e.g., "first paying customer in 4 weeks"), surface the tension. Either compress posture (BALANCED at SPRINT pace = brutal scope cuts) or extend horizon.

### Question 3 — Team shape

> "Who's building this? Solo? Small team? Dedicated design + frontend?"

Options:
- **Solo founder / 1–2 engineers** — SPRINT or BALANCED
- **Small team with designer access (3–5)** — BALANCED
- **Dedicated design-engineering function (6+)** — BALANCED or CRAFT

### Question 4 — Quality bar

> "What does 'good' look like at ship? Pick one."

Options:
- **Functional and CONSTITUTION-compliant** (anchor flows work, polish floor met, rare modules deferred) — SPRINT
- **Polished V1** (paying-customer quality, all canonical states, anchor flows feel like Linear/Stripe) — BALANCED
- **Best-in-class** (Linear-grade across all anchors, motion calibrated, every edge case considered) — CRAFT

### Question 5 — Deferral tolerance

> "If a feature isn't ready by ship date, what happens?"

Options:
- **High tolerance** — ship without; add later. SPRINT-aligned.
- **Medium tolerance** — ship most; gate the rest. BALANCED-aligned.
- **Low tolerance** — extend timeline; ship complete. CRAFT-aligned.

### Question 6 — Polish vs scope tradeoff

> "If you had to choose, which would you cut?"

Options:
- **Reduce scope to ship at polish floor** — SPRINT
- **Hold scope; accept polish at the floor for rare; polish anchor** — BALANCED
- **Hold scope; polish everything** — CRAFT

### Question 7 — Specific deferrals already known

> "Are there features or modules you've already decided to defer for now?"

Free-form. Capture as deferral list. This populates the EXECUTION-POSTURE.md → Known deferrals section.

---

## Posture derivation

After the seven answers, derive the posture:

```text
Answers majority-aligned with SPRINT → posture = SPRINT
Answers majority-aligned with BALANCED → posture = BALANCED
Answers majority-aligned with CRAFT → posture = CRAFT
Mixed → surface tension to user
```

If mixed (e.g., "weeks horizon" + "first paying customer" + "polished V1 quality"):
> "Three of your answers indicate SPRINT (timeline, team size, deferral tolerance) but two indicate BALANCED (quality bar, validation target). This is a common trap — wanting BALANCED outcomes at SPRINT pace. To resolve:
>
> 1. **Commit to SPRINT and reduce the quality bar.** Ship anchor flows polished; defer everything else.
> 2. **Commit to BALANCED and extend the horizon.** Add weeks/months for polish.
> 3. **Commit to a hybrid: SPRINT for V0 → BALANCED for V1.** Two distinct iterations, each with its own posture.
>
> Which?"

Wait for explicit choice. Then write to EXECUTION-POSTURE.md.

---

## What to write

`EXECUTION-POSTURE.md`:

```markdown
# Execution Posture — <PRODUCT NAME>

> The project's ship-pace + fidelity commitment.
> Downstream skills (`saas-ui-composite` phase 11, `saas-ui-module` phase 10) read this to decide build order, MVP/V1 cuts, and parallel streams.

**Posture:** SPRINT | BALANCED | CRAFT
**Committed on:** <YYYY-MM-DD>
**Re-evaluation trigger:** <date or event when this should be reconsidered — e.g., "after first 10 paying customers", "after demo on <date>", "quarterly review">

---

## Posture summary

<one paragraph stating the posture and what it means for this project. E.g.:

"SPRINT — shipping MVP for investor demo in 5 weeks. Solo founder + 1 contract engineer. Quality bar: anchor flows functional and CONSTITUTION-compliant; rare-tier modules deferred; polish floor only.">

---

## The seven commitments

| # | Question | Answer |
|---|---|---|
| 1 | Target ship horizon | <weeks / months / quarter+> |
| 2 | Validation target | <internal / first-customer / established> |
| 3 | Team shape | <solo / small / dedicated> |
| 4 | Quality bar | <functional / polished V1 / best-in-class> |
| 5 | Deferral tolerance | <high / medium / low> |
| 6 | Polish vs scope tradeoff | <reduce scope / hold scope, accept floor / hold scope, polish everything> |
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
- SPRINT: strict on functional rules (atom invention, duplicate buttons, generic empty states); lenient on polish-tier (Peak-End polish, motion calibration)
- BALANCED: strict on all canonical-state coverage; expects module-specific copy
- CRAFT: strictest on cross-flow consistency, voice, motion

### saas-ui-status

Reads this file. Surfaces the current posture in every status check.

---

## Known deferrals

Features, flows, or modules explicitly out-of-scope for this iteration:

| Item | Reason | Re-consider when |
|---|---|---|
| <e.g., "Bulk import flow"> | <e.g., "Manual entry is acceptable for first 100 customers"> | <e.g., "Customer count > 100 or user feedback demands"> |

---

## Re-commitment log

Postures upgrade as products mature. Each re-commitment lands here:

| Date | From | To | Reason |
|---|---|---|---|
| <YYYY-MM-DD> | <prior posture or "(initial commit)"> | <new posture> | <one-line reason> |
```

---

## What NOT to do in phase 10

- **Don't infer posture from the absence of commitment.** Without explicit answers, halt — there is no good default. The whole point of this phase is the commitment itself.
- **Don't accept "all three" or "depends on the module."** The posture is project-level. Module-level variation happens in module BUILD-ORDER.md within the constrained posture.
- **Don't let the user commit to BALANCED at SPRINT pace silently.** Surface the tension and force the explicit choice.
- **Don't bury the re-evaluation trigger.** Postures rot. The trigger is what prompts a re-commitment.
- **Don't combine SPRINT and CRAFT.** They're opposite postures. Pick one for the current iteration. (Hybrid SPRINT-then-BALANCED is fine — that's two sequential commitments, each with its own posture.)

---

## Checkpoint

After writing EXECUTION-POSTURE.md, show the user a summary:

```markdown
## Execution posture committed: <posture>

**The commitment:**
- Ship horizon: <answer>
- Validation: <answer>
- Team: <answer>
- Quality bar: <answer>
- Deferral tolerance: <answer>
- Polish vs scope: <answer>
- Known deferrals: <count> items

**Re-evaluate at:** <trigger>

**Downstream implications:**
- saas-ui-composite will compute IMPLEMENTATION.md per composite using this posture
- saas-ui-module will compute BUILD-ORDER.md per module using this posture
- saas-ui-audit will scale strictness to this posture
- saas-ui-status will surface this posture in every check

The bootstrap is complete. Run `/saas-ui-status` to see the project state, or `/saas-ui-composite <name>` to spec the first composite.
```

---

## Hand-off

This phase completes bootstrap. Update STATE.md: `bootstrap: complete` with timestamp + committed posture name. Print the summary above.

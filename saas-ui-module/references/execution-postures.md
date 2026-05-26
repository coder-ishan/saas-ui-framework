# Execution postures — SPRINT / BALANCED / CRAFT

> The project's `EXECUTION-POSTURE.md` (from `saas-ui-bootstrap` phase 10) names one of three postures. This document expands what each posture means for module-level decisions in `BUILD-ORDER.md`.

---

## Posture: SPRINT

**When this fits:**
- Ship target: weeks, not months
- Validation goal: investor demo, first paying customer, internal pilot
- Team: solo founder, 1–2 engineers
- Quality bar: functional + CONSTITUTION-compliant, not yet polished
- Polish budget: floor only (CONSTITUTION → Density → Polish floor minimum)

**Module-level implications:**

- **Flow priority cut:** P0 flows only. P1 stubbed or deferred. P2 deferred.
- **States required:** idle, loading-initial, error-recoverable, empty (true), success exit. Other canonical states deferred.
- **Copy:** shared registry only. Module-specific copy uses sensible defaults; full module copy lands in BALANCED.
- **Peak-End:** identify the peak; do *not* invest polish budget here yet. The peak being satisfying is a BALANCED-tier achievement.
- **Goal-Gradient:** progress indicator for genuinely multi-step flows; skip for 2-step flows.
- **Zeigarnik:** save-and-resume only for flows that *must* span sessions (file uploads, multi-day forms). Defer optional resumability.
- **Serial Position:** apply at toolbar level only; defer dashboard order tuning.
- **Flow + Selective Attention:** anchor flows get protection; rare flows ship "good enough."
- **Composite variants:** minimum needed for the flows that ship. Defer per-module domain language polish.
- **Stubs everywhere appropriate:** better to ship 3 working anchor flows + 7 honest stubs than 5 half-working flows.

**Anti-pattern under SPRINT:**
Trying to polish everything to BALANCED quality with SPRINT's time. The result is half-polished UI that signals neither MVP urgency nor finished craft. Pick one.

**Quality bar:**
> "An internal user could use this without me explaining it. A demo audience would see what's good without seeing what's missing."

---

## Posture: BALANCED

**When this fits:**
- Ship target: months
- Validation goal: paying-customer launch, product-market-fit phase
- Team: small (2–5 engineers + designer)
- Quality bar: polished V1; meets Linear-adjacent quality on anchor flows
- Polish budget: meaningful on anchor + occasional modules; minimal on rare

**Module-level implications:**

- **Flow priority cut:** P0 + P1 flows full. P2 stubbed.
- **States required:** all canonical states from composite STATES.md.
- **Copy:** module-specific per EMPTY-STATES.md. Pluralization handled.
- **Peak-End:** identify *and* invest. Anchor flow peaks get polished motion + copy.
- **Goal-Gradient:** all multi-step flows have appropriate indicators.
- **Zeigarnik:** save-and-resume designed for relevant flows. Module-home pinned-incomplete pattern where applicable.
- **Serial Position:** applied across nav, menus, settings, dashboard widgets. Consistency check across module passes.
- **Flow + Selective Attention:** all flows get interruption budget; modal discipline enforced.
- **Composite variants:** all per-module deltas documented in composite VARIANTS.md.
- **Stubs:** only for genuinely P2 surfaces.

**Anti-pattern under BALANCED:**
Cutting BALANCED to SPRINT scope because of pressure — without renaming the posture. Doing this hides the trade-off; the team thinks they're shipping BALANCED quality when they're actually shipping SPRINT. Re-commit the posture explicitly if scope shifts.

**Quality bar:**
> "A paying customer relies on this. The anchor flows feel as good as Linear or Stripe. The rare flows are clearly functional even if not polished."

---

## Posture: CRAFT

**When this fits:**
- Ship target: months or quarters
- Validation goal: scale-ready product; expanding into competitive market; established brand
- Team: dedicated frontend team, design-engineering function
- Quality bar: Linear-grade. Best-in-class for the category.
- Polish budget: full investment on all modules; budget for design exploration

**Module-level implications:**

- **Flow priority cut:** all flows full. No stubs.
- **States required:** all canonical states + composite-specific extensions. Long-content, partial-permission, offline all designed.
- **Copy:** module-specific copy polished, voice-tested. Per CONSTITUTION → Copy → Voice, every empty/error/success is on-brand.
- **Peak-End:** explicit peak polish. Motion calibrated. Concrete value rendered at peak (real numbers, real names — not placeholders).
- **Goal-Gradient:** progress indicators tuned per flow (subtle vs explicit). Final-step affordances precise.
- **Zeigarnik:** resumability is a design feature, not a fallback. Module-home incomplete-task signaling tuned per user research.
- **Serial Position:** consistency across the entire module audited. First/last placement justified per element.
- **Flow + Selective Attention:** flow-state protected fiercely. Modal discipline absolute. Notification placement tuned per surface.
- **Composite variants:** fully documented and reviewed for cross-module consistency.
- **Edge cases:** every category from composite EDGE-CASES.md addressed at module level too (e.g., print considerations, high-contrast mode, keyboard-only power users).

**Anti-pattern under CRAFT:**
Over-engineering. CRAFT is about polish, not feature density. Adding more features under CRAFT *dilutes* the polish budget. CRAFT-posture modules ship fewer, more polished flows.

**Quality bar:**
> "A discerning user notices the details. The module makes the product feel like a peer of Linear, Stripe Dashboard, Notion."

---

## Posture transitions

Postures upgrade over a project's lifetime:

```text
SPRINT (MVP) → BALANCED (V1) → CRAFT (V2+)
```

Downgrades are unusual but legitimate — e.g., a CRAFT-posture team picks up a SPRINT-posture experiment on a new market segment.

When the posture changes:
1. Update `.saas-ui/EXECUTION-POSTURE.md`
2. Re-run `saas-ui-module` phase 10 for affected modules to recompute BUILD-ORDER.md
3. Re-run `saas-ui-composite` phase 11 for affected composites to recompute IMPLEMENTATION.md

The artifacts themselves don't change with posture — only the build order and scope cuts.

---

## How `saas-ui-status` reports posture

`saas-ui-status` reads `EXECUTION-POSTURE.md` and surfaces:
- Current posture
- Whether the team's recent commit pattern matches the posture's pace
- Modules whose BUILD-ORDER.md was computed under a now-stale posture (needs re-run)

This catches the silent posture drift where the team's intent changes but the artifacts lag.

---

## How `saas-ui-audit` uses posture

The audit checks artifacts against CONSTITUTION + posture:
- SPRINT posture: audit ignores polish-tier rules (Peak-End polish, motion calibration); strict on functional rules (atom invention, duplicate buttons, generic empty states)
- BALANCED posture: audit checks polish-tier rules; expects all states designed; strict on consistency
- CRAFT posture: audit checks everything; strictest on cross-flow consistency, voice, motion

Same artifacts, different gate thresholds.

---

## Cheat: which posture applies?

Quick heuristics if EXECUTION-POSTURE.md is unclear:

| Sign | Posture |
|---|---|
| "We need to demo next month" | SPRINT |
| "First paid customer onboarding next quarter" | BALANCED |
| "We need this as good as Linear" | CRAFT |
| Team is solo or duo | typically SPRINT or BALANCED |
| Team has dedicated designers + design engineers | typically BALANCED or CRAFT |
| Project is internal tools or admin panels | typically SPRINT (rare-tier nature) |
| Project is the primary external product | typically BALANCED minimum |
| Project will sell against Linear / Notion / Stripe | typically CRAFT for anchor modules |

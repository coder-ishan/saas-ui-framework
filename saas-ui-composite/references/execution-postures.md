# Execution postures (composite implementation lens)

> The project's `EXECUTION-POSTURE.md` (from `saas-ui-bootstrap` phase 10) names one of three postures. This document expands what each posture means for composite-level decisions in `IMPLEMENTATION.md`.

For module-level lens, see `~/.claude/skills/saas-ui-module/references/execution-postures.md`.

---

## How posture cascades to composites

A composite's SPEC is *posture-neutral* — it documents the full design. The composite's IMPLEMENTATION.md is *posture-shaped* — it picks which slices of the SPEC ship now vs later.

The same SPEC can produce three different IMPLEMENTATION.md outputs depending on posture. This is intentional: the SPEC is durable design intent; the implementation strategy is the current-iteration commitment.

---

## SPRINT — minimum viable composite

**Goal:** the composite fulfills its Contract (phase 1) with as little surface as possible.

**States: 5 only.**
- idle, loading-initial, error (recoverable), empty (true-empty), success exit / post-mutation idle.
- Everything else (hover, focus, selected, active-edit, loading-mutation, filter-empty, error-terminal, partial-permission, offline, long-content) deferred.

**Actions: primary affordances only.**
- Every action from Action Inventory ships with its primary affordance.
- Secondary modalities (keyboard shortcuts, command-palette entries, context menus) deferred unless they're the *only* affordance (rare).

**Motion:** CONSTITUTION canonical only. No composite-specific overrides.

**Variants:** none. Consuming modules use canonical SPEC without per-module deltas. If a module needs a delta to function (not just polish), promote to a composite SPEC revision before MVP ship.

**Keyboard:** Tab traversal (browser default) + Esc + primary-action shortcuts. Advanced shortcuts deferred.

**Test:** smoke — does the composite render and the primary action work end-to-end?

**Build-order stop:** stage 7 (empty states). Stages 8–11 deferred.

**Typical implementation time per composite (rough order):** 1–3 days for a simple composite (Toast, Tooltip); 1 week for a complex one (DataTable).

**SPRINT failure mode:** trying to ship V1 quality at MVP scope. The result is half-polished — neither MVP-honest nor V1-polished. Pick one.

---

## BALANCED — polished V1 composite

**Goal:** the composite feels good to use, supports all canonical states, and is ready for paying-customer scrutiny.

**States: all 13.**
- Full enumeration per STATES.md.
- Every state's visual treatment, allowed actions, copy, and transitions implemented.

**Actions: all from Action Inventory.**
- Primary + at least one secondary modality each.
- Argued duplicates resolved per phase 2.
- Cross-region duplications justified.

**Motion:** CONSTITUTION canonical + composite-specific overrides from MOTION.md.

**Variants:** per consuming module's BUILD-ORDER.md. Cross-variant consistency check passes.

**Keyboard:** full keyboard map per KEYBOARD.md, full focus contract, full escape ladder.

**Test:** state coverage (every required state renders correctly) + data-shape edge cases (zero rows, one row, very long content, etc.).

**Build-order stop:** stage 10 (motion polish). Stage 11 (variants) only for the variants in current BUILD-ORDER scope.

**Typical implementation time per composite:** 1–2 weeks for a simple composite; 3–4 weeks for a complex one.

**BALANCED failure mode:** scope creep. Adding "just one more state" or "one more variant" inflates timeline beyond BALANCED. Re-commit to CRAFT or hold the line.

---

## CRAFT — best-in-class composite

**Goal:** the composite is Linear-grade. Discerning users notice the details.

**States: all 13 + composite-specific extensions.**
- Long-content, partial-permission, offline all designed and tested.
- Edge cases from EDGE-CASES.md every category addressed.

**Actions: full Action Inventory with every secondary modality.**
- Keyboard, command palette, context menu all live.
- Power-user shortcuts and chord-keys if applicable.

**Motion:** fully calibrated per MOTION.md.
- Per-state durations tuned.
- Composite-specific patterns promoted to CONSTITUTION if reused.

**Variants:** all per-module variants polished. Cross-variant consistency check passes with zero argued deviations.

**Laws of UX:** all 11 composite-level laws verified.

**Performance:** streaming SSR meets Doherty budget. Virtualization where row count justifies. Cache tags tuned per consumption pattern.

**Accessibility:** ARIA roles complete. Live regions tested with screen reader. Keyboard-only walkthrough passes. Reduced motion supported.

**Test:** full state coverage + keyboard walkthrough + screen-reader walkthrough + edge case suite + reduced motion + RTL + print.

**Build-order stop:** stage 11 (variants). Nothing deferred from SPEC.

**Typical implementation time per composite:** 2–4 weeks for a simple composite; 6–8 weeks for a complex one.

**CRAFT failure mode:** scope expansion. Adding features not in the SPEC because "while we're at it." CRAFT is about polish of designed features, not feature density. New features require SPEC revision, not CRAFT-mode addition.

---

## Posture transitions for composites

A composite implementation can progress through postures over time:

```text
SPRINT impl (v0)
  ↓ posture upgrades to BALANCED
BALANCED impl (v1) — fills in deferred states, polish, variants
  ↓ posture upgrades to CRAFT
CRAFT impl (v2) — final polish, edge cases, performance, a11y
```

When a posture upgrade lands in `EXECUTION-POSTURE.md`:
1. Re-run `saas-ui-composite` phase 11 for each spec'd composite to produce updated IMPLEMENTATION.md
2. The composite's existing implementation is the starting point; the new IMPLEMENTATION.md says what to add

The audit (`saas-ui-audit`) compares the implemented code against the IMPLEMENTATION.md for the current posture — not against the full SPEC. This is how the framework grants permission to ship deliberately-incomplete composites at SPRINT/BALANCED postures without the audit failing them.

---

## Posture-driven recommendations cheat sheet

| Concern | SPRINT | BALANCED | CRAFT |
|---|---|---|---|
| States covered | 5 minimum | All 13 | All 13 + extensions |
| Actions | Primary only | + 1 secondary modality | All modalities |
| Motion | Canonical only | Canonical + overrides | Fully calibrated |
| Variants | None | In-scope per module | All |
| Keyboard | Minimum (Tab, Esc, primary) | Full keyboard map | Full + power-user |
| Edge cases | Data-shape only | Data-shape + concurrency + browser | Every category |
| Tests | Smoke | State coverage + data shape | Full suite + a11y |
| Build-order stop | Stage 7 | Stage 10 | Stage 11 |
| Defer list | Long | Modest | Empty |

---

## How `saas-ui-audit` uses posture

The audit reads `EXECUTION-POSTURE.md` and applies posture-scaled thresholds:

- **SPRINT:** strict on functional rules (atom invention, duplicate buttons, generic empty states, Contract satisfaction). Lenient on polish-tier (Peak-End polish, motion calibration, variant consistency).
- **BALANCED:** strict on all canonical-state coverage and module-specific copy. Expects all primary actions to have at least one secondary modality.
- **CRAFT:** strictest on cross-variant consistency, voice, motion, Laws-of-UX verification, accessibility.

Same artifacts. Different gate thresholds. This is how the framework respects the team's commitment without compromising on the things that must always be true (atom invention, duplicate buttons, etc. are forbidden under all postures).

---
name: saas-ui-module
description: Use when a module from .saas-ui/MODULES.md needs a flow-level design — user names a module to design, says "design the inbox flows" / "spec the customers module", runs /saas-ui-module, or saas-ui-status flags a module ready for design. Produces modules/<name>/ with OVERVIEW, FLOWS, LAWS, EMPTY-STATES, COMPOSITES, BUILD-ORDER. Applies module-level Laws of UX (Peak-End, Goal-Gradient, Zeigarnik, Serial Position, Flow, Selective Attention). Phase 10 produces BUILD-ORDER.md tuned to the project's EXECUTION-POSTURE (SPRINT / BALANCED / CRAFT). Pre-flight requires saas-ui-bootstrap complete; composites used in this module should be spec'd or have noted gaps.
---

# saas-ui-module

Designs a single module to flow-level depth. Where composites are *spatial* (they exist in a moment), modules are *temporal* — they have entry, peak, end, and the resolution the user remembers.

This is where the module-level Laws of UX apply:
- **Peak-End Rule** — users remember the peak and the end
- **Goal-Gradient Effect** — users accelerate near completion; show progress
- **Zeigarnik Effect** — incomplete tasks pull attention; design for resumability
- **Serial Position Effect** — first and last items remembered; place rare-but-critical at ends
- **Flow** — sustained attention; avoid breaking with modals
- **Selective Attention** — one thing per screen wins

## When to invoke

- A module in `.saas-ui/MODULES.md` is ready for flow design (its composites are spec'd or have noted gaps)
- The user explicitly names a module to design
- `saas-ui-status` flags a module as ready
- `saas-ui-audit` requires re-design of a module's flows after a CONSTITUTION change

## Entry protocol

Before any flow work:

1. **Bootstrap must be complete.** `.saas-ui/STATE.md` phases 0–9 done.
2. **Target module must exist in `.saas-ui/MODULES.md`.** If named module is not in the catalog, halt — modules are discovered in bootstrap phase 2, not invented here.
3. **Composite dependencies should be addressed.** Read the module's `Likely composites used` block. For each composite:
   - **Spec'd:** good — read its SPEC.md.
   - **Unspec'd but in COMPOSITES.md:** acceptable — flow design can proceed, noting that composite-specific affordances may shift when spec'd. Track in OVERVIEW.md → Composite readiness.
   - **Missing entirely:** halt — the module references a composite that doesn't exist. Either add to COMPOSITES.md (revisit bootstrap phase 3) or remove from this module's composite list.
4. **Anchors load.** `.saas-ui/CONSTITUTION.md`, `.saas-ui/PRINCIPLES.md`, `.saas-ui/INVENTORY.md`, the MODULES.md entry, dependent composite SPECs (`.saas-ui/composites/<name>/SPEC.md` for each used composite), module-tagged references.

## Iron rules

1. **No new composites here.** If a flow exposes a need for a composite not in COMPOSITES.md, halt and add it to the catalog (via bootstrap phase 3 revisit) before continuing. Modules consume composites; they don't invent them.
2. **No CONSTITUTION violations.** Modules inherit Motion, Density, Copy, State Model, Forbidden Patterns from CONSTITUTION. Module-level design respects them.
3. **Peak-End is non-negotiable.** Every module SPEC must identify the peak moment and the end moment of its primary flows. "There is no peak" is not an answer — it means the module hasn't been designed yet.
4. **Polish budget tier governs depth.** From MODULES.md → Tier:
   - **anchor** (daily-use, high polish): full 10-phase pass. Every law applied. Empty states named precisely. Composite variants documented.
   - **occasional** (weekly): phases 0–8 in full; phases 9–10 may be abbreviated.
   - **rare** (monthly / admin / billing): phases 0–3 in full; phases 4–10 may be terse ("Peak-End: not applicable, rare module"). Be explicit when abbreviating.
5. **No fork from composite SPECs.** Per-module composite variants are deltas written into the composite's VARIANTS.md (cross-linked here), not redefined in this module's COMPOSITES.md.
6. **Reason: line for every divergence** from CONSTITUTION or PRINCIPLES.

## Phase outline

| # | Phase | Output |
|---|---|---|
| 0 | Pre-flight & anchor load | (validation only) |
| 1 | Module deep purpose | `OVERVIEW.md` |
| 2 | Entry points & flow enumeration | `FLOWS.md → enumeration` |
| 3 | Per-flow design | `FLOWS.md → flow specs` |
| 4 | Peak-End design | `LAWS.md → Peak-End` |
| 5 | Goal-Gradient & Zeigarnik | `LAWS.md → multi-step / resumability` |
| 6 | Serial Position | `LAWS.md → ordering` |
| 7 | Flow state & Selective Attention | `LAWS.md → attention` |
| 8 | Empty / error / loading states | `EMPTY-STATES.md` |
| 9 | Composite variant cross-references | `COMPOSITES.md` (module-scoped index) |
| 10 | Build order & implementation strategy | `BUILD-ORDER.md` (tuned to EXECUTION-POSTURE) |
| 11 | Checkpoint & hand-off | `.saas-ui/STATE.md` updated |

Each phase has a checkpoint. Anchor modules get the full 10. Occasional and rare modules abbreviate per polish budget tier (see Iron rule #4).

## On-disk layout

```
.saas-ui/
  modules/
    <module-name>/
      OVERVIEW.md      ← module purpose, tier, entry points, composite readiness
      FLOWS.md         ← per-flow design (entry → steps → branches → exits)
      LAWS.md          ← 6 module-level Laws of UX applied
      EMPTY-STATES.md  ← module-specific empty/error/loading copy
      COMPOSITES.md    ← per-module composite variant index (cross-links to composite VARIANTS.md)
      BUILD-ORDER.md   ← implementation strategy: MVP/V1 cuts, parallel streams, critical path
```

Names are kebab-case: `inbox/`, `customers/`, `billing/`, `audit-log/`.

## Output discipline

- Flows are written as step lists with entry conditions, decision points, branches, and exits — not as wireframes or prose narratives.
- Peak-End identifications are concrete: "The peak of the invoice-creation flow is the moment the invoice number appears on the success screen; the end is the email-sent confirmation toast."
- Cross-reference composite SPECs by relative path: `[DataTable](../../composites/data-table/SPEC.md)`.
- Empty-state copy uses CONSTITUTION → Copy → Empty state structure ([What absent] · [Why] · [Recovery]).

## Hand-off

When the module design is complete:

1. Update `.saas-ui/STATE.md` — mark `modules/<name>/` as `designed` with timestamp + tier abbreviation noted.
2. If this module's flows surfaced composite variants, append them to the canonical composite's `VARIANTS.md`.
3. If this module's design discovered a need for a new composite, halt the hand-off and direct the user to bootstrap phase 3 to add it.
4. Suggest the next module by build order from MODULES.md.

## Files in this skill

- `phases/00-preflight.md` — entry checks
- `phases/01-purpose.md` — module deep purpose
- `phases/02-flow-enumeration.md` — entry points + flow list
- `phases/03-flow-design.md` — per-flow step design
- `phases/04-peak-end.md` — Peak-End Rule application
- `phases/05-goal-gradient-zeigarnik.md` — progress + resumability
- `phases/06-serial-position.md` — ordering in lists, dashboards
- `phases/07-flow-attention.md` — Flow state + Selective Attention
- `phases/08-empty-states.md` — module-specific copy
- `phases/09-composite-variants.md` — per-module composite variants
- `phases/10-build-order.md` — execution strategy tuned to EXECUTION-POSTURE
- `phases/11-handoff.md` — STATE update + next module
- `templates/OVERVIEW.md`, `FLOWS.md`, `LAWS.md`, `EMPTY-STATES.md`, `COMPOSITES.md`, `BUILD-ORDER.md`
- `references/module-laws.md` — 6 module-level Laws of UX cheat-sheet
- `references/flow-patterns.md` — canonical flow patterns (linear, branching, multi-step wizard, save-and-resume, single-action)
- `references/empty-state-patterns.md` — empty-state structures by module type
- `references/execution-postures.md` — how SPRINT / BALANCED / CRAFT postures shape build order

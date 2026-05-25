---
name: saas-ui-composite
description: Use when a composite from .saas-ui/COMPOSITES.md needs a deep SPEC — user names a composite to spec, says "spec the X composite" / "build the data table spec", runs /saas-ui-composite, or saas-ui-module flags a missing composite SPEC. Produces composites/<name>/ with SPEC, STATES, KEYBOARD, MOTION, EDGE-CASES, LAWS, VARIANTS. Enforces the Action Inventory contract (anti-duplicate-button) and inventory-as-constraint (no atom invention). Next.js (App Router, SSR+CSR). Pre-flight requires saas-ui-bootstrap complete.
---

# saas-ui-composite

Specs a single composite (DataTable, CommandPalette, FilterBuilder, DetailDrawer, etc.) to implementation depth.

This is where the **Action Inventory contract** lives — the single most important rule in the framework, because duplicate-purpose affordances are the most common LLM UI failure.

## When to invoke

- A composite in `.saas-ui/COMPOSITES.md` is ready for deep spec (atoms present, or gaps resolved)
- The user explicitly names a composite to spec
- A module phase (`saas-ui-module`) flagged a composite that needs spec before flow design
- A failing audit (`saas-ui-audit`) requires re-spec of a composite

## Entry protocol

Before any spec work:

1. **Bootstrap must be complete.** `.saas-ui/STATE.md` must show phases 0–9 done. If not, halt and direct to `saas-ui-bootstrap`.
2. **Target composite must exist in `.saas-ui/COMPOSITES.md`.** If the user names a composite not in the catalog, halt — composites are discovered in bootstrap phase 3, not invented here. Direct the user to revisit COMPOSITES.md.
3. **Atom dependencies must be resolved.** Check the composite's `Atoms required` block. If any atom is `✗ gap` and no resolution exists in `Primitive Gaps`, surface the three options from INVENTORY.md (add primitive / defer composite / substitute) and halt.
4. **Anchors must be readable.** Load `.saas-ui/CONSTITUTION.md`, `.saas-ui/PRINCIPLES.md`, `.saas-ui/INVENTORY.md`, the COMPOSITES.md entry, and any `.saas-ui/references/` annotations tagged for this composite.

If any pre-flight check fails, do not start writing — fix the prerequisite first.

## Iron rules

1. **No atom invention.** Every primitive named in the SPEC must exist in INVENTORY.md by exact name. If a needed primitive is missing, list it in `SPEC.md → Primitive Gaps` and stop — do not invent inline.
2. **No motion redefinition.** Motion durations, easings, and canonical patterns live in CONSTITUTION.md → Motion. `MOTION.md` here records **overrides with argument only**. If there's no override, MOTION.md says "No overrides — CONSTITUTION canonical patterns apply."
3. **Action Inventory is the gate.** Every user-meaningful action in this composite has exactly ONE primary affordance per region. Secondary modalities (keyboard, palette, context-menu) are not duplicates. If two affordances look duplicate, either delete one or argue the semantic distinction under `Argued duplicates`.
4. **Every canonical state declared, every state designed.** STATES.md enumerates the 13 canonical states (see `references/state-catalog.md`). None may be marked "TBD" in a finished SPEC.
5. **Next.js boundary is explicit.** SPEC.md → Rendering names which parts are Server Component, which are Client, where the Suspense boundary sits, and which React 19 primitives (useOptimistic, useFormStatus, useTransition, useDeferredValue) are used and why.
6. **Reason: line for every divergence.** Any rule from CONSTITUTION or PRINCIPLES that this composite stretches must carry a `Reason:` line. No bare overrides.
7. **No code in artifacts.** Pseudocode for state machines is allowed in STATES.md only. Everything else is prose, tables, and rule lists.
8. **Cross-reference, don't restate.** If CONSTITUTION says modal motion is `scale 0.96→1, opacity 0→1, 200ms ease-out`, MOTION.md says "modal: per CONSTITUTION canonical pattern" — not the duration. Restating drifts.

## Phase outline

The skill runs these phases sequentially. Each phase has its own file in `phases/`.

| # | Phase | Output |
|---|---|---|
| 0 | Pre-flight & anchor load | (validation only) |
| 1 | Purpose & contract | `SPEC.md → Purpose, Contract, Non-goals` |
| 2 | Action Inventory | `SPEC.md → Action Inventory` |
| 3 | Anatomy & atoms | `SPEC.md → Anatomy, Primitive Gaps` |
| 4 | State enumeration | `STATES.md` |
| 5 | Keyboard map | `KEYBOARD.md` |
| 6 | Motion overrides | `MOTION.md` |
| 7 | Rendering (Next.js) | `SPEC.md → Rendering` |
| 8 | Edge cases | `EDGE-CASES.md` |
| 9 | Laws application | `LAWS.md` |
| 10 | Variants (if canonical) | `VARIANTS.md` |
| 11 | Checkpoint & hand-off | `.saas-ui/STATE.md` updated |

The user approves each phase's output before the next begins. Composite SPECs cannot be batched — depth is the whole point.

## On-disk layout

```
.saas-ui/
  composites/
    <composite-name>/
      SPEC.md           ← canonical doc for this composite
      STATES.md         ← state enumeration with visual + behavior per state
      KEYBOARD.md       ← keyboard map with conflict checks
      MOTION.md         ← overrides only
      EDGE-CASES.md     ← edge cases with handling rule per case
      LAWS.md           ← composite-level Laws of UX applied here
      VARIANTS.md       ← per-module variants (only if Type = canonical)
```

Names are kebab-case: `data-table/`, `command-palette/`, `filter-builder/`.

## Output discipline

- Every override carries `Reason:`. Every argued duplicate carries `Argue:`. Every primitive gap carries one of `Substitute:`, `Defer:`, `Add to design system:`.
- Never define motion durations here; reference CONSTITUTION canonical patterns by name.
- Never invent atoms; reference INVENTORY.md atoms by exact name.
- Variants live in VARIANTS.md, never inline in SPEC.md. The SPEC is canonical.

## Hand-off

When the composite SPEC is complete:

1. Update `.saas-ui/STATE.md` — mark `composites/<name>/` as `spec'd` with timestamp.
2. If this composite was blocking modules in MODULES.md, surface the unblocked list — those modules can now proceed via `saas-ui-module`.
3. If the SPEC discovered new atom gaps not previously in INVENTORY.md, append them to INVENTORY.md → Gaps observed (with priority + blocked composites list).
4. Suggest the next composite to spec, ordered by COMPOSITES.md → Build order suggestion.

## Files in this skill

- `phases/00-preflight.md` — entry checks & anchor load
- `phases/01-purpose-contract.md` — purpose, contract, non-goals
- `phases/02-action-inventory.md` — **the duplicate-button gate**
- `phases/03-anatomy-atoms.md` — atom binding & primitive gaps
- `phases/04-states.md` — 13 canonical states
- `phases/05-keyboard.md` — keyboard map with conflict detection
- `phases/06-motion.md` — overrides only, no redefinition
- `phases/07-rendering.md` — Next.js SSR/CSR boundary & React 19 primitives
- `phases/08-edge-cases.md` — edge cases catalog
- `phases/09-laws.md` — composite-level Laws of UX
- `phases/10-variants.md` — per-module variants (canonical composites only)
- `templates/SPEC.md`, `STATES.md`, `KEYBOARD.md`, `MOTION.md`, `EDGE-CASES.md`, `LAWS.md`, `VARIANTS.md`
- `references/state-catalog.md` — the 13 canonical states with definitions
- `references/composite-laws.md` — composite-level Laws of UX cheat-sheet
- `references/nextjs-primitives.md` — when to use which React 19 / Next.js primitive

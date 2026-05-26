# Roadmap

The framework is planned as five skills. Two shipped; three to go. This document tracks intent — actual shipping order may shift based on what real projects need first.

---

## Shipped

### `saas-ui-bootstrap` — v0.1.0 / v0.3.0 (2026-05-26)

11-phase project initialization interview (v0.3.0 added Phase 10: Execution Posture). Produces the canonical planning artifacts under `.saas-ui/`:

- `CONSTITUTION.md` — Motion, Density, Copy, State Model, Forbidden Patterns, Action Affordances
- `PRINCIPLES.md` — 8 project-level Laws of UX applied to the specific product
- `INVENTORY.md` — Discovered design-system atoms / molecules / gaps (read-only constraint)
- `MODULES.md` — Module catalog with tiers (anchor / occasional / rare)
- `COMPOSITES.md` — Composite catalog with build order
- `EXECUTION-POSTURE.md` — Ship-pace + fidelity commitment (SPRINT / BALANCED / CRAFT)
- `STATE.md` — Phase tracking

### `saas-ui-composite` — v0.2.0 / v0.3.0 (2026-05-26)

12-phase per-composite deep SPEC generator (v0.3.0 added Phase 11: Implementation Strategy). For each composite in `.saas-ui/COMPOSITES.md`, produces `composites/<name>/`:

- `SPEC.md` — purpose, contract, non-goals, action inventory, anatomy, atoms in use, primitive gaps, rendering (Next.js boundary, Suspense, React 19 primitives), composition rules
- `STATES.md` — 13 canonical states + cross-state rules + optional state machine
- `KEYBOARD.md` — shortcuts, conflict resolutions, focus contract, escape ladder, ARIA roles
- `MOTION.md` — overrides only (canonical from CONSTITUTION)
- `EDGE-CASES.md` — canonical checklist with per-case handling rules
- `LAWS.md` — 11 composite-level Laws of UX applied with verification rules
- `VARIANTS.md` — per-module deltas (canonical composites only)
- `IMPLEMENTATION.md` — MVP/V1/CRAFT cuts, build order, parallelization, defer list, test priority, stub strategy — tuned to EXECUTION-POSTURE

This skill is where the **Action Inventory contract** lives — the single most important rule in the framework. It catches duplicate-purpose affordances before any code is written.

### `saas-ui-module` — v0.3.0 (2026-05-26)

12-phase per-module flow design. For each module in `.saas-ui/MODULES.md`, produces `modules/<name>/`:

- `OVERVIEW.md` — module purpose, tier, polish budget, entry points, composite readiness
- `FLOWS.md` — per-flow design (entry → steps → branches → exits) for each of 5 canonical patterns (linear, branching, save-and-resume, single-action, continuous)
- `LAWS.md` — module-level Laws of UX:
  - **Peak-End Rule** — identified peak + end moments per primary flow; polish budget concentrated there
  - **Goal-Gradient Effect** — progress indicators tuned by step count; accelerator design
  - **Zeigarnik Effect** — save-and-resume contracts; module-home incomplete-task pinning
  - **Serial Position Effect** — primacy/recency ordering in nav, dashboards, menus, settings
  - **Flow** — interruption budgets per flow; modal discipline
  - **Selective Attention** — one winner per screen; notification placement strategy
- `EMPTY-STATES.md` — module-specific empty / error / loading copy with 8 canonical patterns
- `COMPOSITES.md` — module-scoped composite index with cross-variant consistency check
- `BUILD-ORDER.md` — flow priority cuts by posture, per-flow MVP/V1/CRAFT cuts, critical path, parallel streams, stub strategy

Polish budget governance: anchor modules get full 12-phase pass; occasional abbreviate phases 9–11; rare modules abbreviate phases 4–11 with reasons.

---

## Planned

### `saas-ui-status` (next)

Lightweight orchestrator. Reads `.saas-ui/STATE.md` and surfaces:

- Which phase of bootstrap is incomplete (if any)
- Which composites are spec'd vs unspec'd
- Which composites are blocked by atom gaps
- Which modules are ready to design vs blocked by missing composites
- Suggested next action

Single-command status check: `/saas-ui-status`. Outputs a one-screen summary.

**Estimated effort:** ~1 week. This is mostly read-only orchestration.

---

### `saas-ui-audit` (last; the critic)

The linter. Takes any artifact (`SPEC.md`, `STATES.md`, implementation code) and checks against:

- `CONSTITUTION.md` — does the artifact comply with motion, density, copy, state model, forbidden patterns?
- `PRINCIPLES.md` — are the project-level laws upheld?
- `INVENTORY.md` — are all referenced atoms in inventory? Any atom invention?
- `common-llm-ui-mistakes.md` — does the artifact ship any of the 17 known pathologies?

Outputs a scorecard per artifact with pass / fail / argued-deviation per check.

Two modes:
- **Pre-implementation audit** — checks artifacts before code is written
- **Post-implementation audit** — checks code against artifacts

The post-implementation audit is what catches the moment when an LLM-implemented composite drifts from its SPEC.

**Estimated effort:** ~4 weeks. This is the most complex skill because every rule needs a verification strategy.

---

## Not on the roadmap (yet)

- **Stack ports** — Remix, SvelteKit, Astro, Tanstack Start. Each would need a fork that swaps the rendering phase. Open issues with concrete use cases before forking.
- **Component library** — the framework deliberately doesn't ship React code. If you want a component library, use `shadcn/ui` or `Radix` and let this framework be the planning layer above it.
- **AI-generated screenshots / mocks** — the framework consumes references via annotation schema, but doesn't generate them. Use a dedicated tool (`v0`, `Stitch`, etc.) and feed the output back as references.
- **A "fast path" that skips phases** — the framework's value is the depth. Shortcuts defeat the purpose. If a project doesn't need this depth, it probably doesn't need this framework.

---

## How priorities shift

Roadmap order is influenced by:

1. **What real projects hit first.** If three battle-tests in a row report "we kept ending up with module-level flow gaps," `saas-ui-module` jumps the queue.
2. **What audits catch.** The need for `saas-ui-audit` will sharpen as more composites get spec'd and ship.
3. **What surprises emerge.** A pathology that wasn't on the 17-list, observed twice, becomes #18 and shapes the audit's rule set.

File issues describing what you'd want most. The framework is small enough that the next skill can pivot based on real-world need.

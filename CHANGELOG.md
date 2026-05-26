# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- `saas-ui-status` — lightweight orchestrator that reads `.saas-ui/STATE.md` and reports posture, composite SPEC status, module readiness, next-action suggestion
- `saas-ui-audit` — linter for any artifact against CONSTITUTION, INVENTORY, and the 17 LLM pathologies; posture-scaled strictness

---

## [0.3.0] — 2026-05-26

### Added
- **Execution-strategy layer** across the framework — every downstream artifact now ships tuned to the project's commitment about ship-pace and fidelity.
  - `saas-ui-bootstrap` Phase 10: Execution Posture interview. Produces `EXECUTION-POSTURE.md` with one of three postures (SPRINT / BALANCED / CRAFT), seven commitments (ship horizon, validation target, team shape, quality bar, deferral tolerance, polish vs scope, known deferrals), and re-evaluation trigger.
  - `saas-ui-composite` Phase 11: Implementation Strategy. Produces `IMPLEMENTATION.md` per composite with MVP/V1/CRAFT cuts, build order within composite (11 stages), parallelization hints, defer list, test priority, stub strategy.
  - Reference documents: `execution-postures.md` in both `saas-ui-composite` and `saas-ui-module` skills, explaining how each posture shapes decisions at that level.
  - Audit posture-scaling: `saas-ui-audit` (planned) will scale strictness to the committed posture — strict on functional rules always; lenient on polish-tier rules under SPRINT.

- **`saas-ui-module` skill (v0.3.0)** — 12-phase per-module flow design
  - 12 phases: pre-flight → purpose → flow enumeration → per-flow design → Peak-End → Goal-Gradient & Zeigarnik → Serial Position → Flow & Selective Attention → empty/error states → composite variant cross-references → **build order & implementation strategy** → hand-off
  - 6 templates: OVERVIEW, FLOWS, LAWS, EMPTY-STATES, COMPOSITES, BUILD-ORDER
  - 4 reference documents: module-laws (6 module-level Laws of UX with checks), flow-patterns (5 canonical: linear, branching, save-and-resume, single-action, continuous), empty-state-patterns (8 patterns with anti-pattern catalog), execution-postures
  - Module-level Laws of UX applied: Peak-End, Goal-Gradient, Zeigarnik, Serial Position, Flow, Selective Attention
  - Polish budget governance: anchor / occasional / rare modules get different phase abbreviation policies

### Changed
- `saas-ui-bootstrap` README, SKILL.md, STATE.md template updated for 11 phases (was 10)
- `saas-ui-composite` SKILL.md updated for 12 phases (was 11)
- Both skills now consume `EXECUTION-POSTURE.md`; halt if missing when running phase 11 (composite) or phase 10 (module)

---

## [0.2.0] — 2026-05-26

### Added
- `saas-ui-composite` skill — per-composite deep SPEC generator
  - 11-phase workflow (pre-flight → purpose → action inventory → anatomy → states → keyboard → motion → rendering → edge cases → laws → variants)
  - Action Inventory contract enforcement (the anti-duplicate-button gate)
  - Inventory-as-constraint enforcement (no atom invention)
  - 13 canonical states catalog
  - Composite-level Laws of UX cheat-sheet (Fitts, Miller, Hick, Von Restorff, Working Memory, Chunking, 5 Gestalt laws, Prägnanz)
  - Next.js / React 19 primitives reference (useOptimistic, useFormStatus, useTransition, useDeferredValue, Server Actions, Suspense topology)
  - 7 output templates (SPEC, STATES, KEYBOARD, MOTION, EDGE-CASES, LAWS, VARIANTS)

---

## [0.1.0] — 2026-05-26

### Added
- Initial release
- `saas-ui-bootstrap` skill — 10-phase project initialization interview
  - Phase 0: Design system discovery (Branch A: existing system / Branch B: intent-only) → `INVENTORY.md`
  - Phase 1: Product definition → `CONSTITUTION.md` Product section + `PRINCIPLES.md` seeds
  - Phase 2: Module discovery via "first week" interview → `MODULES.md`
  - Phase 3: Composite discovery via cross-module pattern extraction → `COMPOSITES.md`
  - Phase 4: Motion philosophy and Doherty budget → `CONSTITUTION.md` Motion section
  - Phase 5: Density posture and Aesthetic-Usability floor → `CONSTITUTION.md` Density section
  - Phase 6: Copy voice and forbidden phrasings → `CONSTITUTION.md` Copy section
  - Phase 7: State Model (loading, mutation, autosave, error, offline, recovery) + Postel's Law → `CONSTITUTION.md` State section
  - Phase 8: 17 forbidden LLM-UI patterns + Action Affordances contract → `CONSTITUTION.md` Forbidden + Action Affordances sections
  - Phase 9: External references and annotation schema → `references/` directory
- Laws of UX reference document with 30 laws mapped to per-level rubrics (project, module, composite, cross-cutting)
- Common LLM UI Mistakes reference document with 17 pathologies
- 6 output templates (CONSTITUTION, PRINCIPLES, INVENTORY, MODULES, COMPOSITES, STATE)
- MIT license, README, CONTRIBUTING, ROADMAP

[Unreleased]: https://github.com/coder-ishan/saas-ui-framework/compare/v0.3.0...HEAD
[0.3.0]: https://github.com/coder-ishan/saas-ui-framework/releases/tag/v0.3.0
[0.2.0]: https://github.com/coder-ishan/saas-ui-framework/releases/tag/v0.2.0
[0.1.0]: https://github.com/coder-ishan/saas-ui-framework/releases/tag/v0.1.0

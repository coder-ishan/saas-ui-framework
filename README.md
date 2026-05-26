# saas-ui-framework

> A multi-skill Claude Code framework for building enterprise SaaS UIs with the meticulousness of Linear and the extensiveness of ClickUp.

**Status:** All five skills shipped (plus the cross-cutting execution-strategy layer). Battle-testing in progress.
**Stack:** Next.js (App Router, SSR+CSR). React 19.
**Author:** [@coder-ishan](https://github.com/coder-ishan)

---

## Why this exists

When you ask Claude Code to build a SaaS UI, it generates *plausible* UI — coherent at a glance, but missing the depth, taste, and judgment that distinguish polished products like Linear, Notion, Stripe Dashboard, or ClickUp from forgettable ones.

The default LLM failure modes are predictable:
- **Duplicate-purpose affordances** — two buttons that do the same thing, in the same region
- **Generic empty states** — "No data" with no context, no recovery
- **Confirmation modal addiction** — "Are you sure?" instead of undo toasts
- **Decorative motion** — bouncy animations on functional actions
- **Spinner overuse** — full-screen spinners instead of skeleton loading
- **State holes** — `loading-mutation`, `partial-permission`, `filter-empty` are missing in 90% of LLM-generated UI
- **Atom invention** — ignoring the existing design system and creating one-off primitives inline
- (12 more pathologies — full list in `saas-ui-bootstrap/references/common-llm-ui-mistakes.md`)

This framework externalizes UI taste into **persistent planning artifacts** — a Design Constitution, a Principles document, a Design System Inventory, Module Catalog, and Composite Catalog. Future Claude Code sessions inherit depth from those artifacts rather than rebuild toward the average.

It's structured as a sequence of skills that mirror how a thoughtful design-engineer would actually plan an enterprise SaaS product:

1. **`saas-ui-bootstrap`** — initial interview that produces the constitution, catalogs, and execution-posture commitment (shipped, v0.3.0)
2. **`saas-ui-composite`** — per-composite deep SPEC + implementation strategy (DataTable, CommandPalette, etc.) — shipped, v0.3.0
3. **`saas-ui-module`** — per-module flow design + build order — shipped, v0.3.0
4. **`saas-ui-status`** — lightweight, read-only orchestrator that reports project state and prioritizes next actions — shipped, v0.4.0
5. **`saas-ui-audit`** — the critic; lints artifacts and code against the constitution with posture-scaled strictness — shipped, v0.4.0

**Cross-cutting:** v0.3.0 added the **execution-strategy layer** — a single posture commitment (SPRINT / BALANCED / CRAFT) in bootstrap that flows downstream into composite IMPLEMENTATION.md and module BUILD-ORDER.md. Same design artifacts → different implementation cuts depending on what the team is shipping for.

The framework is influenced by GSD's architectural pattern of multi-skill workflows with shared state directories.

---

## What's in here

| Skill | Status | Purpose |
|---|---|---|
| [`saas-ui-bootstrap/`](saas-ui-bootstrap/) | ✅ v0.3.0 | 11-phase interview producing `.saas-ui/CONSTITUTION.md`, `PRINCIPLES.md`, `INVENTORY.md`, `MODULES.md`, `COMPOSITES.md`, `EXECUTION-POSTURE.md`, `STATE.md`. |
| [`saas-ui-composite/`](saas-ui-composite/) | ✅ v0.3.0 | 12-phase per-composite deep SPEC + implementation strategy. Produces `composites/<name>/{SPEC, STATES, KEYBOARD, MOTION, EDGE-CASES, LAWS, VARIANTS, IMPLEMENTATION}.md`. Enforces the Action Inventory contract (anti-duplicate-button). |
| [`saas-ui-module/`](saas-ui-module/) | ✅ v0.3.0 | 12-phase per-module flow design. Module-level Laws of UX (Peak-End, Goal-Gradient, Zeigarnik, Serial Position, Flow, Selective Attention). Produces `modules/<name>/{OVERVIEW, FLOWS, LAWS, EMPTY-STATES, COMPOSITES, BUILD-ORDER}.md`. |
| [`saas-ui-status/`](saas-ui-status/) | ✅ v0.4.0 | Lightweight, read-only orchestrator. Reads STATE + EXECUTION-POSTURE + catalogs and prints a single-screen project status with prioritized next-action list. |
| [`saas-ui-audit/`](saas-ui-audit/) | ✅ v0.4.0 | The critic. Lints artifacts and code against CONSTITUTION, INVENTORY, 17 LLM pathologies, and composite/module Laws of UX. Posture-scaled strictness; functional rules always strict. |

See [ROADMAP.md](ROADMAP.md) for the future skills.

---

## Quick start

### 1. Install

```bash
# Clone the repo
git clone https://github.com/coder-ishan/saas-ui-framework.git
cd saas-ui-framework

# Copy the skills you want into your Claude Code skills directory
cp -r saas-ui-bootstrap ~/.claude/skills/
cp -r saas-ui-composite ~/.claude/skills/
cp -r saas-ui-module ~/.claude/skills/
cp -r saas-ui-status ~/.claude/skills/
cp -r saas-ui-audit ~/.claude/skills/
```

### 2. Use

In Claude Code, in your SaaS project directory:

```
/saas-ui-bootstrap
```

Claude will conduct a chunked, resumable interview that produces canonical planning artifacts under `.saas-ui/`. Don't expect it to write code — this phase produces the planning depth that prevents the LLM from generating mediocre UI later.

Once bootstrap is complete, for each composite you want to spec deeply:

```
/saas-ui-composite DataTable
```

This runs the 12-phase per-composite workflow (SPEC, STATES, KEYBOARD, MOTION, EDGE-CASES, LAWS, VARIANTS, IMPLEMENTATION). The output is read by future Claude sessions when they implement that composite.

For each module you want to design at flow level:

```
/saas-ui-module Inbox
```

This runs the 12-phase per-module workflow (OVERVIEW, FLOWS, LAWS, EMPTY-STATES, COMPOSITES, BUILD-ORDER) with module-level Laws of UX applied (Peak-End, Goal-Gradient, Zeigarnik, etc.).

To check your place in the workflow at any time:

```
/saas-ui-status
```

This prints a single-screen report: bootstrap progress, posture freshness, composite SPEC status, module readiness, atom gaps, and a prioritized next-action list tuned to your current posture.

Before merging code or shipping, audit it:

```
/saas-ui-audit composite/DataTable       # pre-implementation audit of the SPEC
/saas-ui-audit code:src/components/DataTable.tsx  # post-implementation audit
/saas-ui-audit                            # project-wide
```

The audit produces a scorecard with a SHIP / NEEDS-FIXES / NEEDS-REVIEW verdict, finding-by-finding severity, and concrete remediation steps. Strictness scales to your committed posture — functional rules (atom invention, duplicate buttons, generic empty states, 17 pathologies) are always strict; polish-tier rules ease under SPRINT and tighten under CRAFT.

### 3. The artifact set

After running the three skills, your project has:

```
.saas-ui/
├── CONSTITUTION.md          ← Design constitution (motion, density, copy, state model, forbidden patterns)
├── PRINCIPLES.md            ← Project-level Laws of UX applied to your product
├── INVENTORY.md             ← Design system as a read-only constraint set
├── MODULES.md               ← Module catalog with tiers (anchor/occasional/rare)
├── COMPOSITES.md            ← Composite catalog with build order
├── EXECUTION-POSTURE.md     ← Ship-pace + fidelity commitment (SPRINT/BALANCED/CRAFT)
├── STATE.md                 ← Phase tracking
├── composites/
│   ├── data-table/
│   │   ├── SPEC.md
│   │   ├── STATES.md
│   │   ├── KEYBOARD.md
│   │   ├── MOTION.md
│   │   ├── EDGE-CASES.md
│   │   ├── LAWS.md
│   │   ├── VARIANTS.md
│   │   └── IMPLEMENTATION.md ← MVP/V1/CRAFT cuts tuned to your posture
│   └── command-palette/
│       └── ...
├── modules/
│   ├── inbox/
│   │   ├── OVERVIEW.md
│   │   ├── FLOWS.md
│   │   ├── LAWS.md
│   │   ├── EMPTY-STATES.md
│   │   ├── COMPOSITES.md
│   │   └── BUILD-ORDER.md   ← Per-flow MVP cuts, critical path, parallel streams
│   └── customers/
│       └── ...
└── references/              ← Annotated screenshots and external reference material
```

These artifacts are the source of truth. Implementation code reads from them. Audits check against them. Future sessions inherit them.

**The execution-strategy layer** (v0.3.0) is what lets the framework adapt to your team's actual constraints. The same SPEC ships differently under SPRINT (MVP for demo), BALANCED (V1 for paying customers), or CRAFT (Linear-grade for established product). Posture is committed once in bootstrap and flows downstream into every implementation decision.

---

## Design principles of the framework itself

1. **Externalize taste into artifacts.** A design constitution is more durable than chat history.
2. **Inventory-as-constraint.** Composites reference atoms by exact name. Missing atoms go in `Primitive Gaps`, never invented inline.
3. **One primary affordance per region.** The Action Inventory contract catches duplicate-button generation before it ships.
4. **Reason: line for every rule.** When edge cases arise, return to the reason, not the letter.
5. **Cross-reference, don't restate.** Motion durations live in CONSTITUTION; composite MOTION.md references them. Restating creates drift.
6. **Phases are resumable.** Long interviews are chunked. State is on disk. You can stop and resume across days.
7. **No code in artifacts.** Pseudocode for state machines only. Everything else is prose, tables, and rules — readable by humans and LLMs alike.
8. **Stack-specific.** Next.js App Router (SSR+CSR) is the assumed substrate. The artifacts call out Server vs Client Component boundaries, Suspense topology, and React 19 primitives. Other stacks need a fork.

---

## What this framework is NOT

- **Not a component library.** It doesn't ship React code. It ships planning skills that produce design artifacts.
- **Not a design system.** It expects you to have (or want to build) one. The Inventory phase discovers what you already have.
- **Not a substitute for taste.** It externalizes the user's taste; it doesn't manufacture taste from nothing. The interview phases are the moment where taste enters the system.
- **Not stack-agnostic.** Next.js App Router is baked in. Other stacks would need a fork that swaps the rendering phase.
- **Not a replacement for designers.** A solo founder or design-engineer can use it to get to Linear-quality. A team with dedicated designers should use it to harmonize the team's decisions and keep AI assistants on-rail.

---

## Influences

- **GSD** (`~/.claude/skills/gsd-*`) — multi-skill workflows with shared state directories, phase orchestration, atomic artifacts
- **Laws of UX** by Jon Yablonski — the 30-law rubric layer
- **Linear** — opinionated minimalism, keyboard-first, atomic primitives
- **ClickUp** — extensiveness with discipline, polish at scale
- **Stripe Dashboard** — density without clutter, error copy that names failures
- **Notion** — composite reuse with per-context variants

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Adding a skill, refining a phase, or improving a reference doc are all welcome. The bar for the LLM-pathology reference is: the pathology must be observed in real LLM output, and the prevention rule must be enforceable in audit.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Status & versioning

This is v0.x. The skills will evolve as the remaining three are built and as the first two are battle-tested on real projects. CHANGELOG tracks user-visible changes.

If you adopt the framework on a project, please file issues for: pathologies the framework didn't prevent, rules that were unclear, and phases that felt over- or under-scoped.

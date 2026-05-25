# Phase 4 — Motion

**Purpose:** Establish the motion language — durations, easings, what motion *means* in this product, what's forbidden. Motion is one of the cheapest places to ship slop and one of the highest-leverage places to ship polish.

**Produces:** `.saas-ui/CONSTITUTION.md` (Motion section) + `.saas-ui/PRINCIPLES.md` (Doherty section finalized)

**Estimated time:** 10–15 min

## Interview prompts

### 1. The latency budget (Doherty)
> What is your product's latency budget for perceived feedback on user actions? Specifically:
> - **Sort, filter, search, status change** — should feel instant. What's the ms budget?
> - **Page navigation** — can it slip into seconds, or do you want SSR-streamed feel?
> - **Save / mutate** — instant via optimistic UI with rollback, or wait-and-confirm?
>
> Be concrete in milliseconds, not vibes.

Default to lock in if user is uncertain: <400ms for interactions, <800ms for navigation, optimistic-with-rollback for mutations.

### 2. Animation philosophy
> Three options. Pick one:
> - **Functional only** — motion only when it clarifies cause-effect (state change, dismissal direction, layered ordering). No decoration.
> - **Functional + restrained delight** — functional motion plus 1-2 signature moments per module (a satisfying success animation, a subtle hero load). Used sparingly.
> - **Expressive** — motion as personality (Notion-ish). Animation present in many small ways.
>
> Most enterprise SaaS picks #1. Don't pick #3 unless your product has a creative/consumer flavor.

### 3. Durations
> What are your default durations?
> - State changes (hover, focus, selection): __ ms (suggest 120–180)
> - Dismissal / appearance (modals, sheets, popovers): __ ms (suggest 180–250)
> - Layered transitions (page changes, view switches): __ ms (suggest 200–300)
> - Anything above 300ms must be argued for.

### 4. Easings
> What easings? Options:
> - ease-out for appearance, ease-in for dismissal (most common)
> - custom cubic-bezier (specify)
> - spring physics (only if expressive philosophy)
>
> Most enterprise SaaS uses ease-out + ease-in or a single custom cubic-bezier like `cubic-bezier(0.16, 1, 0.3, 1)`.

### 5. Forbidden motion patterns
> Confirm:
> - No bounce / overshoot in functional UI (unless explicit exception)
> - No fade-only transitions — always pair with a 2–4px transform
> - No animations longer than 300ms by default
> - No simultaneous animations on >3 elements (looks chaotic)
> - No animations on scroll-triggered enter (jank risk; only for hero moments)
>
> Any of these you want to override? With what rationale?

### 6. Motion meaning (mapping)
> Map gestures to motion. For each, what's the canonical motion?
> - Modal/dialog open: <e.g., scale 0.95 → 1 + fade in, 200ms ease-out>
> - Side panel / sheet open: <e.g., translateX from edge, 220ms ease-out>
> - Popover/dropdown: <e.g., translateY 4px + fade, 150ms ease-out>
> - Toast appearance: <e.g., translateY 8px + fade, 180ms ease-out>
> - Row expand/collapse: <e.g., height auto-anim, 200ms ease-out>
> - Status change (badge update): <e.g., subtle scale ping, 150ms>
>
> These become the canonical motion patterns referenced by every composite SPEC.

## What to write

### CONSTITUTION.md — Motion section

```markdown
## Motion

**Philosophy:** functional only | functional + restrained delight | expressive
  Reason: <user's rationale>

**Latency budget (Doherty):**
- Interactions (sort/filter/search/status): <ms>
- Navigation: <ms>
- Mutations: optimistic with rollback | wait-and-confirm
  Reason: <user's rationale>

**Durations:**
- State changes: <ms>
- Dismissal / appearance: <ms>
- Layered transitions: <ms>
- Above-default rule: <e.g., "anything above 300ms must be argued for in the SPEC">

**Easings:**
- Default: <e.g., cubic-bezier(0.16, 1, 0.3, 1) for ease-out>
- Dismissal: <e.g., cubic-bezier(0.7, 0, 0.84, 0) for ease-in>
- Spring: <only if expressive; specify config>

**Canonical motion patterns:**
- Modal open: <pattern>
- Sheet open: <pattern>
- Popover/dropdown: <pattern>
- Toast: <pattern>
- Row expand: <pattern>
- Status change: <pattern>
- <other patterns specific to this product>

**Forbidden:**
- <items confirmed in Q5, with any user-specific overrides>
```

### PRINCIPLES.md — Doherty section (finalize)

```markdown
## 1. Doherty Threshold (<400ms)

**Project stance:**
- Interaction latency budget: <ms from Q1>
- Navigation latency budget: <ms from Q1>
- Mutation strategy: <optimistic-with-rollback | wait-and-confirm>

**Implementation patterns (Next.js):**
- <e.g., Streaming SSR + Suspense for initial paint <400ms>
- <e.g., useOptimistic + toast-with-undo for mutations>
- <e.g., useDeferredValue for search-as-you-type>

**Where we accept slippage and why:**
- <e.g., report generation can take 2-10s with progress + cancel>
```

## Success criteria

Phase 4 is complete when:
1. CONSTITUTION.md Motion section is fully populated with durations, easings, canonical patterns.
2. PRINCIPLES.md Doherty section is finalized with concrete ms numbers and Next.js implementation patterns.
3. Forbidden motion patterns are confirmed (or overridden with rationale).
4. STATE.md updated: `phase_4_complete: true`, `current_phase: 5`.

## What NOT to do

- Don't accept "feels right" as duration. Get numbers.
- Don't skip Q6 (motion meaning mapping) — this is the most consumed section by future composite SPECs.
- Don't allow "all three" for philosophy (Q2). Force a single choice.
- Don't pick spring physics for functional UI without explicit user override.

## Checkpoint

When phase 4 completes, ask: "Motion language set. Continue to phase 5 (density), or pause here?"

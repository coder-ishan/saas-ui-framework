# Phase 3 — Per-flow design

**Purpose:** For each P0 flow (and abbreviated for P1/P2), write the step-by-step design. This is the longest phase and the heart of module design.

**Produces:** `FLOWS.md` → flow specs.

**Estimated time:** 15–25 minutes per P0 flow. P1/P2 flows: 5–10 minutes each.

---

## Per-flow template

For each flow from phase 2, produce:

```markdown
## Flow: <name>

- **Pattern:** linear / branching / save-resume / single-action / continuous
- **Goal:** <user-facing goal — what success looks like>
- **Priority:** P0 / P1 / P2
- **Composites used:** <list with cross-links>

### Pre-conditions

What must be true for the flow to start:
- <e.g., "User has at least one record"; "User has billing permission">
- <e.g., "Inbox is non-empty">

### Steps

| # | Step | What user does | What system does | Composite | State (per composite STATES.md) |
|---|---|---|---|---|---|
| 1 | Entry | <action that brings them here> | <render decision> | <composite> | idle |
| 2 | <scan / decide> | <interaction> | <response> | <composite> | hover / focus |
| 3 | <act> | <interaction> | <response> | <composite> | loading-mutation |
| 4 | <confirm> | <interaction> | <response> | <composite> | idle (post-state) |
| 5 | Exit | <how they leave or loop> | | | |

### Branches (if pattern = branching)

```text
Step 3 (decision point):
  ├─ Choice A → Step 3a → Step 4 → Exit (success-A)
  ├─ Choice B → Step 3b → Step 4 → Exit (success-B)
  └─ Cancel  → Exit (abandon)
```

### Save-and-resume contract (if pattern = save-resume)

- **What's persisted:** <list of state pieces>
- **When persisted:** <every keystroke / every step / explicit save>
- **Resume entry point:** <URL or notification — how user gets back>
- **Resume UI:** <what they see — "Resume where you left off" CTA + which step>
- **Reason:** Zeigarnik — incomplete tasks pull attention; let user act on the pull, not lose work.

### Decision affordances (if pattern = branching)

For each decision point:
- **The choices** must be visible simultaneously (no progressive reveal that hides options).
- **The most common choice** is the visually primary affordance per CONSTITUTION → Action Affordances.
- **The destructive or terminal choice** uses the destructive variant per CONSTITUTION.
- **Cancel/back** is always available; never trap the user.

### Keyboard path

The same flow, expressed in keyboard:
1. Entry: <how they arrive via keyboard — palette? shortcut?>
2. Navigate to step 2: <key>
3. Act in step 3: <key>
4. Confirm step 4: <key>
5. Exit: <key>

If the entire flow can be done from keyboard, document it. If not, identify the gap and either accept (mouse-only step, with reason) or escalate to composite SPEC.

### Latency budget for this flow

From CONSTITUTION → Motion → Latency budget:
- Step 1 → 2 transition: <ms>
- Step 2 → 3 (the mutation): <ms perceived, with optimistic UI>
- Step 4 → exit: <ms>

If any step exceeds the latency budget, name the mitigation (progress indicator, skeleton, optimistic UI).

### Success exit copy

When the user completes the flow:
- **Confirmation:** <single line; CONSTITUTION → Copy → Confirmation structure: action past tense · subject · undo if applicable>
- **Where it appears:** toast (action-source has left viewport) / inline (action source still visible) / persistent banner (long-running)
- **What happens next:** <stay in module / navigate elsewhere / continuation prompt>

### Abandon exit (graceful)

If the user cancels mid-flow:
- **What's preserved:** <draft state? selection? nothing?>
- **Where they land:** <back to flow start / module home / wherever they came from>
- **Indication:** <none / toast "Discarded" with undo / silent>

### Error exits

For each step that can fail:
- **Step <N> error:** <what failure looks like, recovery path, copy per CONSTITUTION → State Model error handling by scope>
```

---

## Per-pattern emphasis

### Linear flow

Emphasize:
- Progress indicator visible at every step (Goal-Gradient)
- Back is always available (not just browser back — explicit "Back" affordance)
- No surprise modal interruptions mid-flow (Flow)

### Branching flow

Emphasize:
- All branches visible at decision point
- Clear visual hierarchy for the recommended choice (Von Restorff at composite level — but the decision is module-level)
- "Default" choice obvious — many users will accept default

### Save-and-resume flow

Emphasize:
- Persistence contract is explicit
- Resume entry point is durable (URL or notification, not "the page you were on")
- Resume UI doesn't make user re-traverse completed steps

### Single-action flow

Emphasize:
- Keyboard primary affordance — most single-action flows are best as shortcuts
- Optimistic UI — single actions should feel instant
- Undo affordance for destructive single actions

### Continuous flow

Emphasize:
- Fatigue management: no captcha, no friction per action
- Batch actions when patterns emerge ("Selected 12 items: Archive all?")
- Natural rest points (no infinite scroll without scroll-position memory)

---

## Success criteria for phase 3

- Every P0 flow has a complete step table
- Every P1 flow has at minimum: pattern, steps (terse), exit
- Every P2 flow has at minimum: pattern, one-line description
- Every step references a composite from OVERVIEW.md and a state from that composite's STATES.md
- Every branching flow has its branch tree
- Every save-and-resume flow has its persistence contract
- Every flow has a success exit, abandon exit, and error exits per step

---

## What NOT to do in phase 3

- **Don't design flows in prose.** Step tables are required. Prose hides ambiguity.
- **Don't invent new states.** Reference states from composite STATES.md by name. New states require a composite SPEC update.
- **Don't reuse copy templates without filling them in.** "Saved." is not error-handled copy; CONSTITUTION's structure must be applied.
- **Don't skip error exits per step.** Each step that can fail needs an error exit. "Generic error handling" defeats the audit.

---

## Checkpoint per flow

> "Three checks per flow:
> 1. Walk through the steps. Does each step correspond to one mental moment, or does a step actually bundle 2–3 sub-actions? Split bundled steps.
> 2. Are the composite-state references valid? (Each step's State column must exist in that composite's STATES.md.)
> 3. Are error exits per step proportionate to risk? Single-action flows can collapse errors into one toast; multi-step flows need per-step recovery."

After all flows: phase 4.

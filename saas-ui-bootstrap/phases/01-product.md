# Phase 1 — Product

**Purpose:** Establish the product foundation that all downstream artifacts depend on: who the user is, what the product does, what the wedge is, what mental model the UI inherits/breaks, and the cognitive-load posture.

**Produces:** `.saas-ui/CONSTITUTION.md` (Product section) + `.saas-ui/PRINCIPLES.md` (initial seeds for Mental Model, Cognitive Load, Jakob, Tesler)

**Estimated time:** 15–25 min

## Why this phase

Module and composite catalogs only make sense in the context of the product. "What is the inbox for?" depends on what the product is. This phase anchors all downstream phases.

## Interview prompts

Ask these conversationally. Don't speed-run with AskUserQuestion — the answers should be substantive. After each, write your understanding back to the user in 2-3 sentences and confirm before moving on.

### 1. What is this product?
- One-sentence pitch (no jargon).
- Who is the primary user role? (role, seniority, daily routine context)
- What is the core job-to-be-done?

### 2. Where does this live in the user's workday?
- Is this the user's *primary* app (8+ hours/day, e.g., Linear for an eng team) or a *visited* app (5 minutes/day, e.g., a billing portal)?
- This determines polish budget allocation and keyboard-first vs mouse-first design.

### 3. What's the wedge?
- What does this product do *uniquely well* that the alternatives don't?
- The wedge informs which 20% of flows get 80% of the polish (Pareto, written to PRINCIPLES.md).

### 4. What products is your user already fluent in?
- Name 3-5 SaaS products your typical user uses daily.
- This sets the Jakob's Law inheritance baseline. We will match conventions from these unless we have a specific reason not to.
- Ask: "are there conventions from these you specifically want to *break*? Why?"

### 5. What's the mental model?
- What metaphor does the product use? (inbox, canvas, board, spreadsheet, graph, timeline, document, feed)
- Are you borrowing a model from a familiar product? (Gmail-like, Linear-like, Notion-like, Excel-like)
- What models are you *breaking* — and is the break worth the cognitive cost?

### 6. Cognitive-load posture
Ask directly:
> Are you leaning Linear (opinionated, minimalist, the app decides) or ClickUp (configurable, maximalist, the user decides)? There is no third option — pick one and we'll record it. We can shift later but the choice cascades into every other decision, so don't sit on the fence.

If user resists picking, push back. The whole framework's value is forcing this decision. Record their answer with rationale.

### 7. Tesler's Law — who absorbs complexity?
- For the 2-3 most complex flows, who absorbs the complexity?
  - User (power-user controls, settings, customization)
  - App (smart defaults, AI-assisted, opinionated workflow)
  - Admin (workspace-level configuration that users don't see)
  - Support (out-of-band assistance)

### 8. Polish floor (Aesthetic-Usability seed)
- What does "below standard" look like for you? Name a SaaS that you consider visually polished and one you consider mediocre. We'll come back to this in phase 5 (density) for concrete spacing/typography decisions.

## What to write

### CONSTITUTION.md — Product section
```markdown
## Product

**Pitch:** <one-sentence>
**Primary user:** <role, context>
**Job-to-be-done:** <core job>

**Usage pattern:** <primary daily app | visited briefly>
**Polish budget implication:** <derived from above>

**Wedge:** <unique value>
**80/20 — flows getting 80% of polish:**
1. <flow>
2. <flow>
3. <flow>

**Mental model:**
- Metaphor: <metaphor>
- Inherits from: <products>
- Breaks: <models broken> — Reason: <why>

**Cognitive-load posture:** <Linear-style | ClickUp-style>
  Reason: <user's rationale>

**Tesler's Law — complexity absorption:**
- <flow>: absorbed by <user|app|admin|support>
- <flow>: absorbed by <user|app|admin|support>

**Polish floor reference:**
- Polished: <name>
- Mediocre: <name> — what makes it mediocre: <reason>
```

### PRINCIPLES.md — seed sections (will be expanded across phases)

Initialize PRINCIPLES.md with the structure from `templates/PRINCIPLES.md` and fill these laws now:

- **Mental Model** (Law 8) — written from interview Q5
- **Cognitive Load** (Law 7) — written from interview Q6
- **Jakob's Law** (Law 2) — written from interview Q4 (inheritances + intentional breaks)
- **Tesler's Law** (Law 3) — written from interview Q7
- **Pareto Principle** (Law 4) — written from interview Q3 (the 20% flows)
- **Aesthetic-Usability** (Law 6) — seed from interview Q8; finalized in phase 5

Other project-level laws (Doherty, Postel) finalize in later phases.

## Success criteria

Phase 1 is complete when:
1. CONSTITUTION.md Product section is fully written and confirmed by user.
2. PRINCIPLES.md has 6 of 8 sections populated (Doherty + Postel filled in later phases).
3. The cognitive-load posture (Linear vs ClickUp) is decided and recorded with rationale.
4. STATE.md updated: `phase_1_complete: true`, `current_phase: 2`.

## What NOT to do

- Don't accept "we want both Linear and ClickUp" as an answer to Q6. They're opposites. Force a choice.
- Don't write CONSTITUTION rules without `Reason:` lines.
- Don't invent the wedge if the user is unclear — ask probing questions. "What would a user lose if they couldn't use you?"
- Don't proceed to phase 2 if the cognitive-load posture is unresolved.

## Checkpoint

When phase 1 completes, ask: "Product foundation is set. Continue to phase 2 (modules), or pause here?"

# Phase 6 — Copy

**Purpose:** Establish the voice. Copy is the highest-frequency way the product talks to the user — error messages, empty states, confirmations, button labels, helper text. Generic copy ("Oops! Something went wrong") signals generic product. Specific, direct copy signals craft.

**Produces:** `.saas-ui/CONSTITUTION.md` (Copy section)

**Estimated time:** 10–15 min

## Interview prompts

### 1. Voice tone
> Three options:
> - **Direct, terse, professional** — Linear, Stripe. Says what happened, no decoration. Examples: "Task created", "Card declined", "Sync paused".
> - **Direct, warm, conversational** — Notion, Slack. Direct but with light personality. Examples: "Got it — task created", "Looks like that card was declined", "Sync is taking a break".
> - **Playful, branded** — MailChimp, Duolingo. Heavy personality. Examples: "Bingo, your task is in!", "Whoops, that card didn't go through". *Almost never appropriate for enterprise SaaS.*
>
> Most enterprise SaaS picks #1. Pick #2 only if your brand personality demands it. Don't pick #3.

### 2. Tense and grammar rules
> Confirm or override:
> - **Confirmations:** past tense. "Task created" not "Task has been created" not "Created task!"
> - **Empty states:** present tense, action-oriented. "No issues yet — create your first." not "There are no issues."
> - **Errors:** name the failure + recovery. "Card declined. Try a different card or contact your bank." not "Payment failed."
> - **Buttons:** verb-first imperative. "Create task" not "Task creation" not "Submit".
> - **Labels:** noun, sentence case. "Due date" not "Date Due" not "DUE DATE".
>
> Any overrides?

### 3. Forbidden phrasings
> Confirm these are off-limits in this product:
> - Exclamation marks (except in playful brand — which you've ruled out)
> - "Oops!", "Whoops", "Yikes", any apology-as-feedback
> - "Great job!", "Awesome!", "Nice work!" — gamification phrasings
> - "Are you sure you want to...?" — replace with undo
> - "Something went wrong" — never. Always name the specific failure
> - Filler: "Please", "Kindly", "Just"
>
> Any of these you want to allow with rationale?

### 4. Error message structure
> What's the canonical error message structure?
>
> Suggested: **[What failed] · [Why, if relevant] · [Recovery action].**
>
> Examples:
> - "Card declined. Try a different card or contact your bank."
> - "Couldn't save changes — connection lost. We'll retry automatically."
> - "Webhook delivery failed: receiver returned 500. Retry now, or check the receiver's logs."
>
> Confirm structure or define your own.

### 5. Empty state structure
> What's the canonical empty state structure?
>
> Suggested: **[What's absent] · [Why it's absent / context] · [Single recommended action].**
>
> Examples:
> - "No tasks in your inbox. New tasks appear here as teammates assign them to you. Create a task."
> - "No usage yet. Once you start running queries, you'll see usage trends here. Open the query editor."
>
> Confirm or define.

### 6. Confirmation copy
> What's the canonical confirmation structure?
>
> Suggested: **[Action verb in past tense] · [Subject, terse] · [Undo affordance if applicable].**
>
> Examples:
> - "Task created" (with optional [Undo])
> - "Invite sent to alice@example.com" (with optional [Undo])
> - "5 issues archived" (with [Undo])
>
> Confirm or define.

### 7. Labels and helper text
> What's the rule for when helper text appears?
>
> Suggested: **Only when there is non-obvious behavior or a non-obvious constraint.**
> - "Slug" — no helper needed if user knows what a slug is
> - "Slug" — helper: "Used in URLs. Lowercase letters, numbers, hyphens." if non-obvious
> - "Email" — no helper needed
> - "Webhook URL" — helper: "We POST to this URL. Must accept JSON."
>
> Generic helper text ("Enter your email") is forbidden — it's noise.

### 8. Product-specific vocabulary
> Are there terms in your product domain that need standard wording?
> - What do you call the unit of work? (Task, Issue, Ticket, Item, Card)
> - What do you call the workspace? (Workspace, Team, Org, Project)
> - What do you call assignment? (Assigned, Owner, Responsible)
>
> Establish glossary terms so they're consistent across the app.

## What to write

### CONSTITUTION.md — Copy section

```markdown
## Copy

**Voice:** direct/terse/professional | direct/warm/conversational | playful/branded
  Reason: <user's rationale>

**Tense and grammar rules:**
- Confirmations: past tense ("Task created")
- Empty states: present tense, action-oriented
- Errors: [what failed] · [why] · [recovery]
- Buttons: verb-first imperative
- Labels: sentence case, noun
- Overrides: <list any>

**Forbidden phrasings:**
- Exclamation marks (except: <exceptions>)
- "Oops!", apology-as-feedback
- "Great job!", "Awesome!", gamification
- "Are you sure?" → undo instead
- "Something went wrong" → always name the failure
- Filler: "Please", "Kindly", "Just"
- <additional forbidden phrasings>

**Error message structure:**
[What failed] · [Why, if relevant] · [Recovery action].
Example: <product-specific example>

**Empty state structure:**
[What's absent] · [Why / context] · [Single recommended action].
Example: <product-specific example>

**Confirmation structure:**
[Action past tense] · [Subject] · [Undo if applicable].
Example: <product-specific example>

**Helper text rule:**
Only when behavior or constraint is non-obvious. Generic helper text forbidden.

**Glossary (product-specific terms):**
- <Term>: <definition + usage>
- <Term>: <definition + usage>
```

## Success criteria

Phase 6 is complete when:
1. CONSTITUTION.md Copy section is fully populated.
2. Forbidden phrasings list is explicit.
3. Error / empty / confirmation structures have product-specific examples.
4. Glossary captures product-specific vocabulary.
5. STATE.md updated: `phase_6_complete: true`, `current_phase: 7`.

## What NOT to do

- Don't accept generic voice ("friendly but professional"). Force one of three options with rationale.
- Don't let forbidden phrasings be added back as exceptions without explicit rationale per phrasing.
- Don't write error/empty/confirmation structures without a product-specific example proving the rule works.
- Don't skip the glossary. Vocabulary inconsistency is a silent killer of polish.

## Checkpoint

When phase 6 completes, ask: "Voice is set. Continue to phase 7 (state model), or pause here?"

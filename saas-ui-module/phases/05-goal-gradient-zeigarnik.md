# Phase 5 — Goal-Gradient & Zeigarnik

**Purpose:** Apply two laws that govern multi-step flows: Goal-Gradient (users accelerate near completion; show progress to harness this) and Zeigarnik (incomplete tasks pull attention; design for resumability so the pull is productive, not stressful).

**Produces:** `LAWS.md` → Goal-Gradient & Zeigarnik sections.

**Estimated time:** 10–15 minutes.

---

## The laws

**Goal-Gradient Effect:** As people approach a goal, they accelerate toward it. The closer the perceived end, the faster they push. Showing progress harnesses this — hiding progress wastes it.

**Zeigarnik Effect:** Incomplete tasks occupy mental working memory more than completed ones. They pull attention. This is a tax on the user — but if the system supports return-and-resume, it converts into a productive return.

These two laws apply to **multi-step flows** specifically. Single-action flows skip this phase (or note N/A).

---

## Goal-Gradient — the four checks

### Check 1 — Identify the multi-step flows

From phase 2: which flows are pattern `linear` or `save-resume` or `branching` with ≥3 steps?

(Single-action and continuous flows skip.)

### Check 2 — Progress indicator design

For each multi-step flow, decide the progress indicator:

| Step count | Indicator |
|---|---|
| 2–3 | Subtle (active step highlighted; no numbered stepper needed) |
| 4–6 | Numbered stepper showing current / total |
| 7+ | Numbered stepper + percent complete + "N of M" |

For each:
```markdown
#### <Flow name>

- **Step count:** <N>
- **Indicator type:** <subtle | stepper | stepper+percent>
- **Where indicator lives:** <top of flow surface / drawer header / sticky bar>
- **Persistence:** indicator visible at every step (no hiding it during transitions)
```

### Check 3 — Accelerator design

As the user approaches the final step, the design should *match* their acceleration:

- **Final-step affordance is the primary action.** Not "Next"; the actual outcome verb ("Submit invoice", "Send", "Confirm").
- **Final step has minimum input cost.** If the user has been entering data for 4 steps, the final step should be one button — not a form they didn't expect.
- **Acceleration cue:** the visual signal that this is the last step. Stepper highlights "Final"; copy says "Last step"; the primary action verb is the outcome.

Anti-pattern: a 5-step wizard where step 5 introduces 8 new required fields. The user expected acceleration; the system pulled the rug.

### Check 4 — Completion celebration restraint

When the user completes:

- **Confirm completion concretely** (per Peak-End phase 4 — the success exit is part of the peak).
- **Do NOT add a celebration screen** unless the flow is genuinely rare (onboarding completion, first-time setup). Frequent celebrations become noise.
- **The "you're done" feeling comes from the concrete result, not from animation or copy.**

---

## Zeigarnik — the three checks

### Check 1 — Identify save-and-resume flows

From phase 2: which flows are pattern `save-resume`? Also include `linear` flows with >3 steps where the user might leave mid-flow.

### Check 2 — Persistence contract

For each, expand the save-and-resume contract from phase 3:

```markdown
#### <Flow name> — Persistence

- **What's saved:** <list of fields/state>
- **When it's saved:** <every keystroke | every step | explicit save>
- **Where it's stored:** <server (Server Action) | localStorage | URL params>
- **Reason for storage choice:**
  - Server: cross-device resume, but every keystroke costs a write. Reserve for explicit "Save draft."
  - localStorage: instant, but device-bound. Good for in-flow state that doesn't need cross-device.
  - URL params: best for resumable from any link. Good for filters, partial selections.
```

### Check 3 — Resume entry point

For each, design the resume:

```markdown
- **Resume trigger:** <how the user returns — notification / dashboard "Continue where you left off" / bookmark / URL>
- **Resume UI:** <what they see when they return>
  - For form-based flows: "Resume editing <subject>" with the form pre-filled and focused on the next blank field
  - For multi-step flows: "Step <N> of <M>: <step name>" — drop user at the step they were on, not at step 1
  - For data-import flows: "Resume import of <filename>" with progress shown
- **Stale-resume handling:** if the user returns 30 days later and the world has changed (entity referenced was deleted, validation rules changed), surface the issue inline, not as a failure. Let user repair-and-continue rather than restart.
```

### Check 4 — Active-incomplete-task signaling

If the user has incomplete tasks pending in this module, surface them when the user returns to the module (not just when they return to the specific flow):

- **Pattern:** at module home, a "Pinned" or "Continue" section shows incomplete-flow items.
- **Cap:** ≤ 3 incomplete-task pins at a time per Miller's Law. More than 3 becomes noise.
- **Dismiss:** user can dismiss individual pins; dismissed pins move to history (not deleted).

This pattern earns the Zeigarnik pull: the user's brain wants to return; the system shows them where.

---

## What NOT to do in phase 5

- **Don't show progress on single-action flows.** That's overhead with no payoff.
- **Don't use percentage progress for unbounded operations.** Vague indeterminate loading is honest; fake percent bars are insulting (Forbidden Pattern #4).
- **Don't auto-save without telling the user.** The sync indicator from CONSTITUTION → State Model is mandatory for autosave.
- **Don't make resume too clever.** "Resuming exactly where you left off mid-keystroke" is creepy. Resume to the next step boundary; let the user re-orient.
- **Don't expire saved drafts silently.** If the system will drop a draft after 30 days, warn the user before deletion.

---

## What to write

`LAWS.md` additional sections:

```markdown
## Goal-Gradient Effect

### Multi-step flows

#### <Flow 1>
<per Check 2>

#### <Flow 2>
<per Check 2>

### Accelerator design
<per Check 3 — cross-flow patterns>

### Completion restraint
<per Check 4 — module-level posture on celebrations>

---

## Zeigarnik Effect

### Save-and-resume contracts

#### <Flow 1>
<per Check 2>

#### <Flow 2>
<per Check 2>

### Resume entry points

#### <Flow 1>
<per Check 3>

### Module-home incomplete-task signaling

<per Check 4 — pinned-tasks pattern if applicable>
```

---

## Checkpoint

> "Three checks:
> 1. Are progress indicators consistent across multi-step flows in this module? Different patterns (stepper vs percent vs subtle) for similar flows confuses the user — pick one default per module.
> 2. For save-and-resume flows — walk through 'user closes browser mid-flow, returns 4 days later.' Does the system handle it without losing data, asking 'are you sure?', or restarting?
> 3. Module-home incomplete-task pinning — is the cap of 3 right for this module? Some modules legitimately have only one resumable flow type (Inbox doesn't need it); others have many (Compliance might)."

Iterate. Lock. Move to phase 6.

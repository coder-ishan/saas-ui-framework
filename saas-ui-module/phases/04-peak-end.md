# Phase 4 — Peak-End Rule

**Purpose:** Identify the peak moment and the end moment of each primary flow, then commit to polishing those two moments above all others. Users remember the peak and the end; the middle averages out.

**Produces:** `LAWS.md` → Peak-End section.

**Estimated time:** 15 minutes for anchor modules.

---

## The law

**Peak-End Rule (Kahneman):** People judge an experience largely by how they felt at its most intense moment (the peak) and at its end, rather than the average of every moment in the experience.

**Implication for SaaS modules:** Don't try to make every screen feel equally satisfying. That's expensive and the budget gets diluted. Instead, identify the peak and the end of each flow, and concentrate polish there.

---

## The two-step identification

### Step 1 — Identify the peak

For each P0 flow from phase 3, ask:

> "What's the single most satisfying moment in this flow? When does the user feel 'yes, this worked'?"

Examples:
- **Invoice creation flow** — peak: the moment the invoice number renders on the success screen. The number is concrete proof the invoice exists.
- **Customer onboarding flow** — peak: the welcome message + first dashboard render. The user sees their data, configured for them.
- **Inbox triage flow (continuous)** — peak: the inbox-zero moment. The list empties; a quiet "All caught up" state appears.
- **Bulk import flow** — peak: the "Imported 1,247 records" toast — concrete count, concrete success.
- **Refund flow** — peak: the "Refund processed" confirmation. The user was nervous about doing this; the system confirmed it cleanly.

If you can't identify a peak, the flow has no emotional shape — it's a checklist of clicks. That's often acceptable for rare-tier flows, but for anchor flows it's a flag that the flow needs more design.

### Step 2 — Identify the end

The end is *not* always the success exit. The end is the user's last interaction before leaving the flow's mental scope.

- **Linear/branching flows:** the end is usually the success exit or abandon exit.
- **Continuous flows:** the end is the "I'm done here, moving on" moment — often the last action before leaving the module.
- **Save-and-resume flows:** there are TWO ends — the explicit save (pause point) and the eventual completion. Design both.

For each P0 flow, ask:

> "What's the last moment the user has in this flow's mental scope? What do they see, feel, or do as they leave?"

---

## Designing the peak

For each peak, write:

```markdown
### <Flow name> — Peak

- **Moment:** <one sentence describing the peak>
- **What the user sees:** <the visual + textual artifact>
- **What the user feels (intended):** <competence | relief | accomplishment | reassurance>
- **Polish investment here:**
  - Motion: <e.g., subtle scale-in on the success number, per CONSTITUTION canonical>
  - Copy: <e.g., includes the concrete value, not just "Success">
  - Composite affordances: <e.g., share/copy/print buttons present here for the artifact>
- **Where this peak fails (LLM common):** <e.g., "Generic 'Success' toast that doesn't include the invoice number — peak collapses into noise">
```

---

## Designing the end

For each end, write:

```markdown
### <Flow name> — End

- **Moment:** <one sentence>
- **Type:** success exit | abandon exit | pause point | natural rest
- **What the user sees:** <visual + textual>
- **What the user feels (intended):** <closure | confidence | quiet acknowledgment>
- **Continuation:** <stay in module / navigate / explicit "What's next?" prompt>
- **Polish investment here:**
  - Copy: <CONSTITUTION → Copy → Confirmation structure>
  - Motion: <how the flow visually closes>
- **Where this end fails (LLM common):** <e.g., "Dumping user back at module home with no acknowledgment that the flow completed — end disappears, no closure">
```

---

## Cross-flow Peak-End strategy

After identifying each flow's peak and end, look at the module as a whole:

```markdown
### Module-level Peak-End

The user's overall impression of this module is shaped by:
- **The dominant peak** (which flow's peak does the user encounter most often?): <flow + peak>
- **The dominant end** (where do most sessions in this module end?): <flow + end>

These two moments carry disproportionate weight in user perception of the module's quality. They are the polish budget's highest-leverage targets.
```

---

## What NOT to do in phase 4

- **Don't try to make every step a peak.** Diluted polish is worse than concentrated polish.
- **Don't make the peak a celebration.** Confetti, fanfare, "Awesome!" copy — all forbidden by CONSTITUTION → Copy → Forbidden phrasings. The peak is satisfying because it's *real* — concrete value, clean acknowledgment.
- **Don't conflate peak with success exit.** Sometimes the peak is mid-flow (e.g., the moment a complex query returns the right result). The end is later — when the user closes the report.
- **Don't skip end design for continuous flows.** "There's no end, it's continuous" is a cop-out. The end is the moment the user mentally checks out of the module.
- **Don't claim Peak-End for rare modules without arguing.** A rare quarterly compliance module may genuinely not have a polish-worthy peak — but say that explicitly, with a reason.

---

## What to write

`LAWS.md` → Peak-End section:

```markdown
## Peak-End Rule

### Per-flow Peak-End

#### <Flow 1> — Peak
<from template>

#### <Flow 1> — End
<from template>

#### <Flow 2> — Peak
<from template>

...

### Module-level Peak-End

<from template>

### Polish budget allocation

| Moment | Investment | Why |
|---|---|---|
| <peak> | high | most satisfying moment of dominant flow |
| <end> | high | last impression of dominant flow |
| <middle steps> | floor (CONSTITUTION → Density → Polish floor) | per Pareto, not where the impression forms |
```

---

## Checkpoint

> "Three checks:
> 1. For each peak — if you removed the peak's polish and made it generic, would the flow still feel satisfying? If yes, you haven't identified the peak. The peak is where polish *changes* the felt quality.
> 2. For each end — does the user have a clear sense of closure? An anxious 'Did that work?' end-state is a sign the end needs explicit acknowledgment.
> 3. Is the module-level dominant peak/end consistent with the Primary user goal from OVERVIEW.md? They should align — the dominant peak is the satisfying moment of the most-done flow."

Iterate. Lock. Move to phase 5.

# Phase 1 — Module deep purpose

**Purpose:** Deepen the one-line purpose from MODULES.md into the working definition that governs every flow decision. This phase forces clarity about *what users come here to do* — not what features the module has.

**Produces:** `OVERVIEW.md`.

**Estimated time:** 10 minutes.

---

## The four questions

### Question 1 — Job-to-be-done

The MODULES.md entry has a one-line purpose. Now drill in:

> "When a user opens this module, what are they *doing*? Not 'using features' — what's the job they're hiring this module to do?"

For an Inbox module: "Triaging incoming work — deciding what's urgent, what's noise, and what to act on first."

For a Billing module: "Confirming they're being charged correctly and managing their plan."

For an Audit Log module: "Investigating what happened during a specific period for compliance or debugging."

If the answer is feature-shaped ("manages invoices and payment methods and subscriptions"), push back: "What's the user doing when they reach for it? What problem are they about to solve?"

### Question 2 — Primary user goal in this module

> "Pick the ONE primary goal the user is trying to accomplish here. The whole module is in service of this goal."

Examples:
- Inbox → "Get to zero — every incoming item triaged."
- Customers → "Find a customer and act on their record."
- Reports → "Generate the report I need with confidence the data is correct."

This becomes the **anchor for the Peak-End decision in phase 4**.

### Question 3 — When users typically visit and for how long

> "When does the user visit this module, how often, and how long do they stay? This sets the polish budget and the flow shape."

- **Anchor + daily, hours-long:** flows must be high-precision, keyboard-friendly, recoverable from interruption.
- **Anchor + daily, minutes-long:** flows must be fast — Doherty matters intensely.
- **Occasional + weekly:** balance — discoverability and speed both matter.
- **Rare + monthly:** discoverability over speed; explain context inline (the user has forgotten how it works).

### Question 4 — Entry conditions

> "How do users arrive here? List every entry point, in priority order."

- URL (direct nav, bookmark)
- In-app navigation (sidebar, breadcrumb, command palette)
- Notification (email link, in-app banner, push)
- External (auth callback, share link, integration webhook)
- After-action (just completed something elsewhere; was redirected here)
- Search (top search, command palette)

This list feeds phase 2 (flow enumeration).

---

## What to write

`OVERVIEW.md`:

```markdown
# <Module name> — Overview

**Tier:** anchor | occasional | rare (from MODULES.md)
**Polish budget:** high | medium | minimal
**Last updated:** <YYYY-MM-DD>

## Job-to-be-done

<from Question 1>

## Primary user goal

<from Question 2>

This is the anchor for Peak-End decisions. The module is successful when this goal is achieved with a satisfying end and a memorable peak.

## Visit pattern

- **Frequency:** <daily | weekly | monthly | event-driven>
- **Duration:** <hours | minutes | seconds>
- **Implications for flow shape:** <one or two implications — speed vs precision, discoverability vs muscle memory>

## Entry points

1. <entry point — priority highest>
2. <entry point>
3. <entry point>
...

## Composites used

| Composite | Role here | Spec status | Notes |
|---|---|---|---|
| <composite> | <e.g., "main canvas"> | spec'd / unspec'd / blocked-on-atom | |
| <composite> | <e.g., "row detail drawer"> | | |

## Composite readiness

If any composite is unspec'd:
> Module flows assume <composite> behaves per its COMPOSITES.md entry. Re-validate flows after the composite SPEC lands.

## Module-specific glossary

If the module introduces domain terms (Engagement, SKU, Tenant, Cohort):

- **<Term>:** <definition + how it's used in this module's flows>
```

---

## Success criteria for phase 1

- Job-to-be-done is action-shaped (verb), not feature-shaped (noun list)
- Primary user goal is ONE sentence — the Peak-End anchor
- Visit pattern is concrete (daily/weekly/etc.) with at least one flow-shape implication
- Entry points are listed in priority order
- Composites used table is populated with readiness per composite
- Glossary covers any domain terms that will appear in flows

---

## What NOT to do in phase 1

- **Don't restate MODULES.md.** The MODULES.md entry is the seed. This phase deepens it.
- **Don't list features as the job.** "Pricing management, plan switching, billing history" is a feature list, not a job. The job is "confirm I'm being charged correctly and manage my plan."
- **Don't pretend a rare module is an anchor.** If MODULES.md says rare, design for rare — the worst rare modules are over-polished and under-supported.

---

## Checkpoint

> "Three checks:
> 1. Is the Job-to-be-done sentence verifiable in user research? If you handed it to a user, would they say 'yes that's what I do here'?
> 2. Is the Primary user goal singular? If there are multiple goals, is one really primary, or is this actually two modules glued together?
> 3. Do the Entry points match the visit pattern? (A 'rare monthly' module with 8 entry points is suspicious — most rare modules have 1–2.)"

Iterate. Lock. Move to phase 2.

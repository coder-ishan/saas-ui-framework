# Phase 2 — Modules

**Purpose:** Discover the modules of this SaaS from user behavior, not from default templates. Modules are the middle layer between Project and Composite — coherent areas of the product where flows live (Inbox, Settings, Billing, Reports, etc).

**Produces:** `.saas-ui/MODULES.md`

**Estimated time:** 15–25 min

## Why fully-discovered (no defaults)

Default module lists ("every SaaS has auth, billing, settings, admin...") trap the user into a shape that may not fit. The framework's value is forcing the user to articulate what *their* product actually contains — which may include modules a generic template would miss and may exclude modules a generic template would assume.

## Interview protocol

### Primary question: First-week walkthrough

Ask:
> Walk me through a typical user's first week with this product. Day-by-day:
> - Day 1: What do they do? What screens do they visit?
> - Day 2-3: What do they come back for?
> - Day 4-7: What patterns emerge?
>
> Be concrete. Name specific screens. Don't summarize.

Listen for *screens, areas, flows*. From this, derive candidate modules. A module is a coherent area where the user has a *distinct goal* and a *distinct mental space*.

Examples of how to identify modules from user answers:
- "They check what's new" → likely an Inbox / Feed / Updates module
- "They configure their team" → Settings or Workspace module
- "They look at how things are going" → Reports / Dashboard / Analytics module
- "They process incoming work" → Triage / Queue / Inbox module
- "They build something" → Editor / Canvas / Composer module

### Secondary question: Returning user

> Now imagine the same user 3 months in. They open the app. Where do they go *first*? Where do they spend the most time? Where do they almost never go but need to be there?

This separates **anchor modules** (used daily) from **occasional modules** (used weekly/monthly) from **rare modules** (admin, billing, settings). Polish budget differs per tier.

### Tertiary question: Missing modules

After deriving the candidate list, probe:
> Looking at this list of modules, what's missing? Specifically:
> - Is there an admin or workspace-config area for owners that regular users never see?
> - How does billing work — is there a self-serve area or is it sales-led?
> - Is there an integrations / connections area?
> - Is there a notifications center?
> - Is there an onboarding/first-run flow that is itself a module?
>
> Add anything we missed. Remove anything that doesn't actually apply.

### Quaternary question: Mental space boundaries

For each candidate module, confirm:
> What's the *one thing* that defines this module? If I described this module in one sentence to a new engineer, what would it be?

If the user can't summarize a module crisply, it's either two modules merged or not a module at all (it's a flow within another module). Refine.

## What to write

### MODULES.md

Use `templates/MODULES.md`. For each module:

```markdown
## <Module name>

**Tier:** anchor | occasional | rare
**One-line purpose:** <crisp summary>
**Primary user goal here:** <what they're trying to accomplish>
**Polish budget tier:** <high | medium | minimal — derived from tier + wedge alignment>

**Key flows (rough — will be detailed in saas-ui-module):**
- <flow 1>
- <flow 2>
- <flow 3>

**Likely composites used (will be confirmed in phase 3):**
- <e.g., DataTable, FilterBuilder, DetailDrawer>

**Notes / open questions:**
- <anything the user flagged as uncertain>
```

At the bottom of MODULES.md, write a **dependency note**: which modules depend on which (e.g., "Settings → Workspace must exist before per-module settings work").

## Success criteria

Phase 2 is complete when:
1. MODULES.md lists every module with: tier, purpose, user goal, polish budget tier.
2. Anchor modules vs occasional vs rare is decided per module.
3. User has confirmed no major modules are missing.
4. Each module has a crisp one-line purpose (no waffly summaries).
5. STATE.md updated: `phase_2_complete: true`, `current_phase: 3`.

## What NOT to do

- Don't auto-add modules from a template. If the user didn't mention billing, ask about it explicitly — don't add it silently.
- Don't accept fuzzy module definitions. "Stuff related to projects" is not a module — push for the crisp one-liner.
- Don't merge modules that the user treats as separate. Even if they look similar to you, the user's mental model is what matters.
- Don't assign polish budget without rationale. Polish budget is derived from tier + wedge alignment, not gut feel.

## Checkpoint

When phase 2 completes, ask: "Module catalog is set. Continue to phase 3 (composites), or pause here?"

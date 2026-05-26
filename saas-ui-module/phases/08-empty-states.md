# Phase 8 — Empty / error / loading states (module-specific copy)

**Purpose:** Compose module-specific copy for the canonical states. The composite SPECs define the STATE behaviors; this phase fills in the module's domain language.

**Produces:** `EMPTY-STATES.md`.

**Estimated time:** 15 minutes.

---

## The principle

CONSTITUTION defines the *structure* of empty / error / loading copy. The composite STATES.md define the *behavior*. This phase fills in the *text* — and that text must use the module's domain language, not generic placeholders.

Generic "No data" is forbidden (Forbidden Pattern #5). Specific "You haven't sent any invoices yet" is required.

Reference: `references/empty-state-patterns.md`.

---

## The four canonical contexts

For each flow's primary screens (from phase 3), produce copy for these contexts:

### Context 1 — True-empty (first time)

The user has never had data here.

```markdown
#### <Flow / Screen name> — True-empty

- **Surface:** <where it appears — full screen, content area, etc.>
- **Composite state:** empty (true-empty) — see <composite>/STATES.md
- **Copy:**
  - **Header (3–6 words):** <e.g., "No invoices yet">
  - **Body (1–2 sentences):** [What's absent] · [Why / context] · [Single recommended action]
    Example: "You haven't sent any invoices yet. Create your first invoice to get paid."
  - **Primary action label:** <verb-first imperative — "Create invoice">
  - **Secondary action (optional):** <e.g., "Import from CSV" — only if a genuine alternative path>
- **Illustration:** present | absent | <reason for choice — illustrations OK for true-empty in anchor modules, omit for rare>
```

### Context 2 — Filter-empty

Data exists; the user's filter excludes everything.

```markdown
#### <Flow / Screen name> — Filter-empty

- **Surface:** Body area only (toolbar / filter chips stay visible & interactive)
- **Composite state:** empty (filter-empty)
- **Copy:**
  - **Header:** "No <items> match these filters."
  - **Body:** "Adjust filters or clear them to see all <total count> <items>."
  - **Primary action:** "Clear filters"
- **Illustration:** absent — this isn't a true-empty.
```

### Context 3 — Error (recoverable)

An operation in this module failed; user can retry.

```markdown
#### <Flow / Step name> — Error (recoverable)

- **Surface:** inline below field | banner above section | toast (action source out of viewport)
- **Composite state:** error (recoverable)
- **Copy structure:** [What failed] · [Why, if relevant] · [Recovery action]
- **Examples for this module:**
  - Save failure: "Couldn't save changes: your session expired. Sign in and try again."
  - Validation failure: "Email format isn't recognized: must include @ and a domain. Example: name@company.com"
  - Network failure: "Couldn't reach the server. Check your connection — your changes are kept here until you retry."
- **Recovery action label:** verb-first ("Retry", "Sign in", "Reconnect", "Edit email")
```

### Context 4 — Error (terminal)

Permission denied, resource deleted, 404 within this module.

```markdown
#### <Flow / Step name> — Error (terminal)

- **Surface:** Full screen or section banner
- **Composite state:** error (terminal)
- **Copy structure:** Name the failure explicitly. No "Something went wrong."
- **Examples for this module:**
  - 404: "This invoice doesn't exist. It may have been deleted, or the link is wrong."
  - Permission: "You don't have access to <subject>. Ask <admin role> if you need it."
  - Deleted: "This customer was deleted by <user> on <date>. <If recoverable: 'Restore' button. If not: 'Back to customers' link.>"
- **Recovery:** Navigation, not retry.
```

---

## Loading copy (when needed)

Most loading states are visual (skeletons per CONSTITUTION). But some need text:

### "Taking longer than usual" affordance

Per CONSTITUTION → State Model → Loading strategy: at 5s, surface a "Taking longer than usual" affordance. Module-specific copy:

```markdown
#### Long-load copy

- **At 5s:** "Loading <subject> — taking longer than usual. <Cancel button> | <Reload button>"
- **At 15s:** "Still loading. <If known, name the slow operation: 'Generating the report'. If not: keep silent — don't speculate.>"
```

### Long-running mutations

Some module flows have inherently slow mutations (report generation, bulk imports, data exports). Per CONSTITUTION → Motion → Latency budget, these need progress affordance:

```markdown
#### Long-mutation copy

- **In-progress copy:** "<Verb-ing> <subject>… (<progress if known>)"
  Examples: "Generating report… (page 2 of 5)" / "Importing customers… (812 of 1,247)"
- **Success copy:** [Action past tense] · [Subject] · [Undo if applicable]
  Examples: "Imported 1,247 customers." / "Generated Q3 report. (link)"
- **Failure copy:** Recovery-oriented per Context 3.
```

---

## Module-level copy registry

Sometimes a piece of copy appears in many flows. Register it once here so it's consistent:

```markdown
## Shared copy registry

| Phrase | Use case | Exact wording |
|---|---|---|
| Save success | After saving any record in this module | "Saved." |
| Discarded changes | After abandon | "Discarded." (with 5s undo) |
| Network down | Connection lost banner | "You're offline. Changes are kept here until reconnect." |
| Item count (plural) | Lists | "<N> <items>" / "1 <item>" (singular variant) |
```

This registry is the source of truth — flow specs reference these by name.

---

## What NOT to do in phase 8

- **Don't use "Oops" / "Sorry" / "Uh oh".** Forbidden by CONSTITUTION → Copy → Forbidden phrasings.
- **Don't apologize.** Name what happened and how to recover.
- **Don't write "Please".** Direct + warm is the voice; "please" is filler.
- **Don't use generic placeholder copy.** "Lorem ipsum" and "<header>" are spec leakage — write the real string.
- **Don't write copy without considering pluralization.** "1 row" vs "N rows" matters; "1 invoice" vs "N invoices" matters.
- **Don't write copy for states the composite doesn't have.** Reference composite STATES.md; only address states that exist.

---

## What to write

`EMPTY-STATES.md`:

```markdown
# <Module name> — Empty / error / loading copy

## Per-flow per-context copy

<for each P0/P1 flow's screens — one subsection per context>

## Loading copy

<long-load + long-mutation>

## Shared copy registry

<the table>

## Pluralization notes

<any irregular plurals in this module's domain language>

## Domain glossary references

<links to OVERVIEW.md → Module-specific glossary terms used in copy here>
```

---

## Checkpoint

> "Three checks:
> 1. Read every error copy aloud. Does it name the failure specifically, or does it deflect to generic phrasing? 'Couldn't save' is generic; 'Couldn't save: session expired' is specific.
> 2. Empty states — would a brand-new user reading this empty state know exactly what to do next? If they'd hesitate, the recovery action isn't clear enough.
> 3. Is the shared copy registry tight? Drift between 'Saved.' and 'Changes saved' and 'Saved your changes' across flows = inconsistent voice."

Iterate. Lock. Move to phase 9.

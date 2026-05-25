# Phase 9 — References

**Purpose:** Capture reference screenshots, URLs, and annotations that anchor the user's taste in concrete, comparable evidence. References make composite SPECs operational — "match the fidelity of `references/linear-cmdk.png`" is enforceable; "be polished" isn't.

**Produces:** `.saas-ui/references/` directory populated with reference manifest entries

**Estimated time:** open-ended (10 min minimum; can grow over time)

## Why references matter

Even with a strong constitution, prose can't capture everything a screenshot can: exact spring of a transition, the *feeling* of nested-command breadcrumbs, the precise restraint of a hover state. References fill the gap. They also serve as the test set: "does our DataTable feel like `references/linear-issues-table.png`?"

This phase is **open-ended**. Capture a starting set now; add references over time as the user builds.

## Ingestion mechanisms (both supported)

### Mechanism A — drag-drop (preferred for static screenshots)

1. User drags/copies image files (PNG, JPG, PDF) into `.saas-ui/references/`.
2. Skill detects new unannotated files (any image without a sibling `.md`).
3. Skill prompts the user for annotation per file: source, what to extract, applies-to.
4. Skill writes the annotation `.md` alongside the image.

### Mechanism B — URL/path (preferred for live demos, blog posts)

1. User provides a URL.
2. Skill records the URL + asks user for annotation.
3. If feasible (and user permits), skill caches the URL content for offline reference.
4. Skill writes a manifest entry.

Both produce the same manifest format. The user picks whichever is convenient.

## Annotation schema

Every reference has a sibling `.md` file. Same `id` (the filename stem) for both files.

```markdown
---
id: <kebab-case-stem-matching-file>
source: <Linear | ClickUp | Notion | Stripe | Vercel | Airtable | Other | URL>
type: <static-image | url | video>
captured: <YYYY-MM-DD>
---

# <Reference title>

**File / URL:** `<filename>` or `<url>`

**Extract:**
- <specific thing to extract — e.g., "180ms ease-out push for nested breadcrumb">
- <another — e.g., "Esc pops one level, not the whole palette">
- <another — e.g., "Selected row has 2px left border + 2% tint, not just tint">

**Applies to:**
- composite: <name(s) — e.g., CommandPalette, DataTable>
- module: <name(s) — e.g., Inbox>
- principle: <e.g., Doherty, Aesthetic-Usability>

**Notes:**
<free-text, optional>

**Counter-example (optional):**
<if this reference is what NOT to do, mark it. e.g., "This is what 'bloated modal' looks like — we want the opposite.">
```

## Interview prompts for initial reference set

Ask the user:

> Let's seed your reference library. We need at least:
> 1. **3-5 product references** — full screens from products whose feel you admire. Most useful: Linear, Stripe, Vercel, Notion, Airtable, Cron/Notion Calendar, Plaid Dashboard, GitHub. Pick the ones that match your aesthetic.
> 2. **1-2 references per anchor composite** — focused screenshots of the specific composite (DataTable in Linear, CommandPalette in Linear/Raycast, FilterBuilder in Airtable, DetailDrawer in Linear, FormSystem in Stripe).
> 3. **Counter-examples (optional)** — 1-2 screenshots of UI you find *bad*. These are useful negative anchors.

For each reference, walk through the annotation schema. Spend time on **Extract:** — vague extracts ("looks nice") are useless; specific extracts ("180ms ease-out push") are gold.

## Cross-link references back to artifacts

After the initial set is captured, update:

- `CONSTITUTION.md` — Motion section: link to motion-specific references
- `CONSTITUTION.md` — Density section: link to density-specific references
- `COMPOSITES.md` — per-composite "Anchor references" entry

This ensures references are not orphan files; they're consumed by the artifacts that need them.

## Ongoing usage

Phase 9 doesn't "complete" the way other phases do — the reference library grows over time. Mark `phase_9_complete: true` once the *initial set* (3-5 product references + 1-2 per anchor composite) is captured. The user can add more whenever.

## What to write

### references/ directory

- One image/file per reference
- One `.md` annotation per reference (same stem)

### references/INDEX.md (optional but recommended)

A simple index of references with one-line summaries:

```markdown
# Reference Library Index

## Product-level references
- `linear-overview-1.png` — Linear inbox view, density anchor
- `stripe-dashboard.png` — Stripe data presentation, color discipline
- `vercel-deployments.png` — Vercel list-with-status pattern

## Composite-specific references
- `linear-cmdk-nested.png` — CommandPalette nested breadcrumb motion
- `linear-issues-table.png` — DataTable density + selection visual
- `airtable-filter.png` — FilterBuilder multi-predicate UI
- `stripe-form.png` — FormSystem field rhythm

## Counter-examples
- `bloated-modal-example.png` — what we DON'T want in modals
```

## Success criteria

Phase 9 (initial pass) is complete when:
1. At least 3-5 product-level references captured + annotated.
2. At least 1-2 composite-specific references per anchor composite (from `COMPOSITES.md` P0 list).
3. Each reference has Extract bullets that are *specific* (not vague).
4. references/INDEX.md exists (or `.md` annotations are sufficient — your call).
5. References cross-linked from CONSTITUTION.md and COMPOSITES.md where appropriate.
6. STATE.md updated: `phase_9_complete: true`, `bootstrap_complete: true`.

## What NOT to do

- Don't accept vague Extracts. "Looks polished" tells future SPEC sessions nothing. Push for specifics.
- Don't capture references without annotations. An un-annotated screenshot is just clutter.
- Don't try to be exhaustive in one sitting. The library grows. Capture the high-leverage anchors now.

## Final checkpoint — bootstrap complete

When phase 9 (initial pass) completes:

1. Update STATE.md:
   - `phase_9_complete: true`
   - `bootstrap_complete: true`
   - `next_skills_available: [saas-ui-module, saas-ui-composite, saas-ui-audit]` (when those exist)

2. Print a summary to the user:
   - List of artifacts produced and their paths
   - Top 3 composites to spec next (from `COMPOSITES.md` build order)
   - Reminder that the framework's next skills (when built) will consume these artifacts

3. Suggest next action:
   > Bootstrap complete. The artifacts in `.saas-ui/` are your design contract for every downstream UI session. Next: when `saas-ui-composite` is available, start with `<first composite>` — it has the highest priority and unlocks the most modules.
   >
   > For now, you can hand these artifacts to your existing build skills (`plan-build-ship`, `gsd-plan-phase`, etc.) — they'll have the depth they were missing.

---
name: saas-ui-bootstrap
description: Use when starting a new enterprise SaaS UI project, or when an existing SaaS project lacks a written design constitution, principles document, design-system inventory, or module/composite catalogs. Triggers include user mentions of "build SaaS UI", "design constitution", "Linear-quality UI", "polish", "composites", "design system", or "where should I start with the UI". Conducts a chunked, resumable, multi-phase interview that produces canonical planning artifacts in .saas-ui/. Stack: Next.js App Router (SSR+CSR). Does NOT write component code — produces planning artifacts only.
---

# saas-ui-bootstrap

## Overview

Conducts a chunked, resumable interview that produces the canonical planning artifacts for a Next.js enterprise SaaS UI: design constitution, UX principles document (Laws of UX applied), design-system inventory, module catalog, composite catalog, and reference manifest.

**Core principle:** Externalize the user's taste, their existing design system, and universal UX laws into artifacts on disk so every downstream session inherits depth instead of rebuilding from average.

**What this skill does NOT do:**
- Write component code or scaffold Next.js apps
- Invent atoms (uses user's existing design system; flags gaps)
- Decide library choices (asks per-project — user-supplied)
- Produce final composite SPECs (that's `saas-ui-composite`, a later skill)
- Make decisions silently (records every taste call in CONSTITUTION.md with rationale)

## When to Use

- Starting a new enterprise SaaS UI from scratch
- Existing project lacks a written design constitution or principles document
- User wants "Linear-quality" or "ClickUp-detail" UI and is asking how to start
- About to scale UI work and needs shared standards before delegating
- User has design system primitives but no composites and no plan for them

**Do NOT use when:**
- User wants a marketing site, landing page, or content site (use design-consultation)
- User just wants to build one component (use saas-ui-composite when available)
- Project already has .saas-ui/ with completed bootstrap (use saas-ui-status to resume)

## On-disk artifact layout

All artifacts live under `.saas-ui/` in the user's project root:

```
.saas-ui/
  STATE.md             ← current phase, completed phases, last-updated timestamp
  CONSTITUTION.md      ← user's taste; the design constitution
  PRINCIPLES.md        ← Laws of UX applied to this product (project-level laws)
  INVENTORY.md         ← discovered design system (tokens, atoms, molecules, gaps)
  MODULES.md           ← module catalog discovered via "first week" interview
  COMPOSITES.md        ← composite catalog discovered via cross-module pattern extraction
  references/          ← annotated screenshots + URLs (drop-in or skill-cached)
    <id>.{png,jpg,pdf}
    <id>.md            ← source, extract, applies-to, notes
```

The framework's downstream skills (`saas-ui-module`, `saas-ui-composite`, `saas-ui-audit`) consume these artifacts as inputs.

## The 10 phases (chunked, resumable)

Each phase is a focused interview that produces or updates specific artifact sections. The user can do them in one sitting (~90 min total) or across sessions. STATE.md tracks position.

| # | Phase | Detail file | Produces / updates |
|---|---|---|---|
| 0 | Design system discovery | phases/00-design-system.md | INVENTORY.md |
| 1 | Product | phases/01-product.md | CONSTITUTION.md (Product section) |
| 2 | Modules | phases/02-modules.md | MODULES.md (catalog) |
| 3 | Composites | phases/03-composites.md | COMPOSITES.md (catalog) |
| 4 | Motion | phases/04-motion.md | CONSTITUTION.md (Motion section) |
| 5 | Density | phases/05-density.md | CONSTITUTION.md (Density section) |
| 6 | Copy | phases/06-copy.md | CONSTITUTION.md (Copy section) |
| 7 | State model | phases/07-state.md | CONSTITUTION.md (State section) |
| 8 | Forbidden patterns | phases/08-forbidden.md | CONSTITUTION.md (Forbidden section) |
| 9 | References | phases/09-references.md | references/ (manifest + annotations) |

**PRINCIPLES.md** (Laws of UX at project level) is written cross-cutting — populated as relevant laws emerge during phases 1, 4, 5, 6, 7. Finalized at end.

## Execution protocol

### On invocation

1. **Check for existing `.saas-ui/`.**
   - If absent: this is a fresh bootstrap. Create `.saas-ui/`, initialize STATE.md, start phase 0.
   - If present and STATE.md shows incomplete bootstrap: resume from `STATE.md.current_phase`. Confirm with user: "you're mid-bootstrap at phase N (<phase name>). Continue here, or jump to a different phase?"
   - If present and STATE.md shows completed bootstrap: do NOT redo. Tell the user the bootstrap is complete; suggest `saas-ui-status` (when available) or running a specific phase with `--rerun`.

2. **Load reference files into working context for each phase as needed:**
   - `references/laws-of-ux.md` — when writing PRINCIPLES.md or any phase that maps to laws
   - `references/common-llm-ui-mistakes.md` — when writing CONSTITUTION.md, especially Forbidden section
   - `templates/<NAME>.md` — when first creating that artifact

3. **For each phase:**
   - Read `phases/0N-<name>.md` for the interview prompts
   - Conduct the interview (use AskUserQuestion for structured choices; conversational for open-ended)
   - Write/update the corresponding artifact(s)
   - Update STATE.md: mark phase complete, set `current_phase` to next
   - Ask user: "continue to next phase, or pause here?"

### Iron rules

1. **Never write CONSTITUTION.md or any artifact without an interview answer backing each section.** No defaults. No inferred opinions. If the user hasn't answered something, leave it as `[TO DO: interview pending]` — not a guess.

2. **Constitution rules must cite the user's rationale.** Each rule includes a one-line `Reason:` from the interview. "Why" lives next to "what." This is what makes the constitution defensible and durable.

3. **Inventory is read-only against the user's design system.** The skill does not propose new atoms inline. Missing primitives go into INVENTORY.md's `Gaps` section as TODO for the design-system track — separate work.

4. **No code in artifacts.** This skill produces planning artifacts. Code goes elsewhere (in your build skills). The artifacts may reference React/Next.js/library names (the project's stack), but no code blocks beyond illustrative snippets.

5. **Resume must not lose work.** Every phase completion writes to disk before asking "continue?". A crashed session must be resumable from STATE.md without data loss.

## Quick reference: the artifacts

**CONSTITUTION.md** — your taste, written down. Sections: Product, Motion, Density, Copy, State, Forbidden, Action Affordances. Every rule has a `Reason:` line.

**PRINCIPLES.md** — Laws of UX applied at project level. 8 laws: Doherty, Jakob, Tesler, Pareto, Postel, Aesthetic-Usability, Cognitive Load, Mental Model. Each applied to this specific product with rationale.

**INVENTORY.md** — the design system you already have (tokens, atoms, molecules, partial composites) + observed gaps. Read-only constraint for all downstream SPECs.

**MODULES.md** — catalog of modules (Inbox, Settings, Billing, etc) derived from "first week" interview. Each module has a one-paragraph purpose + user goal.

**COMPOSITES.md** — catalog of composites observed across modules (DataTable, CommandPalette, etc). Cross-references which modules use each. Indicates module-specific variants per usage.

**references/** — annotated reference screenshots and URLs. Each has a `<id>.md` annotation: source, what to extract, which composite/module/principle it applies to.

## Common mistakes

| Mistake | Fix |
|---|---|
| Skipping the design-system phase ("we'll figure out atoms later") | Don't. Without INVENTORY.md, composite SPECs will invent atoms and ship broken. |
| Writing CONSTITUTION rules without `Reason:` lines | Re-interview. Rules without rationale don't survive edge cases. |
| Inferring defaults instead of asking | Every rule must come from a user answer. Defaults defeat the point of the constitution. |
| Conducting phases out of order | Order matters: design system before composites (so SPECs constrain); product before modules (so module rationale anchors). |
| Treating MODULES/COMPOSITES catalogs as exhaustive after one pass | They're discovered, not final. Phase 2 and 3 explicitly probe for what's missing. |
| Writing component code | Wrong skill. This is planning. Hand off to your build skills. |

## Hand-off

When all 10 phases complete:
- Update STATE.md: `bootstrap: complete`, set `next_skills_available: [saas-ui-module, saas-ui-composite]`
- Print summary of artifacts produced and where they live
- Suggest first composite to spec (highest cross-module usage from COMPOSITES.md)

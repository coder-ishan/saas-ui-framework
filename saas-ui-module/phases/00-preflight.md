# Phase 0 — Pre-flight & anchor load

**Purpose:** Refuse to start unless the project is ready. Modules sit between composites and the project; missing pieces upstream make module design speculative.

**Produces:** No artifact. Validation only.

**Estimated time:** 2 minutes.

---

## The four pre-flight checks

### Check 1 — Bootstrap completion

Read `.saas-ui/STATE.md`. All bootstrap phases 0–9 must be `done`.

If not:
> "Bootstrap not complete. Phase <N> (<phase name>) is not done. Module design depends on CONSTITUTION sections that haven't been written. Run `saas-ui-bootstrap` to finish."

**Stop.**

### Check 2 — Target module in catalog

The user names a module. Read `.saas-ui/MODULES.md`.

If not present:
> "`<name>` is not in MODULES.md. Modules are discovered through the 'first week' interview in bootstrap phase 2 — not invented here. Either:
> 1. Add `<name>` to MODULES.md by revisiting bootstrap phase 2
> 2. Pick a module that IS in the catalog"

**Stop.**

### Check 3 — Composite dependencies

Read the module's `Likely composites used` block in MODULES.md. For each composite:

- **Spec'd:** good — read its SPEC.md.
- **Unspec'd but in COMPOSITES.md:** acceptable — flow design can proceed with the caveat that composite-specific affordances may shift when spec'd. Track in OVERVIEW.md → Composite readiness.
- **Missing entirely:** halt.

If any composite is missing entirely:
> "The module references composites not in COMPOSITES.md:
> - `<composite name>`
>
> Either:
> 1. Add the composite to COMPOSITES.md (revisit bootstrap phase 3)
> 2. Remove it from this module's composite list in MODULES.md
>
> Modules consume composites; they don't invent them."

**Stop.**

If some composites are unspec'd but present:
> "Heads-up: `<list>` are in COMPOSITES.md but not yet spec'd. We can proceed with flow design — composite-specific affordances may shift when those SPECs land. Logged to OVERVIEW.md → Composite readiness."

Continue.

### Check 4 — Anchor load

Load into working context:

1. `.saas-ui/CONSTITUTION.md` — full
2. `.saas-ui/PRINCIPLES.md` — full
3. `.saas-ui/INVENTORY.md` — full
4. `.saas-ui/MODULES.md` — only this module's entry + neighboring modules in dependency graph
5. `.saas-ui/COMPOSITES.md` — entries for all composites used in this module
6. `.saas-ui/composites/<each>/SPEC.md` — for every spec'd composite in this module's list
7. `.saas-ui/references/` — files tagged for this module
8. `~/.claude/skills/saas-ui-module/references/module-laws.md`
9. `~/.claude/skills/saas-ui-module/references/flow-patterns.md`
10. `~/.claude/skills/saas-ui-module/references/empty-state-patterns.md`

If any required project file is missing:
> "Missing anchor: `<path>`. Cannot proceed — drift risk."

**Stop.**

---

## Polish budget tier check

From MODULES.md → Tier:

- **anchor:** full 10-phase pass.
- **occasional:** phases 0–8 in full; phases 9–10 may be abbreviated.
- **rare:** phases 0–3 in full; phases 4–10 may be terse. Be explicit when abbreviating ("Peak-End: not applicable — rare module visited monthly for compliance reports").

Announce the tier and abbreviation policy to the user before phase 1.

---

## Success criteria for phase 0

- All four checks pass
- The module's entry, neighbors, dependent composite SPECs are in working memory
- The polish budget tier is announced
- The 6 module-level laws are loaded
- The flow-pattern and empty-state-pattern catalogs are loaded

---

## Checkpoint

> "Pre-flight clean. Module: `<name>` (tier: <tier>, polish budget: <budget>). Composites used: <list with readiness notes>. Abbreviation policy for this run: <full / partial / minimal>. Ready to begin Phase 1 — Module deep purpose."

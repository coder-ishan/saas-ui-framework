# Phase 0 — Pre-flight & anchor load

**Purpose:** Refuse to start unless the project is ready. Catch four classes of error before they pollute the SPEC.

**Produces:** No artifact. Validation only. Either proceed or halt with a precise reason.

**Estimated time:** 2 minutes.

---

## The four pre-flight checks

### Check 1 — Bootstrap completion

Read `.saas-ui/STATE.md`.

Required: phases 0 through 9 marked `done`.

If any phase is missing:
> "Bootstrap not complete. Phase <N> (<phase name>) is not done. The composite SPEC depends on CONSTITUTION sections that haven't been written yet. Run `saas-ui-bootstrap` to finish."

Then **stop**. Do not proceed to the next check.

### Check 2 — Target composite in catalog

The user names a composite. Read `.saas-ui/COMPOSITES.md`.

If the named composite is not present:
> "`<name>` is not in COMPOSITES.md. Composites are discovered through cross-module pattern extraction in bootstrap phase 3 — they are not invented here. Either:
> 1. Add `<name>` to COMPOSITES.md by revisiting bootstrap phase 3
> 2. Pick a composite that IS in the catalog
> 3. If `<name>` is really a one-off and not a composite, build it directly in the module SPEC instead"

Then **stop**.

### Check 3 — Atom dependencies

Find the composite's entry in COMPOSITES.md. Read its `Atoms required` block.

For each atom marked `✗ gap`, check if `Primitive Gaps` resolution exists (one of: `Substitute: …`, `Defer: …`, `Add to design system: <ticket/PR>`).

If unresolved gaps remain:
> "The composite has unresolved primitive gaps:
> - `<atom name>`: needed for <reason>
> - `<atom name>`: needed for <reason>
>
> Per INVENTORY.md's hard rule, composite SPECs cannot invent atoms inline. Choose one of:
>   1. **Add to design system** — build the atom first, then return here
>   2. **Defer composite** — wait until the atom is added
>   3. **Substitute** — name an existing inventory atom that will stand in (must be argued)
>
> Which path? (1/2/3)"

If the user picks 3, capture the substitution + argument; it becomes part of the SPEC's `Primitive Gaps` section in phase 3.

If 1 or 2: **stop**. The composite isn't ready.

### Check 4 — Anchor load

Load these files into the working context:

1. `.saas-ui/CONSTITUTION.md` — full
2. `.saas-ui/PRINCIPLES.md` — full
3. `.saas-ui/INVENTORY.md` — full
4. `.saas-ui/COMPOSITES.md` — only the entry for this composite + Build order summary
5. `.saas-ui/MODULES.md` — only the modules listed under this composite's `Used by modules`
6. `.saas-ui/references/` — only files tagged for this composite (filename or annotation contains the composite name)
7. `~/.claude/skills/saas-ui-composite/references/state-catalog.md`
8. `~/.claude/skills/saas-ui-composite/references/composite-laws.md`
9. `~/.claude/skills/saas-ui-composite/references/nextjs-primitives.md`

If any required project file is missing or unreadable:
> "Missing anchor: `<path>`. Cannot proceed without all anchors loaded — drift risk is too high. Restore the file or run `saas-ui-bootstrap` to regenerate."

Then **stop**.

---

## Success criteria for phase 0

- All four checks pass cleanly
- The composite's catalog entry, atom list, and used-by-modules list are now in working memory
- CONSTITUTION's canonical motion patterns, Density grid, Copy voice, State Model, and Forbidden Patterns are all accessible by reference
- The 13 canonical states are loaded (will gate phase 4)
- The composite-level Laws of UX are loaded (will gate phase 9)
- The Next.js primitive cheat-sheet is loaded (will gate phase 7)

---

## What NOT to do in phase 0

- Don't half-load. If a single anchor is missing, halt — partial anchors produce inconsistent SPECs.
- Don't infer "probably done" from partial state. If STATE.md says phase 7 isn't done, the State Model section of CONSTITUTION is likely incomplete; downstream phases will drift.
- Don't substitute atoms silently. Substitution requires the user's explicit acceptance + argument, captured here.

---

## Checkpoint

Once all four checks pass, announce:

> "Pre-flight clean. Anchors loaded. Composite: `<name>` (`<type from COMPOSITES.md>`). Used by modules: <list>. Ready to begin Phase 1 — Purpose & contract."

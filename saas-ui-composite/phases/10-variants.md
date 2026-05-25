# Phase 10 — Variants (canonical composites only)

**Purpose:** Capture per-module variations as deltas from the canonical SPEC. This phase runs ONLY when COMPOSITES.md → Type is `canonical (multi-module)`. Single-module and bespoke composites skip phase 10.

**Produces:** `VARIANTS.md` for this composite.

**Estimated time:** 10 minutes per module that uses this composite.

---

## Why variants live separately

The SPEC is canonical — one source of truth. Variants are documented deltas, not forks. A module can't change the contract, the action inventory, the state model, or the keyboard map; it can only:

1. **Hide actions** from the Action Inventory (some actions don't apply in the module's context)
2. **Lock states** (some states are unreachable in this module — e.g., a read-only audit log can't reach active-edit)
3. **Adjust copy** within CONSTITUTION's voice rules (different domain language)
4. **Restrict atoms** (some columns or controls don't appear)
5. **Add module-specific affordances** (a unique button that lives in the toolbar for this module only)

If a module wants to fundamentally change the composite — add a new state, redo the keyboard map — it's a sign the composite should be a separate one, not a variant.

---

## When NOT to use this phase

- Type = `single-module` (only used in one place — no variants by definition)
- Type = `bespoke` (the variation is the point — no canonical to vary from)

In those cases, write a one-line VARIANTS.md:

```markdown
# <Composite name> — Variants

N/A. This composite is <single-module | bespoke>. No variants.
```

---

## The per-module variant template

For each module in `Used by modules`:

```markdown
## Variant: <module name>

- **Module tier:** anchor | occasional | rare (from MODULES.md)
- **Context:** <one-line — what the user is doing in this module when they hit this composite>

### Action Inventory deltas

| Action | Status here | Reason |
|---|---|---|
| <action> | hidden | <reason — e.g., "audit log is read-only"> |
| <action> | added | <reason — e.g., "audit log adds 'Annotate' action specific to compliance reviews"> |
| <action> | renamed | <reason — e.g., "'Add row' → 'New invoice' for domain language clarity"> |
| <action> | reordered | <reason — e.g., "Export is primary in audit log; Add is hidden"> |

### State deltas

| State | Reachable here? | Reason |
|---|---|---|
| active-edit | unreachable | Read-only module |
| loading-mutation | unreachable | No mutations in this module |
| partial-permission | always | Audit log is view-only by role for most users |

### Copy deltas

- Empty-state copy: <module-specific phrasing, still following CONSTITUTION Copy structure>
- Error copy: <module-specific>
- Search placeholder: <module-specific>

### Column / atom restrictions

- <e.g., "Status column hidden — not relevant in this module">
- <e.g., "Bulk-action strip hidden — single-row workflow">

### Module-specific additions

- <e.g., "Adds a 'Compliance flags' filter chip type, using INVENTORY's `Chip` atom with the `severity` variant">
- <e.g., "Header includes a 'Period selector' between the search and the filter chip group">

If additions exceed two atoms, flag for review — the module may be growing a separate composite.

### Keyboard deltas

| Shortcut | Behavior change | Reason |
|---|---|---|
| `Cmd+E` | No-op (no edit in read-only module) | — |
| `Cmd+P` | Print current view (module-specific) | Compliance officers print audit views regularly |

### Rendering deltas

If the module's data-fetch shape, cache tags, or RSC boundary differs from the canonical, note here:

- **Cache tag:** `audit-log:<entity>` (different from canonical `table:<resource>`)
- **Revalidation:** Audit log uses ISR with 60s revalidate; canonical is on-demand
- **Server Action namespace:** Audit log has no mutations
```

---

## Cross-variant consistency check

After all variants are written, run a check:

```markdown
## Cross-variant consistency

- **Same action, different name across variants:** flag for re-thinking — divergent labels for the same semantic action create cognitive load when users switch modules.
- **Same atom, different visual treatment across variants:** forbidden. Atoms are atoms.
- **Variant adds an action that two modules share:** promote to canonical Action Inventory; not a per-variant addition.
- **Three+ variants restrict the same state to unreachable:** flag the canonical state model — maybe the state shouldn't be in the canonical SPEC.
```

---

## What to write

`VARIANTS.md`:

```markdown
# <Composite name> — Variants

## Variants

<one section per using module>

## Cross-variant consistency

<the check from above>

## Variant inventory summary

| Module | Tier | # of action deltas | # of state deltas | # of additions | Risk flags |
|---|---|---|---|---|---|
| <module> | anchor | 3 | 1 | 0 | — |
| <module> | rare | 8 | 4 | 3 | Many additions — consider splitting |
```

---

## Success criteria for phase 10

- Every module from `Used by modules` has a variant entry
- Every delta has a reason
- Cross-variant consistency check catches divergent action names, divergent atom treatments, and split-candidate composites
- The summary table makes split candidates visible

---

## What NOT to do in phase 10

- **Don't allow variants to violate the contract.** A variant cannot drop a contract item from phase 1. If it needs to, the composite is splitting.
- **Don't add new states in a variant.** New states go in canonical STATES.md or they don't exist.
- **Don't allow keyboard divergence beyond no-op disables and module-specific additions.** Existing shortcuts retain their meanings across variants — Cmd+S saves view everywhere, full stop.
- **Don't fork the SPEC.** No copy-paste of SPEC content into VARIANTS.md. Only deltas.

---

## Checkpoint

> "Three checks:
> 1. For each variant, does the delta count feel proportional to the module's needs? A rare module with 12 deltas is suspicious — either over-customized or wrong composite choice.
> 2. Did the cross-variant consistency check surface any duplicates that should promote to canonical?
> 3. Any variant where additions exceed two atoms? If yes, consider whether that module should have its own composite (rename and graduate it in COMPOSITES.md)."

Iterate. Lock. Phase 11 = hand-off via STATE.md update.

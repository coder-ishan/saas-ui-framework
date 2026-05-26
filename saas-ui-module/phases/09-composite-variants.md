# Phase 9 — Composite variant cross-references

**Purpose:** Build a per-module index of composite variants used in this module. The variants themselves live in each composite's `VARIANTS.md`; this phase produces a *registry* — the module's view of which composites it uses and how they vary here.

**Produces:** `COMPOSITES.md` (module-scoped index).

**Estimated time:** 10 minutes.

---

## The principle

Variants live in the composite's `VARIANTS.md`, never duplicated here. This module's `COMPOSITES.md` is an *index* — it points at the canonical source, lists this module's deltas in one place, and surfaces consistency issues across the module's composite uses.

---

## The three-step index

### Step 1 — Enumerate composites used

From OVERVIEW.md → Composites used + every composite referenced in FLOWS.md step tables.

```markdown
## Composites used in this module

| Composite | Type | Used in flows | Spec status | Variant defined? |
|---|---|---|---|---|
| DataTable | canonical | <flow1>, <flow2> | spec'd | yes — see [VARIANTS.md](../../composites/data-table/VARIANTS.md#variant-<module>) |
| CommandPalette | canonical | <flow1> | spec'd | no — uses canonical, no module-specific delta |
| AuditDrawer | bespoke | <flow3> | unspec'd | N/A — bespoke; variant concept doesn't apply |
```

### Step 2 — Generate or update variant entries in each composite's VARIANTS.md

For each canonical composite where this module needs a delta:

1. Open the composite's `VARIANTS.md` (created in composite phase 10).
2. Look for an existing `## Variant: <this module>` section.
3. If absent: write one per the composite phase 10 template (Action Inventory deltas, State deltas, Copy deltas, Column/atom restrictions, Module-specific additions, Keyboard deltas, Rendering deltas).
4. If present: review and update.

This is the only output that lives outside this module's directory. Note in a hand-off log here:

```markdown
## Variant updates made

Cross-references updated in:
- `composites/data-table/VARIANTS.md` — added/updated section for module `<this module>`
- `composites/command-palette/VARIANTS.md` — N/A (no delta)
```

### Step 3 — Surface consistency issues

The module-level view catches issues that single-composite SPECs can't:

```markdown
## Consistency check

### Same atom, different appearance across composites in this module

If `Badge` appears in two composites this module uses (one as status badge, one as count badge):
- Resolve to ONE visual variant per semantic. If both are "status badges," they must look identical.
- If they're semantically distinct, name the variants explicitly.

| Atom | Composite A | Composite B | Resolution |
|---|---|---|---|

### Same action, different label across composites in this module

E.g., "Add row" in DataTable vs "New record" in DetailDrawer if they're the same semantic action.

| Action semantic | Label in composite A | Label in composite B | Resolution |
|---|---|---|---|

### Composite usage that fights flows

If a flow needs a behavior the composite SPEC doesn't support:
- This is NOT a variant. Variants are deltas (subtraction or domain language).
- It's a SPEC gap. Halt the module design until the composite SPEC is updated.

| Flow | Composite | Missing behavior | Action |
|---|---|---|---|
```

---

## What to write

`COMPOSITES.md` (module-scoped):

```markdown
# <Module name> — Composites

## Composites used

<table from Step 1>

## Variant updates made

<log from Step 2>

## Consistency check

<sections from Step 3>

## Hand-off notes

If any composite SPEC needs updating because this module surfaced a gap:
- <composite>: needs <change> — see `composites/<name>/SPEC.md` and flag for revision
```

---

## What NOT to do in phase 9

- **Don't duplicate composite VARIANTS.md content here.** Cross-reference only.
- **Don't invent new composites.** If this module truly needs one, halt and route to bootstrap phase 3 to add it to COMPOSITES.md.
- **Don't accept inconsistency.** If two composites in this module use the same atom differently, that's a CONSTITUTION/INVENTORY issue surfaced by the module level. Resolve before continuing.
- **Don't treat consistency check as optional.** This is the highest-value moment to catch atom drift — single-composite SPECs can't see it.

---

## Checkpoint

> "Three checks:
> 1. For each composite used, is its variant entry (in the composite's VARIANTS.md) up to date with the deltas this module needs? If not, update before continuing.
> 2. Run the consistency check. Any same-atom-different-appearance pairs? Any same-action-different-label pairs?
> 3. Did any flow surface a composite SPEC gap? If yes, halt the module design and update the composite SPEC first."

Iterate. Lock. Move to phase 10.

# Design System Inventory

> Discovered design system for <PRODUCT NAME>.
> This is the constraint set for all downstream composite SPECs.
> Composite SPECs must reference atoms from this inventory by exact name.
> Missing atoms go into the Gaps section — never invent inline.

**Last updated:** <YYYY-MM-DD>
**Discovery branch:** <A: existing design system | B: intent-only (none yet)>

---

## Source

<For branch A: list paths, packages, Storybook URL>
- Component directory: `<path>`
- Tokens: `<path>` (Tailwind config, tokens file, etc.)
- Storybook: `<url or "none">`

<For branch B: list intent>
- Plan: use <library / build custom>
- Reason: <user rationale>

---

## Tokens

### Colors

<List token names, not hex values. The framework cares about the vocabulary.>

- `<token-name>`: <semantic role, e.g., "primary action background">
- `<token-name>`: <…>
- ...

### Spacing scale

- Base unit: <4px | 8px>
- Scale: <e.g., 0, 1, 2, 3, 4, 6, 8, 12, 16, 24, 32, 48 × base>

### Radii

- `<token-name>`: <e.g., "sm, md, lg, full">
- ...

### Motion durations

<If defined in design system:>
- `<token-name>`: <e.g., "fast: 150ms, base: 200ms, slow: 300ms">
- ...

<If not defined: "Not present in design system; will be defined in CONSTITUTION.md Motion section.">

### Easings

<If defined:>
- `<token-name>`: <e.g., "ease-out: cubic-bezier(0.16, 1, 0.3, 1)">
- ...

<If not defined: same fallback as motion.>

### Type scale

- `<name>`: <size, line-height, weight if specified>
- ...

---

## Atoms (available)

<For each component found in the codebase:>

### `<ComponentName>`

- **Variants:** <e.g., primary, secondary, ghost, destructive>
- **Sizes:** <e.g., sm, md, lg>
- **Props signature:** <brief — key props only>
- **File:** `<relative path>`

---

## Molecules (available)

<For each composite of atoms found:>

### `<ComponentName>`

- **Composes:** <list of atoms it wraps>
- **Purpose:** <what it does>
- **File:** `<relative path>`

---

## Partial composites (started)

<For each more-complex component that's started but incomplete:>

### `<ComponentName>`

- **Composes:** <list>
- **Built so far:** <description>
- **Missing:** <what's incomplete>
- **File:** `<relative path>`

---

## Gaps observed

Required primitives not present in the design system. These block specific downstream composites.

| Missing primitive | Needed by | Priority |
|---|---|---|
| <e.g., Skeleton> | DataTable, CommandPalette | P0 |
| <e.g., Virtualization utility> | DataTable | P0 |
| <e.g., Floating layer (Popover base)> | DropdownMenu, Tooltip | P0 |
| ... | ... | ... |

---

## Constraints for downstream SPECs

**Hard rule:** All composite SPECs must reference atoms from this inventory by exact name. Missing atoms must be flagged in the SPEC's `Primitive Gaps` section, never invented.

**Soft rule:** If a composite SPEC needs a *variant* of an existing atom (e.g., a new Button size), that's design-system work — flag it as a gap, don't invent inline.

**Workflow:** When a composite SPEC identifies a gap, the user has three options:
1. Add the primitive to the design system before specifying the composite
2. Defer the composite until the primitive is added
3. Substitute with an existing primitive and note the substitution

The framework does not pick for the user; it surfaces the choice.

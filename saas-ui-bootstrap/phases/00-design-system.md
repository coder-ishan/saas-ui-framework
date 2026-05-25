# Phase 0 — Design System Discovery

**Purpose:** Inventory what the user already has. The framework constrains every downstream SPEC to use existing primitives, not invent. Missing primitives become explicit gaps for the design-system track — separate work.

**Produces:** `.saas-ui/INVENTORY.md`

**Estimated time:** 15–25 min

## Why this phase comes first

If we don't establish the inventory before catalog phases (2, 3), composite SPECs will invent atoms inline and ship broken code. Inventory-as-constraint is the single most load-bearing artifact for downstream quality.

## Opening question

Ask the user:

> Do you have an existing design system or component library in this project? It could be:
> - A folder of components (e.g., `src/components/ui/`, `app/components/`, `packages/ui/`)
> - A published package (e.g., `@yourorg/ui`, shadcn/ui installed locally)
> - A Storybook
> - Design tokens in `tailwind.config.{js,ts}` or a tokens file
> - A Figma file (point me to a source-of-truth doc if so)
>
> Show me where it lives, or paste a few representative file paths. If you don't have one yet, tell me — we'll set up the inventory differently.

## Branch A — user has an existing design system

1. **Get paths.** Ask for the component directory, tokens file, Tailwind config, and any pattern docs.
2. **Read systematically.** Use `Glob` to enumerate component files. Read each to extract: component name, prop signature, variants, sizes. Read tokens config to extract: color names, spacing scale, radii, motion durations/easings (if present), type scale.
3. **Detect partial composites.** Anything more complex than an atom (e.g., a DataTable wrapper, a CommandPalette wrapper) gets noted as a "partial composite."
4. **Identify gaps.** Compare what's there to a mental checklist of enterprise-SaaS necessities:
   - Layout primitives (Stack, Box/Frame, Divider)
   - Form primitives (Input, Textarea, Select, Checkbox, Radio, Switch, Label, FormField)
   - Action primitives (Button with all variants/sizes, IconButton)
   - Overlay primitives (Dialog, Sheet, Popover, Tooltip, DropdownMenu)
   - Display primitives (Avatar, Badge, Card, Tag/Chip, Skeleton)
   - Feedback primitives (Toast, Alert, Banner)
   - Navigation primitives (Tabs, Breadcrumb)
   - Focus-ring utility, motion utilities, scrollbar styles
   - Virtualization primitive (for >1k row support)
   - Floating layer (for popovers, dropdowns)
5. **Confirm interpretations.** Show the user what you've inventoried and ask them to correct, add, or remove. Don't proceed silently — the inventory must be agreed.

## Branch B — user has no design system yet

1. **Ask about library intent.** What is their plan for primitives? Options:
   - shadcn/ui (copy-paste, Tailwind-native, Radix under the hood) — default for opinionated Next.js 2026
   - Radix Themes / HeadlessUI / Ariakit (headless primitives)
   - Custom from scratch (rare, justify)
   - Other (let them name it)
2. **Note that this becomes design-system work.** This skill does not build the design system. INVENTORY.md captures the *intent* (which lib + which atoms expected). All composite SPECs will treat unbuilt primitives as gaps until they exist.
3. **Write INVENTORY.md** with intent and gap-list filled.

## Writing INVENTORY.md

Use `templates/INVENTORY.md` as the structure. Required sections:
- **Source** — paths, packages, or "to be built using <lib>"
- **Tokens** — colors (names only, not hex; the framework cares about token *vocabulary*), spacing scale, radii, motion (if defined), type scale
- **Atoms (available)** — name + variants + sizes + brief signature
- **Molecules (available)** — name + what it composes
- **Partial composites (started)** — name + state + what's missing
- **Gaps observed** — required atoms not present + which composites/modules will need them
- **Constraints for downstream SPECs** — single statement: "all composite SPECs must reference atoms from this inventory by exact name; missing atoms must be flagged in the SPEC's Primitive Gaps section, never invented."

## Success criteria

Phase 0 is complete when:
1. INVENTORY.md exists with all sections populated (or marked "none" honestly).
2. User has confirmed the inventory matches reality.
3. Gaps are listed explicitly with which downstream composites/modules they affect.
4. STATE.md updated: `phase_0_complete: true`, `current_phase: 1`.

## What NOT to do

- Don't infer atoms that you didn't directly observe in code. If you didn't read the file, don't list the component.
- Don't propose new atoms inline ("I notice you don't have a CommandMenu — I'll add one"). Gaps are flagged, not built here.
- Don't proceed to phase 1 without user confirming the inventory.
- Don't list code blocks or implementation in INVENTORY.md — it's a manifest, not a tutorial.

## Checkpoint

When phase 0 completes, ask the user: "Inventory is done. Continue to phase 1 (product), or pause here?"

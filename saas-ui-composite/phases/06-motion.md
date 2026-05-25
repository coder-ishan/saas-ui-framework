# Phase 6 — Motion overrides

**Purpose:** Catch and justify any motion that diverges from CONSTITUTION canonical patterns. The default is "no override needed." This phase exists to make overrides expensive — every divergence requires an argument.

**Produces:** `MOTION.md` for this composite.

**Estimated time:** 5–10 minutes for most composites. The point is brevity.

---

## The default

For most composites, MOTION.md is short:

```markdown
# <Composite name> — Motion

No overrides. CONSTITUTION → Motion canonical patterns apply for all transitions:
- Modal/drawer open: per canonical pattern
- Popover/dropdown: per canonical pattern
- Toast: per canonical pattern
- Row expand: per canonical pattern
- Status change: per canonical pattern

No custom durations, no custom easings, no springs introduced.
```

That's it. Many composites stop here. Resist the urge to invent motion — CONSTITUTION made the global decision intentionally.

---

## When an override is warranted

An override is warranted only when:

1. **The canonical pattern doesn't cover the motion** (a new composite-specific transition not anticipated by CONSTITUTION → Motion → Canonical motion patterns)
2. **The canonical pattern produces a perceptual problem in this composite** (e.g., a 200ms modal open feels slow when the modal is small and adjacent — argue for 150ms here)
3. **The composite involves choreography across multiple elements** that requires its own timing rules

In any of these cases, the override goes here with a `Reason:` line.

---

## Override format

```markdown
# <Composite name> — Motion

## Overrides

### <transition name>

- **Canonical:** <what CONSTITUTION says, if anything>
- **Override:** <new duration / easing / pattern>
- **Reason:** <why the canonical doesn't fit>
- **Where used:** <which states or interactions>

### <transition name>

- ...

## Composite-specific patterns

Patterns this composite introduces that aren't in CONSTITUTION canonical list. These should be candidates for promotion to CONSTITUTION if other composites need the same.

### <pattern name>

- **What it is:** <description>
- **Duration:** <ms>
- **Easing:** <cubic-bezier or named>
- **Where used:** <state or interaction>
- **Promote to CONSTITUTION?** yes (other composites likely need this) | no (composite-specific)

## Forbidden in this composite

Restate CONSTITUTION → Motion → Forbidden (no bounce, no fade-only, no >300ms default, no simultaneous animations on >3 elements). Add composite-specific forbidden patterns if any.
```

---

## DataTable example (typical: light overrides)

```markdown
# DataTable — Motion

No global overrides. CONSTITUTION → Motion canonical patterns apply.

## Composite-specific patterns

### Row selection visual change

- **What it is:** Row background tint applies when checkbox is toggled.
- **Duration:** 100ms (faster than CONSTITUTION default 200ms for state changes).
- **Easing:** ease-out (CONSTITUTION default appearance easing).
- **Where used:** State `selected` enter/exit.
- **Reason:** Selection toggles happen rapidly during multi-select; 200ms would feel laggy across 10 quick selections.
- **Promote to CONSTITUTION?** No — selection-toggle speed is composite-specific concern.

### Selection-mode strip enter/exit

- **What it is:** Selection-mode strip in Toolbar slides in from above when selection > 0; slides out when selection = 0.
- **Duration:** 150ms.
- **Easing:** CONSTITUTION default appearance easing.
- **Where used:** Toolbar selection-mode strip (anatomy node).
- **Reason:** The right cluster of Toolbar is replaced; a position shift is the only signal. Fade-only is forbidden by CONSTITUTION.
- **Promote to CONSTITUTION?** Yes if other composites use selection-mode strips (CommandPalette doesn't, but ListView might).

## Forbidden (composite-specific in addition to global)

- No row-level entrance animations on data load. Reason: with 50+ rows, staggered enter is noise. Skeleton → data is instant per CONSTITUTION → State Model.
- No animated sort transitions. Reason: rows reordering during sort with animation creates ambiguity about which row is which (especially with selection). Sort is instant.
```

---

## Success criteria for phase 6

- If no overrides: the file is short, explicit, and points at CONSTITUTION.
- If overrides: each has a Canonical, Override, Reason, and Where used line.
- Composite-specific patterns identify whether they should promote to CONSTITUTION.
- Composite-specific forbidden additions extend (don't replace) CONSTITUTION's global forbidden list.

---

## What NOT to do in phase 6

- **Don't restate CONSTITUTION values.** "Modal: scale 0.96 → 1, 200ms ease-out" — if that's the canonical, just reference it. Restating creates drift over time when CONSTITUTION changes.
- **Don't add motion for delight.** The composite serves a job. CONSTITUTION → Motion → Philosophy already locked the position on functional vs delightful motion. Don't re-litigate here.
- **Don't introduce springs.** Springs in functional SaaS UI are forbidden by CONSTITUTION → Motion → Forbidden. If you need a spring, argue for a CONSTITUTION change first.
- **Don't bury an override.** If you override, the override must be findable. Every override has its own subsection.

---

## Checkpoint

> "Two checks:
> 1. Is every override here something you'd defend in a design review? If not, delete it.
> 2. Any composite-specific patterns that should promote to CONSTITUTION? If yes, note it and we'll batch CONSTITUTION updates after a few composites have specced."

Iterate (usually fast). Lock. Move to phase 7.

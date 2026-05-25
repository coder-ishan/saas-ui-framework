# <Composite name> — Variants

> Per-module deltas from the canonical SPEC. Only canonical composites have variants.
> If Type = single-module or bespoke, this file says "N/A" and nothing else.

---

## Variants

### Variant: <module name>

- **Module tier:** anchor | occasional | rare
- **Context:** <one-line — what the user is doing in this module>

#### Action Inventory deltas

| Action | Status here | Reason |
|---|---|---|
| | hidden / added / renamed / reordered | |

#### State deltas

| State | Reachable here? | Reason |
|---|---|---|
| | unreachable / always / sometimes | |

#### Copy deltas

- Empty-state copy: ...
- Error copy: ...
- Search placeholder: ...

#### Column / atom restrictions

- ...

#### Module-specific additions

- ...

> If additions exceed two atoms, flag for review — the module may be growing a separate composite.

#### Keyboard deltas

| Shortcut | Behavior change | Reason |
|---|---|---|
| | | |

#### Rendering deltas

- Cache tag: ...
- Revalidation: ...
- Server Action namespace: ...

---

### Variant: <module name>

<repeat for each module>

---

## Cross-variant consistency

- **Same action, different name across variants:** <list any flagged>
- **Same atom, different visual treatment:** forbidden — list any violations
- **Variant adds an action that two modules share:** <list for promotion>
- **Three+ variants restrict the same state:** <flag for canonical revisit>

---

## Variant inventory summary

| Module | Tier | # action deltas | # state deltas | # additions | Risk flags |
|---|---|---|---|---|---|
| | | | | | |

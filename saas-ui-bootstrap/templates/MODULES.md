# Module Catalog — <PRODUCT NAME>

> Modules are coherent areas of the product where flows live.
> They sit between Project (the whole app) and Composite (reusable UI blocks).
> This catalog is discovered via the "first week" interview in phase 2 — not derived from a template.

**Last updated:** <YYYY-MM-DD>

---

## Module tier definitions

- **Anchor:** user visits daily, spends significant time. Highest polish budget.
- **Occasional:** user visits weekly. Medium polish budget.
- **Rare:** user visits monthly or in specific contexts (admin, billing, onboarding). Minimal polish budget — functional, not delightful, by design.

---

## Modules

<For each module discovered:>

### <Module name>

- **Tier:** anchor | occasional | rare
- **One-line purpose:** <crisp summary — if you can't write this crisply, it's not yet a module>
- **Primary user goal here:** <what they're accomplishing>
- **Polish budget tier:** <high | medium | minimal — derived from tier + wedge alignment>

**Key flows (rough — detailed in `modules/<name>/FLOWS.md` by `saas-ui-module`):**
- <flow 1>
- <flow 2>
- <flow 3>

**Likely composites used (confirmed in phase 3):**
- <composite>
- <composite>

**Notes / open questions:**
- <anything user flagged>

---

## Module dependency notes

<List structural dependencies between modules, e.g.:>
- Settings → Workspace must exist before per-module settings work
- Onboarding → must complete before any anchor module is functional
- Billing → independent (visited rarely)

---

## Polish budget summary

| Module | Tier | Polish budget |
|---|---|---|
| <module> | <tier> | <h/m/min> |
| <module> | <tier> | <h/m/min> |

---

## Build order suggestion

Modules in suggested build order, based on:
1. Anchor modules with most user-visible flows first
2. Modules with fewest unbuilt composite dependencies first
3. Onboarding last (it depends on everything else existing)

1. <module>
2. <module>
3. <module>
...

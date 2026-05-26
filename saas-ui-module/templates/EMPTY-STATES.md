# <Module name> — Empty / error / loading copy

## Per-flow per-context copy

### <Flow / Screen name> — True-empty

- **Surface:** ...
- **Composite state:** empty (true-empty) — see <composite>/STATES.md
- **Copy:**
  - **Header:** ...
  - **Body:** [What's absent] · [Why / context] · [Single recommended action]
  - **Primary action label:** ...
  - **Secondary action (optional):** ...
- **Illustration:** present | absent

### <Flow / Screen name> — Filter-empty

- **Surface:** Body area only
- **Composite state:** empty (filter-empty)
- **Copy:**
  - **Header:** "No <items> match these filters."
  - **Body:** "Adjust filters or clear them to see all <total> <items>."
  - **Primary action:** "Clear filters"
- **Illustration:** absent

### <Flow / Step name> — Error (recoverable)

- **Surface:** inline | banner | toast
- **Composite state:** error (recoverable)
- **Copy structure:** [What failed] · [Why] · [Recovery]
- **Examples:**
  - ...
- **Recovery action label:** verb-first

### <Flow / Step name> — Error (terminal)

- **Surface:** Full screen or section banner
- **Composite state:** error (terminal)
- **Copy:** Name the failure explicitly.
- **Examples:**
  - ...
- **Recovery:** Navigation, not retry.

---

## Loading copy

### Long-load (>5s)
- At 5s: ...
- At 15s: ...

### Long-mutation
- In-progress: ...
- Success: ...
- Failure: ...

---

## Shared copy registry

| Phrase | Use case | Exact wording |
|---|---|---|
| Save success | | |
| Discarded | | |
| Network down | | |
| Item count (plural) | | |

---

## Pluralization notes

- <irregular plurals>

---

## Domain glossary references

See OVERVIEW.md → Module-specific glossary.

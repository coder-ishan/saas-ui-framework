# UX Principles — Laws of UX Applied to <PRODUCT NAME>

> Project-level Laws of UX, applied to this specific product.
> Module-level laws live in `modules/<name>/LAWS.md`.
> Composite-level laws live in `composites/<name>/LAWS.md`.
> Source: `references/laws-of-ux.md`.

---

## 1. Doherty Threshold (<400ms)

**Project stance:**
- Interaction latency budget: <ms>
- Navigation latency budget: <ms>
- Mutation strategy: <optimistic-with-rollback | wait-and-confirm>

**Implementation patterns (Next.js):**
- <e.g., Streaming SSR + Suspense for initial paint <400ms>
- <e.g., useOptimistic + toast-with-undo for mutations>
- <e.g., useDeferredValue for search-as-you-type>

**Where we accept slippage and why:**
- <e.g., report generation can take 2-10s with progress + cancel>

---

## 2. Jakob's Law

**Conventions we inherit:**
- <e.g., Cmd+K command palette>
- <e.g., Esc closes modals>
- <e.g., j/k list navigation>
- <list>

**Conventions we intentionally break:**
- <break> — Reason: <why the break is worth the friction>
- <break> — Reason: <…>

---

## 3. Tesler's Law (Conservation of Complexity)

For each major flow, who absorbs the irreducible complexity:

| Flow | Absorbed by | Surfacing / hiding strategy |
|---|---|---|
| <flow> | <user / app / admin / support> | <e.g., smart defaults + advanced toggle> |
| <flow> | <…> | <…> |

---

## 4. Pareto Principle (80/20)

**The 20% of flows getting 80% of the polish:**
1. <flow>
2. <flow>
3. <flow>

**Everything else:** functional, not delightful, by design. No apologies.

---

## 5. Postel's Law

**Forgiveness defaults:**

| Input | Rule |
|---|---|
| Dates | <accepted formats> |
| Currency | <accepted formats> |
| Emails | <paste handling> |
| Phones | <normalization> |
| URLs | <scheme handling> |
| Search | <tolerance> |
| File drag-drop | <accepted types> |

**Conservative in what we send:**
- API contracts: strict typing, validated outputs
- Persisted data: canonical formats

---

## 6. Aesthetic-Usability Effect

**Polish floor for this product (cross-references Density section of CONSTITUTION.md):**
- Spacing rhythm: <restated>
- Typography rhythm: <restated>
- Color discipline: <e.g., "color used only for: status, primary action, anomaly. No decorative color.">
- Alignment: <e.g., "column edges align to grid; nested content aligns to parent inner edge">

**Minimum standard:** below this, no screen ships.

---

## 7. Cognitive Load

**Posture:** <Linear-minimalist | ClickUp-maximalist-organized>

**How we manage load:**
- <e.g., progressive disclosure: common ops at top, advanced behind toggle>
- <e.g., max 7 visible columns in tables; "+more" reveals rest>
- <e.g., one primary action per screen — Von Restorff>
- <e.g., persistent navigation rather than reset-on-page>

---

## 8. Mental Model

**Metaphor:** <inbox | canvas | board | spreadsheet | other>

**Inherited from:**
- <product> — what we borrow
- <product> — what we borrow

**Intentionally broken:**
- <model broken> — Reason: <why the break is worth the cost>

---

## Cross-cutting reminders

These five laws don't get their own sections but inform every artifact:

- **Choice Overload:** progressive disclosure in settings, filters, command palette
- **Cognitive Bias:** make defaults intentional (status-quo bias is powerful); destructive actions undoable not confirmation-gated
- **Paradox of the Active User:** UI teaches itself; empty states explain; first-use does the work, not a docs page
- **Selective Attention:** one thing must win per screen
- **Working Memory:** persist intermediate state (autosave); avoid modal stacking

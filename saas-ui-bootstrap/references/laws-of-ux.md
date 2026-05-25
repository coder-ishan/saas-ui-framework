# Laws of UX — curated per-level mapping

Source: lawsofux.com (Yablonski). Thirty laws. The framework applies them at three levels — project, module, composite — with each law assigned where it does the most useful work. Do not apply every law at every level; that produces noise. Apply the laws assigned to the level you are working at.

---

## PROJECT LEVEL — sections in PRINCIPLES.md

Eight laws shape the product's overall philosophy. Each becomes a section in PRINCIPLES.md with a project-specific stance.

### 1. Doherty Threshold (<400ms)
Productivity soars when computer and user interact at <400ms.

**Project stance to elicit:** What is our latency budget for perceived feedback? Where in the app must we be under 400ms (sort, filter, search, status change)? Where can we slip (initial dashboard load, report generation)?

**Implementation cue:** Streaming SSR + Suspense in Next.js can make perceived initial paint <400ms even when data isn't ready. Optimistic UI (`useOptimistic`) keeps mutations under the threshold. This is architecture, not optimization.

### 2. Jakob's Law
Users prefer your site to work like other sites they already know.

**Project stance to elicit:** Which SaaS conventions do we match? (Cmd+K command palette, left-nav, escape closes modals, j/k list nav.) Which do we *intentionally* break, and why is the break worth the friction?

**Implementation cue:** Conventions are inheritances. List them explicitly so we don't accidentally break them.

### 3. Tesler's Law (Conservation of Complexity)
There is irreducible complexity in any system; the only question is who absorbs it.

**Project stance to elicit:** For each major flow, who absorbs the complexity — user, app, admin, support? When do we hide complexity (smart defaults) vs surface it (power-user controls)?

### 4. Pareto Principle (80/20)
80% of effects come from 20% of causes.

**Project stance to elicit:** Which 20% of flows deserve 80% of the polish budget? Name them. Everything else is "functional, not delightful" by design.

### 5. Postel's Law
Be conservative in what you send, liberal in what you accept.

**Project stance to elicit:** Where do we accept loose input? Paste handling (dates, emails, phone, currency), search query forgiveness, drag-and-drop file types, URL state restoration. What's our default forgiveness posture?

### 6. Aesthetic-Usability Effect
Aesthetically pleasing design is perceived as more usable.

**Project stance to elicit:** Where is the "polish floor" — what's the minimum aesthetic standard below which no screen ships? Define for: typography rhythm, spacing rhythm, color usage, alignment discipline.

### 7. Cognitive Load
Mental resources required to understand the interface.

**Project stance to elicit:** What's our cognitive-load posture — minimalist (Linear) or maximalist-but-organized (ClickUp)? How do we measure it (visible info density, choices per screen)?

### 8. Mental Model
The compressed model users carry of how the system works.

**Project stance to elicit:** What mental model are we building? What metaphor (inbox? canvas? spreadsheet? graph?). What models are we *borrowing* (Gmail-like? Linear-like?). What are we *breaking* and why?

---

## MODULE LEVEL — sections in modules/<name>/LAWS.md

Six laws shape user journeys through a module. Each module evaluates these.

### 9. Peak-End Rule
People judge experience by its peak and end.

**Application:** For each module, identify the *peak moment* (the thing that defines the module's emotional high — task created, deal closed, report rendered) and the *end moment* (last interaction before user leaves the module). Both must be polished beyond the rest.

### 10. Goal-Gradient Effect
Motivation increases with proximity to goal.

**Application:** Multi-step modules (onboarding, checkout, setup wizards) should show progress that accelerates near completion. Use it for: step indicators, completion percentages, "one more thing" framing.

### 11. Zeigarnik Effect
People remember incomplete tasks better than completed ones.

**Application:** Surface incomplete tasks; resume drafts; remind of unfinished items at module entry. Be careful: this is a *retention lever* but overuse becomes nagging.

### 12. Serial Position Effect
First and last items in a list are most memorable.

**Application:** Lists, menus, navigation. What goes first (primary action, most-used)? What goes last (escape hatches, settings)? Middle is for "everything else."

### 13. Flow
The state of fully immersed focus.

**Application:** What could break flow in this module? Confirmation modals, page reloads, slow searches, sudden modal stacking. Identify and remove flow-breakers per module.

### 14. Selective Attention
The process of focusing on a subset of stimuli.

**Application:** What's the *one thing* the user must see when they enter this module? Visual hierarchy should make it impossible to miss. Everything else competes against it — and should lose.

---

## COMPOSITE LEVEL — sections in composites/<name>/LAWS.md

Ten laws (including the Gestalt set) shape individual interaction patterns.

### 15. Fitts's Law
Time to acquire a target is a function of distance and size.

**Application:** Click target sizes, hit areas, distance from typical pointer rest. Row click targets span full width. Action icons have larger hit areas (32px+) than visual size (14-16px). Primary actions live in predictable, easy-to-reach locations.

### 16. Miller's Law (7±2)
Average working memory holds 7±2 items.

**Application:** Default visible columns in tables ≤ 7. Menu groups ≤ 7 items before subgrouping. Filter chips ≤ 7 visible before "+more". Step counts in flows ≤ 7.

### 17. Hick's Law
Decision time grows with number and complexity of choices.

**Application:** Dropdowns, action menus, command palette. Two levels max. Primary actions inline, secondary in overflow. Don't show all options; show the most likely.

### 18. Von Restorff Effect
The one different element is most remembered.

**Application:** Exactly one primary action per screen — visually distinguished by *more than color* (weight + size + position). Anomaly indicators (failed sync, overdue) use a single dot/icon, not full restyling. **One primary or none** — two "primary" buttons cancel each other.

### 19. Working Memory
Limited capacity to hold and manipulate info during a task.

**Application:** Multi-step flows must persist intermediate state (autosave). Inline edits show the original value somewhere reachable (undo, escape-to-cancel). Long forms break into chunks.

### 20. Chunking
Breaking information into smaller groups.

**Application:** Form sections, table column groups, navigation hierarchy. Group by semantic relationship. Use whitespace and dividers; chunking is a Gestalt operation.

### 21. Law of Common Region
Elements in a shared boundary feel grouped.

**Application:** Cards, panels, sections with borders/background tints. Use to group related controls without verbose labels.

### 22. Law of Proximity
Things close together feel grouped.

**Application:** Spacing scale enforces this. Tight spacing (4-8px) = related. Loose spacing (16-24px+) = separate groups. The framework's density choices are mostly proximity choices.

### 23. Law of Similarity
Similar elements feel like a group.

**Application:** Same shape, color, typography → same function. Two buttons that look the same should *do* the same kind of thing. (This is also the rule that catches duplicate-purpose buttons: if two buttons look the same and live in the same region, users assume they do different things.)

### 24. Law of Uniform Connectedness
Visually connected elements feel more related than disconnected ones.

**Application:** Lines, shared backgrounds, brackets. Stronger than proximity for showing relationship. Use for tabs, breadcrumbs, connected steps.

### 25. Law of Prägnanz
The eye prefers the simplest interpretation.

**Application:** Simplify ambiguous hierarchies. If a layout requires the user to "figure out" what groups with what, the layout is failing this law. Add structure (lines, regions, alignment).

---

## CROSS-CUTTING — applies at every level

Five laws are mentioned where relevant but don't have their own sections.

### 26. Choice Overload
Too many options overwhelm.

**Where it bites:** Settings, filters, command palette, "create" menus. Bias toward progressive disclosure: show common, hide advanced.

### 27. Cognitive Bias
Systematic errors in human thinking.

**Where it bites:** Defaults are powerful (status-quo bias). Confirmation flows can be skipped (optimism bias). Make destructive actions undoable rather than confirmation-gated.

### 28. Paradox of the Active User
Users won't read docs; they start using immediately.

**Where it bites:** UI must teach itself. Empty states explain what to do. First-use experience does the explaining, not a docs page. Tooltips and helper text used sparingly and where misuse is otherwise likely.

### 29. Selective Attention (cross-cutting nuance)
Beyond the per-module application: at the visual-design level, attention is finite. Things compete; not everything can win.

### 30. Working Memory (cross-cutting nuance)
Beyond the per-composite application: across the whole product, working memory bleeds across screens. Modal stacking and deep navigation are working-memory killers.

---

## How the framework uses this file

- **`saas-ui-bootstrap`** loads this file when writing PRINCIPLES.md (project-level laws 1-8). Phases 4-7 (motion, density, copy, state) cross-reference relevant laws when prompting for user stances.
- **`saas-ui-module`** (future skill) loads this file when writing per-module `LAWS.md` (module-level laws 9-14).
- **`saas-ui-composite`** (future skill) loads this file when writing per-composite `LAWS.md` (composite-level laws 15-25).
- **`saas-ui-audit`** (future skill) loads this file as one of its rubrics when auditing artifacts.

Every artifact that references a law cites it by *name* and *number* so audits can trace.

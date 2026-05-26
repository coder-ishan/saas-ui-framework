# Module-level Laws of UX — cheat sheet

> Quick reference. Full per-law application lives in `modules/<name>/LAWS.md`.

These 6 laws have the highest leverage at module level. Project-level laws (Doherty, Jakob's, Tesler's, Pareto, Postel, Aesthetic-Usability, Cognitive Load, Mental Model) live in `.saas-ui/PRINCIPLES.md`. Composite-level laws live in `~/.claude/skills/saas-ui-composite/references/composite-laws.md`.

---

## 1. Peak-End Rule

**Law (Kahneman):** Experiences are remembered by their peak intensity and their end — not by an average of all moments.

**At module level:** Don't try to make every screen equally polished. Identify the peak and the end of each primary flow; concentrate polish there.

**Checks:**
- Every P0 flow has an identified peak with concrete polish investment
- Every flow has an identified end with closure design
- Module-level dominant peak/end are identified
- Polish budget concentrates here

---

## 2. Goal-Gradient Effect

**Law:** As people approach a goal, they accelerate. Showing progress harnesses this.

**At module level:** Multi-step flows show progress indicators; final step is the actual outcome verb, not "Next"; no surprise input demands at the end.

**Checks:**
- Step indicator type matches step count (subtle for 2–3, stepper for 4–6, stepper+percent for 7+)
- Final step's primary affordance is the outcome verb
- Final step input cost is minimal
- No celebration screen for frequent flows

---

## 3. Zeigarnik Effect

**Law:** Incomplete tasks occupy working memory more than completed ones — they pull attention back. Productive return requires resumability.

**At module level:** Save-and-resume flows have explicit persistence contracts; resume drops user at the right step, not at start; incomplete tasks are surfaced at module home (capped at 3 per Miller's Law).

**Checks:**
- Persistence contract is explicit (what, when, where)
- Resume trigger is durable (URL or notification)
- Resume UI doesn't force re-traversal
- Module-home pinned-incomplete pattern present if applicable

---

## 4. Serial Position Effect

**Law:** First and last items in a list are remembered best; middle items blur.

**At module level:** Order items by frequency at the start, by rare-but-critical at the end. Destructive actions always last with separator.

**Checks:**
- Internal nav, dashboard widgets, menus, settings: order rationale documented
- First items earn primacy (most-frequent or default landing)
- Last items earn recency (rare-but-critical, or destructive)
- Destructive actions across all menus in module: last + separator (consistency per Forbidden Pattern #9)

---

## 5. Flow (Csikszentmihalyi)

**Law:** Users enter focused engagement when challenge meets ability with clear feedback. Once in flow, interruption is expensive.

**At module level:** Identify flow-state flows; restrict what can interrupt them. Modals only when irreversible. Drawer/page/modal pattern is consistent.

**Checks:**
- High-flow flows identified
- Interruption budget per flow (what's allowed, what's NOT)
- Modal discipline: only for irreversible actions
- No tours/coachmarks/upsells/announcements mid-flow

---

## 6. Selective Attention

**Law:** Users focus on one foreground thing at a time. Two competing things → oscillation.

**At module level:** One winner per screen. Notification placement strategy. Visual hierarchy enforced (one element at rank 1).

**Checks:**
- Every primary screen has a single visual winner
- Notification placement is strategic (toast / banner / notification center / inline)
- No multiple primary CTAs on a single screen

---

## How `saas-ui-audit` uses this

Each law's checks are verification points the audit can run:

- **Programmatic (audit-checkable):** progress indicator presence on multi-step flows; primary CTA count per screen; menu order patterns; pinned-task cap
- **Cross-flow (audit-flagged):** consistency of drawer/page/modal pattern; consistency of empty-state copy structure; consistency of destructive-action placement
- **Subjective (manual review):** is the peak really the peak? Does the end produce closure?

The audit produces a per-module scorecard with pass/fail/argued-deviation per law.

---

## Common module-level violations

The 5 most-shipped LLM-generated module violations:

1. **No identified peak.** "Every screen is equally important" — but Peak-End says one moment dominates memory. Concentrate.
2. **Modal-everywhere.** Used to confirm undoable actions, to display content that would fit inline, to present multi-step forms (page would be better). Per Forbidden Pattern #13 + this module law.
3. **Generic "No data" empty states.** Per Forbidden Pattern #5 + this law's Peak-End implication (true-empty for a new user IS a peak moment — design it).
4. **Alphabetical default sort regardless of user behavior.** Almost never the right default. The first item in a list under default sort should be the most-likely-relevant one.
5. **Tours and coachmarks for recurring users.** Tours are first-use only. Recurring tours violate Flow.

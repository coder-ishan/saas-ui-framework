# The 13 canonical states

> Every composite must address each of these in `STATES.md`. None may be "TBD."
> If genuinely inapplicable, mark "N/A" with a one-line reason.

The numbering goes to 14 because hover and focus are tracked separately (different mental contexts), but the framework refers to "the 13 canonical states" as shorthand because most teams collapse them.

---

## 1. idle

The default. No user attention. No interaction pending.

**Why it matters:** Defining idle precisely is the only way to define all other states (every state is *not idle plus a delta*).

**Anti-pattern:** A composite that has no clean idle — every visible thing pulses, animates, or pulls attention. Forbidden by CONSTITUTION → Forbidden Patterns #14.

**Canonical handling:** Steady. No motion. Default densities. Cursor neutral.

---

## 2. hover

Pointer-over an interactive element. Does NOT imply intent to click — many users hover-then-think.

**Why it matters:** Reveals tertiary affordances (row context menus, tooltip triggers), but mustn't trigger expensive operations.

**Anti-pattern:** Hover triggers expensive fetches, opens menus immediately, or commits state changes.

**Canonical handling:** Subtle visual change (CONSTITUTION → Density → Hover treatment), reveal of latent affordances, optional tooltip with 500ms delay.

---

## 3. focus

Keyboard focus on an interactive element. Distinct from hover.

**Why it matters:** Accessibility baseline. Many power users live in keyboard focus.

**Anti-pattern:** Focus ring missing or invisible; focus follows mouse (sometimes); focus indicators that don't survive forced-colors mode.

**Canonical handling:** CONSTITUTION-defined focus ring, visible against all backgrounds, NEVER suppressed (no `outline: none` without replacement).

---

## 4. selected

Explicit selection — user has chosen this item for a subsequent action.

**Why it matters:** Distinct from focus. Focus is "where I am"; selection is "what I picked." A user can focus row 5 while having rows 1, 2, 3 selected.

**Anti-pattern:** Conflating selection with focus (selecting moves focus, or focus selects). Both mental models exist; SaaS table UX conventions separate them.

**Canonical handling:** Distinct visual treatment — typically tint + checkbox checked + tonal change. Selection survives sort per Cross-state rules.

---

## 5. active-edit

Inline editing mode. User is actively modifying a value.

**Why it matters:** The most error-prone state — focus loss, dirty state, undo, validation, save all converge here.

**Anti-pattern:** Click-outside silently commits (or silently cancels) — both surprise the user. Validation only on submit. No dirty indicator.

**Canonical handling:** Clear visual distinction from idle/focus; explicit save/cancel affordance OR autosave with sync indicator (per CONSTITUTION → State Model). Esc cancels per Escape ladder.

---

## 6. loading-initial

First paint, no data yet. The composite is mounting.

**Why it matters:** Determines whether Doherty (<400ms) is met. The worst version is a full-screen spinner that says nothing.

**Anti-pattern:** Spinner. Empty white space. Layout shift on data arrival.

**Canonical handling:** Skeleton matching final layout exactly — same row count, same column widths, same typography rhythm. Per CONSTITUTION → State Model → Loading strategy.

---

## 7. loading-mutation

Data is visible; a write operation is pending.

**Why it matters:** Without optimistic UI, every save feels slow. With optimistic UI but no rollback, every failure becomes silent data loss.

**Anti-pattern:** Disable the entire UI during save. No indication something is happening. Save succeeds visually but actually failed.

**Canonical handling:** Optimistic apply (per CONSTITUTION → State Model → Mutation strategy), subtle pending indicator on the changed element, rollback toast on failure (Recovery → undo toast).

---

## 8. empty (true-empty)

No data ever existed. First-use, new account, freshly created module.

**Why it matters:** This is the user's first impression. A bad empty state ("No data") signals an unfinished product.

**Anti-pattern:** "No data." Generic stock illustration. Decorative without action.

**Canonical handling:** [What's absent] · [Why / context] · [Single recommended action] per CONSTITUTION → Copy → Empty state structure. The recommended action is the recovery — the primary CTA from the Action Inventory.

---

## 9. empty (filter-empty)

Data exists, filters/search excludes all.

**Why it matters:** Different mental context from true-empty. The user has data; they just can't see it. Recovery is "adjust filters," not "add data."

**Anti-pattern:** Same treatment as true-empty (offers "Add data" — which doesn't help). No way to clear filters quickly. No indication the issue is filter-based.

**Canonical handling:** "No rows match these filters." + Clear filters action + filter chip remains visible & adjustable.

---

## 10. error (recoverable)

Operation failed. User can retry, adjust, or try a different action.

**Why it matters:** Most errors are recoverable. Treating them as terminal blocks the user unnecessarily.

**Anti-pattern:** "Something went wrong." Modal that blocks the UI. No retry. No context about what failed.

**Canonical handling:** [What failed] · [Why if known] · [Recovery] per CONSTITUTION Copy. Inline error in the affected scope (field/section/page level), not full-screen takeover.

---

## 11. error (terminal)

Operation failed permanently. The user cannot recover from this state; they must navigate away.

**Why it matters:** Some errors are genuinely terminal — 404, permission revoked, account deleted. These need a different treatment than recoverable ones.

**Anti-pattern:** Mixing terminal and recoverable errors. Showing "Retry" when retry will always fail. Generic 404 page that doesn't help the user understand what happened.

**Canonical handling:** Name the failure explicitly. Offer navigation, not retry. Provide context ("This record was deleted by <user> on <date>") when available.

---

## 12. partial-permission

User can see data but some actions are restricted.

**Why it matters:** Multi-tenant SaaS with roles always has this state. Without designing it, the UI either hides too much (user doesn't know features exist) or shows too much (user clicks disabled buttons constantly).

**Anti-pattern:** Disable every action without explanation (Forbidden Pattern #10). Or hide them entirely without indication they exist for other users.

**Canonical handling:** Show what the user CAN do. Disabled actions show a tooltip naming the role required. Or hide actions entirely + show a "view-only" pill near the toolbar to signal the mode.

---

## 13. offline / stale

Connection lost, or data hasn't refreshed in too long.

**Why it matters:** SaaS apps rarely design for offline. When it happens, users are stuck or, worse, lose work.

**Anti-pattern:** Silent failures when network drops. Optimistic UI that doesn't queue. No indication data is stale.

**Canonical handling:** Per CONSTITUTION → State Model → Offline. Either: queue and replay (full local-first), graceful degradation (read-only mode), or explicit banner ("You're offline — changes won't save until reconnected").

---

## 14. long-content

Content overflows the region — long text, wide table, deep nesting, large file lists.

**Why it matters:** Real data is messy. The composite must handle real data, not the perfect-length placeholder strings of design mocks.

**Anti-pattern:** Truncation with no indication. Horizontal overflow that breaks the layout. Tooltips that don't appear on truncated content.

**Canonical handling:** Truncate with ellipsis. Tooltip on hover/focus reveals full content. For very long content: dedicated "expand" affordance (chevron, "Show more"). Cell content rule: single-line truncate by default; multi-line opt-in.

---

## Cross-state rules every composite should consider

- **Selection persistence:** Survives sort? Filter? Mutation?
- **Focus retention:** Where does focus go when an element is removed?
- **No simultaneous loading + error:** transitions are sequential.
- **Skeleton matches final:** prevents layout shift.
- **Empty (true) vs (filter):** mutually exclusive; true-empty wins if both could apply.

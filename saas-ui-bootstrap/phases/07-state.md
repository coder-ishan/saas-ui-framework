# Phase 7 — State Model

**Purpose:** Establish how the UI represents state — loading, errors, optimistic updates, autosave, dirty state, offline. State is where most "looks done but isn't" happens. Every composite SPEC will reference this.

**Produces:** `.saas-ui/CONSTITUTION.md` (State section) + `.saas-ui/PRINCIPLES.md` (Postel section finalized)

**Estimated time:** 15–20 min

## Interview prompts

### 1. Loading state strategy
> By duration tier, what's the loading affordance?
> - **<300ms:** nothing (don't show loading)
> - **300ms–2s:** skeleton matching final layout (not generic gray boxes)
> - **>2s:** skeleton + "Taking longer than usual" affordance at 5s
> - **Submit-button inflight:** spinner inside the button, button disabled
>
> Confirm or override per tier.
>
> Bonus rule (confirm): no full-screen spinners ever. If you need to indicate the whole page is loading, use a skeleton page or top-of-page progress bar.

### 2. Mutation strategy
> Three options:
> - **Optimistic everything** — UI updates instantly, server syncs in background, rollback toast on failure. (Linear's approach.)
> - **Optimistic for fast/safe mutations only** — sort, filter, status changes optimistic; structural changes (delete, create) wait-and-confirm.
> - **Wait-and-confirm everything** — UI waits for server response. Safer but feels slow.
>
> Most enterprise SaaS picks #1 or #2. Pick #1 if you can invest in proper rollback patterns. Pick #2 if you want to ship without that investment.

For the chosen option, codify:
> - Rollback affordance: toast with retry / inline error / both
> - Conflict handling: server-wins / client-wins / prompt-user
> - Pending visual: subtle (low-opacity row?) / explicit ("Saving..." badge) / invisible

### 3. Autosave vs explicit save
> Where does autosave apply?
> - All form fields → autosave on blur or debounce (mostly Linear/Notion approach)
> - Drafts only → autosave on draft state, explicit Save to commit
> - Settings → autosave (most apps)
> - Hard-commit actions (publish, send, charge) → explicit
>
> What's the canonical sync indicator? Suggestions:
> - Quiet footer text: "Saved · 2s ago"
> - Inline checkmark next to field that just saved
> - Status dot near workspace switcher

### 4. Dirty state
> Forms with explicit save (if any) need dirty-state tracking. What's the affordance?
> - "Unsaved changes" badge in the submit area
> - Disable navigation away with confirmation
> - Auto-revert on cancel
>
> If autosave is universal (#3), this question is partially moot — but ask anyway for any explicit-save forms.

### 5. Error state strategy
> By scope, what's the error affordance?
> - **Field-level error** (validation): inline below the field, red, named.
> - **Section-level error** (sub-form failed): banner at top of section.
> - **Page-level error** (data fetch failed): error placeholder where content would be, with retry.
> - **App-level error** (auth lost, network down): toast or persistent banner.
> - **Mutation error after optimistic apply:** rollback toast with retry.
>
> Confirm or override.

### 6. Offline / stale
> Does this app work offline or with degraded connectivity?
> - **No (most enterprise SaaS):** show banner when offline; queue mutations; replay on reconnect.
> - **Yes (Linear-like):** local-first with sync layer; never blocks on network.
> - **Partial:** specific modules cached, rest requires connection.
>
> What's the visual affordance for offline / stale data?

### 7. Recovery and undo
> Confirm: undo is the default destruction-recovery pattern (not "are you sure?"). For each destructive action, the undo window is:
> - **Inline action** (delete one item): 5-second toast undo
> - **Bulk action** (delete N items): 10-second toast undo
> - **Irreversible** (cannot be undone; e.g., permanent purge, send invoice): confirmation modal required, undo not available
>
> List which actions in this product are irreversible — they're the only ones with confirmation modals.

### 8. Postel's Law — input forgiveness
> Where do we accept loose input?
> - **Date parsing:** "next tuesday", "3pm", "tomorrow" — all parsed?
> - **Currency:** "$1,200.00", "1200", "1,200" — all accepted?
> - **Email paste:** multiple emails separated by commas/newlines/spaces?
> - **Phone:** any format normalized?
> - **URL:** with/without scheme?
> - **Search query:** typo-tolerant? case-insensitive? partial match?
> - **File drag-drop:** what MIME types?
>
> Decide per category. Forgiveness is a force-multiplier on perceived polish.

## What to write

### CONSTITUTION.md — State section

```markdown
## State Model

**Loading strategy:**
- <300ms: nothing
- 300ms–2s: skeleton matching final layout
- >2s: skeleton + "Taking longer than usual" affordance at 5s
- Submit-button inflight: spinner inside button, button disabled
- Full-screen spinners: forbidden
- Overrides: <list>

**Mutation strategy:** optimistic-everything | optimistic-for-safe | wait-and-confirm
  Reason: <user's rationale>
- Rollback affordance: <toast | inline error | both>
- Conflict handling: <server-wins | client-wins | prompt>
- Pending visual: <subtle | explicit | invisible>

**Autosave:**
- Applies to: <fields/forms/scope>
- Sync indicator: <description>
- Explicit save reserved for: <list of hard-commit actions>

**Dirty state (for explicit-save forms):**
- Indicator: <description>
- Navigation guard: <yes / no / context>

**Error handling by scope:**
- Field-level: inline below field, red, named
- Section-level: banner at top of section
- Page-level: error placeholder with retry
- App-level: toast or persistent banner
- Mutation post-optimistic: rollback toast with retry

**Offline / stale:**
- Support: <none | full local-first | partial>
- Affordance: <description>
- Queue/replay: <description>

**Recovery (undo over confirm):**
- Inline destructive: 5s undo toast
- Bulk destructive: 10s undo toast
- Irreversible (require confirmation): <list specific actions>

**Postel's Law — input forgiveness:**
- Date parsing: <accepted formats>
- Currency: <accepted formats>
- Email paste: <multi-email behavior>
- Phone: <normalization>
- URL: <scheme handling>
- Search: <tolerance>
- File drop: <accepted MIME types>
- <other categories>
```

### PRINCIPLES.md — Postel's Law section (finalize)

```markdown
## 5. Postel's Law

**Project stance: Forgiveness defaults**

| Input | Forgiveness rule |
|---|---|
| Dates | <accepted formats from Q8> |
| Currency | <accepted formats> |
| Emails | <paste handling> |
| Phones | <normalization> |
| URLs | <scheme handling> |
| Search | <tolerance> |
| File drag-drop | <accepted types> |

**Conservative in what we send:**
- API contracts: strict typing, validated outputs
- Persisted data: canonical formats
```

## Success criteria

Phase 7 is complete when:
1. CONSTITUTION.md State section fully populated.
2. PRINCIPLES.md Postel section finalized.
3. Irreversible actions are explicitly listed (the only ones with confirmation modals).
4. Postel forgiveness rules cover every input category that exists in the product.
5. STATE.md updated: `phase_7_complete: true`, `current_phase: 8`.

## What NOT to do

- Don't accept "we'll figure out optimistic later." It's a foundational choice that changes architecture.
- Don't let confirmation modals creep back in via "well, this one's also destructive." Use the irreversible list strictly.
- Don't write loose Postel rules. Be specific per input category.

## Checkpoint

When phase 7 completes, ask: "State model set. Continue to phase 8 (forbidden patterns), or pause here?"

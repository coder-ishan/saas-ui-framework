# <Composite name> — States

> All 13 canonical states. None may be "TBD" in a finished SPEC.
> Reference: `~/.claude/skills/saas-ui-composite/references/state-catalog.md`.

---

## 1. idle

- **When it occurs:** ...
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Copy (if any):** ...
- **Transitions out:** ...

## 2. hover

- **When it occurs:** ...
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Transitions out:** ...

## 3. focus

- **When it occurs:** ...
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Transitions out:** ...

## 4. selected

- **When it occurs:** ...
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Transitions out:** ...

## 5. active-edit

- **When it occurs:** ...
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Copy (if any):** ...
- **Transitions out:** ...

## 6. loading-initial

- **When it occurs:** First paint, no data yet.
- **Visual treatment:** Skeleton matching final layout (no full-screen spinners — forbidden by CONSTITUTION).
- **Allowed actions:** none (composite still mounting).
- **Transitions out:** Data arrives → idle. Error → error (recoverable).

## 7. loading-mutation

- **When it occurs:** Data visible, write pending.
- **Visual treatment:** Optimistic UI applied; subtle pending indicator per CONSTITUTION → State Model → Mutation strategy.
- **Allowed actions:** ...
- **Transitions out:** Success → idle. Failure → error (recoverable) + rollback toast.

## 8. empty (true-empty)

- **When it occurs:** No data ever existed.
- **Visual treatment:** ...
- **Allowed actions:** Add row (the recovery action) + any toolbar actions that still apply.
- **Copy:** [What's absent] · [Why / context] · [Single recommended action] — per CONSTITUTION Copy → Empty state structure.
- **Transitions out:** Data added → idle.

## 9. empty (filter-empty)

- **When it occurs:** Data exists, filters exclude all.
- **Visual treatment:** ...
- **Allowed actions:** Clear filters, adjust filters, search, switch view.
- **Copy:** "No rows match these filters." + "Adjust filters or clear them to see all <N> rows." + "Clear filters".
- **Transitions out:** Filter change with matches → idle (filtered). Clear → idle.

## 10. error (recoverable)

- **When it occurs:** Operation failed; user can retry.
- **Visual treatment:** ...
- **Allowed actions:** Retry; any actions not blocked by the error.
- **Copy:** [What failed] · [Why if known] · [Recovery] — per CONSTITUTION.
- **Transitions out:** Retry → loading-* → idle / error.

## 11. error (terminal)

- **When it occurs:** Permission denied, resource deleted, 404.
- **Visual treatment:** ...
- **Allowed actions:** Navigate away.
- **Copy:** Name the failure ("Access denied", "Account deleted"). Never "Something went wrong."
- **Transitions out:** Navigate (not back to idle).

## 12. partial-permission

- **When it occurs:** User can see data but some actions are restricted.
- **Visual treatment:** Restricted actions hidden or disabled with explanation.
- **Allowed actions:** ...
- **Transitions out:** Permission change → idle (full or different partial).

## 13. offline / stale

- **When it occurs:** Connection lost, or data hasn't refreshed in long time.
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Transitions out:** Reconnect → loading-initial → idle.

## 14. long-content

- **When it occurs:** Content overflows region (long text, wide table, deep nesting).
- **Visual treatment:** ...
- **Allowed actions:** ...
- **Transitions out:** Content shortened or scope changed → idle.

---

## Cross-state rules

1. <invariant>
2. <invariant>
3. <invariant>

---

## State machine (optional)

```text
<pseudocode if non-trivial transitions>
```

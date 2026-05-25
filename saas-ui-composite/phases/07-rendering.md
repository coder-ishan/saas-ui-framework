# Phase 7 — Rendering (Next.js)

**Purpose:** Lock the Server Component / Client Component boundary, the Suspense topology, the data-fetch strategy, and which React 19 primitives are used and why. This phase is what makes the framework Next.js-specific — and it's where most LLM-generated Next.js code drifts.

**Produces:** `SPEC.md` → Rendering section.

**Estimated time:** 15 minutes.

---

## The four-part rendering section

### Part 1 — Boundary map

Every anatomy node from phase 3 is either Server-rendered or Client-rendered. Decide per node.

```markdown
## Rendering

### Server / Client boundary

| Anatomy node | Server | Client | Why |
|---|---|---|---|
| Toolbar shell | ✓ | | Static layout, no interactivity at this level |
| Search input | | ✓ | Controlled input, debounced state |
| Filter chips | | ✓ | State (active filters), interactive removal |
| View switcher | | ✓ | Dropdown is interactive |
| Header row | ✓ | | Static structure |
| Header cells (label + sort indicator) | ✓ | | Sort state comes via URL params (server-readable) |
| Header chevron (facet trigger) | | ✓ | Opens popover (interactive) |
| Body container | ✓ | | Static |
| Row | ✓ for cells, ✓ Client for row-level interactions | Mixed | Cells render server-side from data; row-level click/hover/select is Client |
| Row context menu | | ✓ | Popover; only mounted when triggered |
| Empty state | ✓ | | Static |
| Footer (count) | ✓ | | Derived from server data |
| Pagination | | ✓ | URL state but with prefetch + transition |
```

**Rule:** When in doubt, prefer Server Component. Client Component is required only for: state that doesn't go in URL, refs to DOM, browser APIs, event handlers that aren't form-submit.

### Part 2 — Suspense topology

Specify where Suspense boundaries are and what they protect.

```markdown
### Suspense boundaries

```text
DataTable (Server Component)
  Toolbar (Server)
  <Suspense fallback={<TableSkeleton rows={20} />}>
    Body (Server async — awaits data)
  </Suspense>
  <Suspense fallback={<FooterSkeleton />}>
    Footer with count (Server async — awaits count)
  </Suspense>
```

- The toolbar streams immediately (shell paint within Doherty, ~100ms).
- Body and Footer are independent Suspense boundaries so the count can arrive separately from the data (count query is faster).
- Skeleton matches final row count and column widths exactly (STATES.md loading-initial requirement).
```

### Part 3 — Data fetch strategy

```markdown
### Data fetching

**Primary fetch:**
- Server Component reads sort/filter/search from URL params
- Fetch happens server-side via `<data layer>` (Drizzle / Prisma / direct DB / API)
- `fetch()` options: `{ next: { tags: ['table:<resource>'], revalidate: <seconds | false> } }`
- Revalidation triggers: <list of `revalidateTag` callsites>

**Secondary fetches:**
- Facet values (column filter dropdown): `<approach — separate Server Action, cached>`
- Saved views: `<approach>`

**Search:**
- Server-side via URL param (Doherty-friendly, sharable, back/forward-safe)
- `useDeferredValue` on the input controls when URL updates fire to keep typing responsive
- Debounce: 200ms before URL push

**Sort/filter state:**
- URL search params are the source of truth (`?sort=name&filter=status:active`)
- Reason: sharable URLs, browser back/forward works, no client-side state hydration mismatch
- Client navigation via `router.push(url, { scroll: false })` wrapped in `useTransition` so the body stays interactive during the transition

**Mutations (add/edit/delete row):**
- Server Actions for the canonical path
- `useOptimistic` for optimistic UI per CONSTITUTION → State Model → Mutation strategy
- `useFormStatus` for inflight submit-button state
- Rollback toast appears on action error (CONSTITUTION → State Model → Recovery)
```

### Part 4 — React 19 primitive usage

```markdown
### React 19 primitive usage

| Primitive | Used for | Reason |
|---|---|---|
| `useOptimistic` | Add row, delete row, edit cell, bulk delete | Doherty requires <400ms perceived; optimistic + rollback toast meets it |
| `useFormStatus` | Submit-button inflight state inside inline edit + add-row drawer | Avoid prop-drilling pending state into button components |
| `useTransition` | Sort/filter/search URL updates, pagination navigation | Keeps current data interactive while next render computes |
| `useDeferredValue` | Search input → URL param push | Keeps input responsive when typing fast |

**NOT used:**
- `use(promise)` for client-side data — server is the source; client receives streamed data via Suspense + RSC
- `useFormState` (legacy alias for `useActionState`) — we use `useActionState` if state-tracking actions are needed
- Manual `useEffect`-based loading flags — Suspense handles this
```

---

## Cross-cutting rules

```markdown
### Hydration

- Server-rendered cells must produce the same output as the first client render. No `Date.now()` or `Math.random()` in render paths. Time-sensitive values (relative timestamps) use a Client Component with `suppressHydrationWarning` or read from a stable input.
- No conditional rendering based on `typeof window`. Use `'use client'` boundaries instead.

### Caching

- Server data fetches are tagged. Tag namespace for this composite: `<namespace from CONSTITUTION or per-product convention>`.
- `revalidateTag` callsites are listed in mutation Server Actions.
- Cache headers for the route (if applicable): <e.g., `'force-dynamic'` for high-mutation tables, default for read-mostly>.

### Streaming

- Initial HTML response streams the Toolbar shell within <100ms TTFB target.
- Body Suspense boundary streams data when ready.
- Below-the-fold rows are not lazy-rendered in v1 (Contract caps at 1000 rows; virtualization is a deferred gap per Phase 3).
```

---

## What to write

The four parts above go directly into `SPEC.md` → Rendering section. No separate file.

---

## Success criteria for phase 7

- Every anatomy node from phase 3 is classified Server or Client
- Suspense boundaries are explicit (a code-fence diagram is required, not prose)
- Data-fetch strategy names: primary path, secondary paths, search handling, sort/filter source-of-truth
- React 19 primitives have a per-use justification
- Hydration constraints are spelled out
- Cache tag namespace is named

---

## What NOT to do in phase 7

- **Don't make everything a Client Component.** That's the LLM default and it surrenders SSR's perf benefits. Default is Server; Client is opt-in with a Reason.
- **Don't put mutation state in URL.** URL is for navigation state (sort, filter, search, page). Mutation state (pending, error, optimistic) is React state.
- **Don't use `useEffect` for data fetching.** Server Components fetch; Client Components receive props from the parent server tree.
- **Don't skip Suspense.** Loading state per CONSTITUTION must come from Suspense + skeleton, not a manual `if (isLoading)` branch.
- **Don't claim "uses useOptimistic" without saying where the rollback happens.** Optimistic without rollback is a lie. Rollback is part of the mutation strategy.

---

## Checkpoint

> "Three checks:
> 1. Walk through a full mutation: user clicks 'Delete row'. What's the sequence — optimistic removal, server action call, on-success no-op (already removed), on-error rollback and toast. Is the React 19 primitive choice right at each step?
> 2. Sort/filter via URL — does the back button work the way you'd expect? (It should restore the prior sort/filter; the table re-fetches server-side.)
> 3. Cache tags — when this table's data changes elsewhere in the app (admin edits, webhook arrives), does the right `revalidateTag` fire?"

Iterate. Lock. Move to phase 8.

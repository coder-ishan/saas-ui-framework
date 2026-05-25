# Next.js / React 19 primitives — when to use which

> Quick reference for phase 7 (Rendering). Each primitive has ONE canonical use.

---

## React 19 primitives

### `useOptimistic`

**Use for:** Mutations that need to feel instant — add, edit, delete, toggle, status change.

**Pattern:**
```text
const [optimisticState, addOptimistic] = useOptimistic(serverState, reducer)

// in handler:
startTransition(() => {
  addOptimistic(predicted)
  await serverAction()  // on error, optimistic state auto-reverts
})
```

**Don't use for:** Reads. Server-side fetches that have their own loading state. Mutations where rollback isn't supported by the UX (irreversible operations).

**Pair with:** Server Action + rollback toast (CONSTITUTION → State Model → Recovery).

---

### `useFormStatus`

**Use for:** Inflight state of a Server Action triggered by a `<form>` submission — typically the submit button shows a spinner.

**Pattern:**
```text
function SubmitButton() {
  const { pending } = useFormStatus()
  return <button disabled={pending}>{pending ? 'Saving…' : 'Save'}</button>
}
```

**Don't use for:** Mutations not triggered by form submit (use `useTransition` + manual pending flag instead). Components outside a `<form>` boundary (it returns null).

**Pair with:** Server Action via `<form action={action}>`.

---

### `useTransition`

**Use for:** Non-urgent UI updates that shouldn't block input — navigating to a new sort/filter URL, switching views, paginating.

**Pattern:**
```text
const [isPending, startTransition] = useTransition()

startTransition(() => {
  router.push(newUrl)
})

// while isPending: show subtle indicator (e.g., slight opacity reduction)
```

**Don't use for:** Mutations (use `useOptimistic`). Synchronous state updates that need to feel instant.

**Pair with:** `router.push(url, { scroll: false })` for in-place navigation.

---

### `useDeferredValue`

**Use for:** Deriving a non-urgent value from an urgent input — typical case is "as-you-type" search where the input stays responsive but the expensive computation/navigation lags slightly.

**Pattern:**
```text
const [query, setQuery] = useState('')
const deferredQuery = useDeferredValue(query)

useEffect(() => {
  router.push(`?q=${deferredQuery}`)
}, [deferredQuery])
```

**Don't use for:** Throttling/debouncing time-based events (use a debounce). Loading states (use Suspense).

**Pair with:** Server-rendered search results via URL param.

---

### `useActionState` (formerly `useFormState`)

**Use for:** Server Action results that need to be reflected back into the form UI (validation errors, success messages tied to the form).

**Pattern:**
```text
const [state, formAction] = useActionState(serverAction, initialState)
// state contains the latest action result; render errors inline
```

**Don't use for:** Mutations where you only care about success/failure toasts (rollback toast doesn't need this).

**Pair with:** Inline field errors per CONSTITUTION → State Model → Error handling by scope.

---

## Next.js App Router

### Server Components (default)

**Use for:** Everything by default. Static structure, data-fetching, server-side rendering, layout shells.

**Capabilities:** `async` functions, direct DB/data layer access, can render Client Components.

**Limitations:** No state, no effects, no event handlers, no browser APIs.

**Decision rule:** If the node doesn't need state, effects, refs, or event handlers, it's a Server Component. Period.

---

### Client Components (`'use client'`)

**Use for:** Interactivity. State. Effects. Refs. Event handlers. Browser APIs.

**Decision rule:** Smallest possible boundary. Don't `'use client'` a whole page — `'use client'` the smallest leaf that needs interactivity. The composite's toolbar shell stays Server; the search input within it is Client.

**Anti-pattern:** Marking every component Client out of habit. This surrenders SSR's performance benefits.

---

### Suspense boundaries

**Use for:** Splitting the page into streamed chunks so slower data doesn't block faster paint.

**Pattern:**
```text
<Suspense fallback={<TableSkeleton />}>
  <AsyncBody />
</Suspense>
<Suspense fallback={<FooterSkeleton />}>
  <AsyncFooter />
</Suspense>
```

**Decision rule:** Place a Suspense boundary at every point where the parallel data fetch matters. The toolbar should never wait for the table data; the count should never wait for the rows.

**Pair with:** Skeleton matching final layout (no layout shift on resolve).

---

### Server Actions

**Use for:** Mutations. They are POST endpoints under the hood, callable directly from Client Components.

**Pattern:**
```text
async function deleteRow(id: string) {
  'use server'
  await db.row.delete({ id })
  revalidateTag(`table:${resource}`)
}

// in Client Component:
<form action={deleteRow}>
  <input type="hidden" name="id" value={id} />
  <SubmitButton />
</form>
```

**Pair with:** `useOptimistic` for immediate UI response, `revalidateTag` for cache invalidation, `useFormStatus` for submit-button state.

---

### Cache (fetch + revalidateTag)

**Use for:** Server-side caching of data fetches, with tag-based invalidation.

**Pattern:**
```text
// fetch:
const data = await fetch(url, { next: { tags: ['table:invoices'] } })

// in a mutation:
revalidateTag('table:invoices')  // invalidates all tagged fetches
```

**Decision rule:** Every composite that fetches data declares its cache tag namespace in SPEC.md → Rendering → Caching. Mutations within the composite (and elsewhere that affects this data) call `revalidateTag` with that namespace.

---

### `loading.tsx` and `error.tsx`

**Use for:** Route-level loading and error boundaries.

**Decision rule:** Composite-level loading/error usually goes inside the composite via Suspense + try/catch (or the new `error.tsx` at the appropriate nested route). Route-level `loading.tsx` is for the initial route transition before any composite renders.

---

## Common anti-patterns

| Anti-pattern | What to use instead |
|---|---|
| `useEffect(() => fetch(...), [])` in Client Component | Server Component with direct fetch |
| Global loading spinner | Suspense + skeleton (CONSTITUTION → State Model) |
| `useState` for sort/filter | URL search params + `useTransition` for navigation |
| Mutation via API route | Server Action |
| `'use client'` at page top to enable any interactivity | `'use client'` only on the leaf that needs it |
| Optimistic UI without rollback | `useOptimistic` + Server Action + rollback toast |
| Cache invalidation by reloading the page | `revalidateTag` with namespaced tags |

---

## Cheat sheet by use case

| Use case | Primitive |
|---|---|
| Add a row, feel instant | Server Action + `useOptimistic` + rollback toast |
| Save form, show spinner in button | `<form action={action}>` + `useFormStatus` |
| Sort table by column | URL params + `useTransition` for the navigation |
| Search as you type | `useDeferredValue` on input → URL param |
| Show validation errors after save | `useActionState` |
| Stream initial render (toolbar fast, body slow) | Server Component + Suspense boundaries |
| Invalidate cache after mutation | `revalidateTag` in the Server Action |
| Make non-interactive structure stay Server | Default — don't add `'use client'` |

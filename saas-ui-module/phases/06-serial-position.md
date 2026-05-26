# Phase 6 — Serial Position Effect

**Purpose:** Order items in lists, navigation, and dashboards based on what users remember best — first and last. Place rare-but-critical items at the ends, not the middle.

**Produces:** `LAWS.md` → Serial Position section.

**Estimated time:** 10 minutes.

---

## The law

**Serial Position Effect:** In a list, users remember the first item (primacy) and the last item (recency) best. Middle items blur together.

**Implication for SaaS modules:** When you control item order — in a sidebar, a menu, a settings list, a dashboard — put the most-frequent items first (they're scanned in a hurry) and the rare-but-critical items last (they're remembered when needed, not hunted for in the middle).

---

## Where Serial Position applies in this module

For each of these surfaces in the module, decide the order:

### Surface 1 — Sidebar / module navigation

If this module has its own internal nav (Settings has sub-tabs; Reports has report-types; Customers has customer segments):

```markdown
#### Internal nav order

| Position | Item | Why here |
|---|---|---|
| 1 (first) | <most-used or default landing> | <reason> |
| 2 | | |
| ... | | |
| N (last) | <rare but critical — billing, plan, deletion> | <reason — recency favors rare-but-important here> |
```

### Surface 2 — Dashboard widget order

If the module has a dashboard:

```markdown
#### Dashboard widget order

| Position | Widget | Why here |
|---|---|---|
| 1 (top-left) | <most-scanned signal> | <reason> |
| ... | | |
| Last | <secondary or summary> | <reason> |
```

Reading order (top-left to bottom-right in LTR) is the de facto Serial Position for a grid. First-glance items go top-left; reference items go bottom-right.

### Surface 3 — Menu and dropdown order

For dropdown menus (toolbar menus, row context menus inside this module's tables):

```markdown
#### Menu order

| Position | Item | Why here |
|---|---|---|
| 1 | <most common action> | primacy |
| 2 | <second-most common> | |
| (middle) | <commonly grouped middle actions> | |
| (separator before destructive) | --- | visual break, per CONSTITUTION → Action Affordances |
| Last | <destructive action> | recency + visual isolation |
```

Destructive actions almost always go last with a separator above them. The separator + position reduces accidental click on the highest-cost action.

### Surface 4 — List default sort and primary item placement

If this module shows a list (Inbox, Customers, Invoices):

```markdown
#### Default sort

- **Sort field:** <date / priority / status / etc.>
- **Sort direction:** <ascending / descending>
- **Why this default:** <which user goal it serves>
- **First item under this sort:** <what's likely to be there — the most-relevant-on-arrival item>

#### Pinned / sticky items

- Some lists pin specific items at top (e.g., "starred" items, "today's tasks").
- Pinned items get primacy regardless of sort.
- Cap: ≤ 3 pinned items per Miller's Law.
```

### Surface 5 — Settings / configuration order

If this module has settings or configuration:

```markdown
#### Settings order

| Position | Section | Why here |
|---|---|---|
| 1 | Most-changed (e.g., profile, notifications) | primacy |
| ... | | |
| (later) | Rare-changed (e.g., advanced, integrations) | |
| Last | Destructive / account-level (delete account, leave workspace) | recency + isolation |
```

---

## What to write

`LAWS.md` → Serial Position section:

```markdown
## Serial Position Effect

### Internal navigation order

<per Surface 1 — only if applicable>

### Dashboard widget order

<per Surface 2 — only if applicable>

### Menu order patterns

<per Surface 3 — list each menu in the module>

### List default sort

<per Surface 4 — only if applicable>

### Settings order

<per Surface 5 — only if applicable>

### Cross-surface rules

- Destructive actions in any menu in this module: always last, separator above.
- Pinned items in any list in this module: cap at 3.
- First item in any list under default sort: must be the most-likely-relevant item, not arbitrary alphabetical.
```

---

## What NOT to do in phase 6

- **Don't alphabetize "for fairness."** Alphabetical order is the laziest defensible default. It serves no user goal. Use it only when the user genuinely doesn't have a frequency pattern (rare).
- **Don't put the destructive action in the middle.** Always last with separator.
- **Don't bury rare-but-critical at position 6 of 9.** That's the worst slot — neither remembered nor scanned. Move to last.
- **Don't pin more than 3 items.** Pinning everything = pinning nothing.

---

## Checkpoint

> "Three checks:
> 1. Walk through each navigable surface in this module. Does the first item earn its primacy? Does the last item earn its recency? If the middle has a 'critical' item, move it.
> 2. Are destructive actions in every menu in this module at last position with a separator? Consistency across menus is part of CONSTITUTION → Forbidden Patterns #9 (inconsistent destructive UI).
> 3. Does default sort serve the user's typical scan, or the database's typical order? They're rarely the same."

Iterate. Lock. Move to phase 7.

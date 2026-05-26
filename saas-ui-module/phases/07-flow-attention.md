# Phase 7 — Flow state & Selective Attention

**Purpose:** Apply two laws that govern user attention: Flow (sustained engagement; avoid breaking it with modals and interruptions) and Selective Attention (one thing wins per screen). These laws govern what NOT to add to the module.

**Produces:** `LAWS.md` → Flow and Selective Attention sections.

**Estimated time:** 10 minutes.

---

## The laws

**Flow (Csikszentmihalyi):** Users enter a focused state when working on tasks at the right challenge level with clear feedback. Once in flow, interruptions are expensive — the user loses minutes of context every time. Design for sustained flow; reserve interruption for genuinely required attention.

**Selective Attention:** Users focus on one foreground thing at a time. Everything else is peripheral. If two things compete for foreground, the user oscillates between them and loses both.

---

## Flow — the four checks

### Check 1 — Identify flow-state targets

Which flows in this module require sustained attention?

- **High-flow flows:** triage flows (Inbox), data-entry flows (form-heavy onboarding), investigation flows (search-and-drill into Audit Log), continuous flows.
- **Low-flow flows:** rare admin flows (changing billing), quick lookups (find customer record).

For each high-flow flow, write Flow-protection rules.

### Check 2 — Interruption budget

For each high-flow flow, list what's allowed to interrupt:

```markdown
#### <Flow name> — Interruption budget

**Allowed to interrupt:**
- Critical system events (data conflict, permission revoked mid-action)
- User-initiated actions (user clicks a "Notify me" button)

**NOT allowed to interrupt:**
- Marketing / upsell / "did you know..."
- Feature announcements / changelog popups
- Tour overlays / coachmarks (these are first-use only, not recurring)
- Surveys / NPS prompts
- "Are you sure?" for undoable actions (forbidden by CONSTITUTION → Forbidden Patterns #2)
- New-feature highlight pulses while user is in flow
- Notifications from other modules (queue them; show in a non-interrupting indicator)

**Decay:** any interruption that does fire should self-dismiss after a reasonable interval (5–10s for toasts; banners may persist but with a clear close).
```

### Check 3 — Modal discipline

Modals break flow. Per CONSTITUTION → Forbidden Patterns #13 (no modal stacking), they're already constrained. Tighten further at module level:

```markdown
#### <Flow name> — Modal usage

**Modals are allowed only when:**
- The action requires confirmation that genuinely cannot be undone (per CONSTITUTION → State Model → Recovery → Irreversible list)
- The action requires input that doesn't fit inline (rare — usually a drawer is better than a modal)

**Modals are NOT allowed for:**
- Forms that could fit as inline edit
- Confirmations of undoable actions (use undo toast)
- Multi-step flows (use a dedicated page or drawer with stepper)
- Filtering / configuration that should be inline
```

### Check 4 — Drawer vs page vs modal decision

For each composite-level surface in this module's flows:

| Surface | Use case |
|---|---|
| **Inline edit** | Single-field change; staying in list context |
| **Drawer (side panel)** | Multi-field record view/edit; user wants to keep list visible |
| **Page** | Complex flow with multiple sections; multi-step wizard; data entry of >7 fields |
| **Modal** | Irreversible confirmation only |

Document the choice per flow surface in this module so the pattern is consistent.

---

## Selective Attention — the three checks

### Check 1 — One winner per screen

For each primary screen in this module:

```markdown
#### <Screen name>

- **The ONE thing that wins:** <the user's primary task on this screen>
- **The visual signal:** <Von Restorff applied at composite level — primary button variant, color, weight>
- **Everything else:** quieter — should not compete visually with the winner
```

If you can't identify a single winner, the screen is doing too much. Either split into multiple screens or accept that the user's experience is fragmented.

### Check 2 — Notification placement

Notifications, alerts, system messages — where do they live in this module?

```markdown
#### Notification placement

- **In-line at top:** for screen-relevant warnings (e.g., "Your plan is expiring in 7 days" on the billing screen)
- **Toast (corner):** for action acknowledgments (CONSTITUTION → Forbidden Patterns #6 — toasts reserved for: actions whose source has left viewport, long-delayed successes, undo affordances)
- **Persistent banner (above content):** for ongoing system state (e.g., "You're in read-only mode because of a permission change")
- **Notification center (inbox-style):** for queued, low-urgency notifications that shouldn't interrupt
```

### Check 3 — Visual hierarchy enforcement

Per CONSTITUTION → Density → Visual weight order:

```text
1. Primary action
2. Active selection
3. Anomaly / urgent indicator
4. Hover / focus
5. Everything else
```

For each screen, verify only one thing claims rank 1 at a time. If a screen has two "primary" buttons (Save Draft + Submit), one must be demoted (Save Draft → secondary).

---

## What NOT to do in phase 7

- **Don't add tours and coachmarks for recurring users.** They break flow. Tours are first-use only.
- **Don't push notifications mid-action.** Even important system messages can wait 5 seconds for the user to complete their click.
- **Don't have multiple primary CTAs on a screen.** Pick one; demote the rest.
- **Don't use modals where drawers would work.** Drawers preserve context; modals erase it.
- **Don't insert "did you know" / "try this new feature" content into flow surfaces.** That belongs in release notes, the changelog, or an opt-in tour — never as an in-flow surprise.

---

## What to write

`LAWS.md` → Flow and Selective Attention sections:

```markdown
## Flow

### Flow-state targets

| Flow | Flow-state? | Why |
|---|---|---|
| <flow> | yes | <reason> |
| <flow> | no | <reason — rare, single-action, etc.> |

### Per-flow interruption budgets

#### <Flow 1>
<per Check 2>

### Modal discipline

#### <Flow 1>
<per Check 3>

### Drawer vs page vs modal pattern

<per Check 4 — cross-flow defaults for this module>

---

## Selective Attention

### Per-screen winner

| Screen | The ONE winner | Visual signal |
|---|---|---|
| <screen> | <task> | <signal> |

### Notification placement strategy

| Type | Surface | When |
|---|---|---|
| Action ack | Toast | Source out of view |
| Persistent state | Banner | Ongoing condition |
| Queued info | Notification center | Low urgency |

### Visual hierarchy enforcement

- Every primary screen in this module has exactly ONE element at rank 1 (primary action).
- Violations require argument: <list any>
```

---

## Checkpoint

> "Three checks:
> 1. Walk through a typical 10-minute session in this module. How many times is the user interrupted (modals, banners, notifications, tours)? If the count > 1 for an anchor module, you're breaking flow.
> 2. For each primary screen — squint at it mentally. Does one element clearly win? If two compete, the user will oscillate.
> 3. Is the drawer/page/modal pattern consistent? Mixing them for similar use cases (sometimes a drawer for record edit, sometimes a modal) is a sign of unprincipled choices."

Iterate. Lock. Move to phase 8.

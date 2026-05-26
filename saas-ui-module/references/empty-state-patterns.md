# Empty-state patterns

> Empty states are where Peak-End starts (for first-time users, the true-empty IS the first impression). They're also where LLM-generated UI most consistently ships generic, useless content.

The rule: every empty state names what's absent, why, and the single recommended action.

---

## The four canonical contexts

Recap from phase 8:

1. **True-empty** — no data ever existed
2. **Filter-empty** — data exists; filter excludes all
3. **Error (recoverable)** — operation failed; user can retry
4. **Error (terminal)** — permission denied / 404 / deleted

Plus loading states (long-load + long-mutation).

---

## Pattern A — True-empty for an anchor module

The user lands here daily. The first time they see it, the empty state shapes their mental model.

**Structure:**
- Header (3–6 words): names what's absent
- Body (1–2 sentences): why + what they can do
- Primary action: verb-first imperative, the recovery
- Optional secondary action: only if a genuine alternative path exists
- Illustration: yes — anchor modules earn the polish budget

**Example (Inbox module, true-empty):**
- Header: "Inbox zero."
- Body: "You're all caught up. New items will appear here when teammates message you or systems send updates."
- Primary action: none (this isn't a recovery moment — it's a *good* empty state)
- Illustration: yes — quiet, on-brand

**Example (Customers module, true-empty):**
- Header: "No customers yet."
- Body: "Add your first customer to start tracking interactions and revenue."
- Primary action: "Add customer"
- Secondary: "Import from CSV"
- Illustration: yes

---

## Pattern B — True-empty for an occasional or rare module

The user reaches this rarely. The empty state should educate without lecturing.

**Structure:**
- Header (3–6 words)
- Body (1–2 sentences) — includes brief context since user has forgotten
- Primary action
- Illustration: optional — usually omit for rare modules

**Example (Audit Log module, true-empty):**
- Header: "No audit events yet."
- Body: "Audit events appear here when team members create, edit, or delete records. They're retained for 90 days."
- Primary action: "View activity dashboard" (alternative path) or none
- Illustration: no — rare modules typically skip illustrations

---

## Pattern C — Filter-empty (always functional)

Filter-empty is *never* a peak moment. It's a frustration: the user filtered too aggressively.

**Structure:**
- Header: "No <items> match these filters."
- Body: "Adjust filters or clear them to see all <total> <items>."
- Primary action: "Clear filters"
- Illustration: absent

**Anti-pattern:** "Sorry, we couldn't find anything matching your filters! 😢" — apology + emoji + filler. Forbidden by CONSTITUTION → Copy.

---

## Pattern D — Error (recoverable), inline

Field-level or section-level errors. The user can fix and retry.

**Structure (inline below field):**
- Single sentence: [What failed] · [Why] · [Recovery]
- No icon needed if the field has visual error treatment
- Color + icon + weight (per Forbidden Pattern #8 — color-only signaling)

**Examples:**
- "Email format isn't recognized: must include @ and a domain. Example: name@company.com"
- "Amount can't exceed $10,000 without admin approval. Reduce the amount or request approval."

**Anti-pattern:** "Invalid email." → no recovery, no example.

---

## Pattern E — Error (recoverable), banner

Section-level or page-level errors. Affects more than one field.

**Structure:**
- Header in red/error variant
- Body: [What failed] · [Why] · [Recovery action with button]
- Persistent until user acts or refreshes
- One banner at a time (no stacking)

**Example:**
- Header: "Couldn't load customer details."
- Body: "Connection to the server failed. Your changes are saved here until you reconnect."
- Action: "Retry connection"

---

## Pattern F — Error (terminal), full-page

The user can't recover here — they need to navigate.

**Structure:**
- Header: names the specific failure
- Body: context + suggested next action
- Action: navigation (not retry)
- Illustration: optional — usually a quiet "missing" illustration; never error-themed (no broken icons, sad faces)

**Examples:**

404 in invoices:
- Header: "This invoice doesn't exist."
- Body: "It may have been deleted, or the link is wrong."
- Action: "Back to invoices"

Permission denied:
- Header: "You don't have access to this customer."
- Body: "Account managers and admins can view customer records. Ask <admin name> if you need access."
- Action: "Back to dashboard"

Deleted record:
- Header: "This invoice was deleted by <user> on <date>."
- Body: "Deleted invoices are kept for 30 days in case you need to restore them."
- Action: "Restore" (if user has permission) | "Back to invoices"

---

## Pattern G — Long-load affordance

Appears at 5s when content is still loading.

**Structure (inline at top of loading area):**
- Quiet single line: "Taking longer than usual."
- Optional: name the slow operation if known ("Generating the report")
- Optional: "Cancel" button (if the operation is cancellable)

**Anti-pattern:** Spinning circle that never updates. Per CONSTITUTION → State Model → Loading strategy, 5s is the threshold.

---

## Pattern H — Long-mutation in-progress

Long-running mutations (imports, exports, reports).

**Structure:**
- Header: progress verb + subject ("Importing customers")
- Progress indicator: numeric (812 of 1,247) or percent
- "Cancel" button if cancellable
- Sub-text: what to expect ("This takes about 30 seconds for files under 10,000 rows")
- On completion: success copy + link to artifact

---

## Cross-pattern rules

For all empty/error/loading copy:

- **Voice:** direct + warm (CONSTITUTION → Copy → Voice). Never apologetic. Never gamified.
- **Specificity:** name the thing — "No invoices" not "No data." "Email format isn't recognized" not "Invalid input."
- **Recovery:** every recoverable state names exactly one recovery action. Multiple actions = decision paralysis.
- **Pluralization:** singular for 1, plural for others. "1 row" / "N rows". Domain-specific irregulars in EMPTY-STATES.md → Pluralization notes.
- **Color + icon + weight:** error states must use at least two cues from these three. Color-only is forbidden.
- **No exclamation marks** unless the brand voice in CONSTITUTION → Copy → Voice explicitly allows them (rare).

---

## Common LLM failures cataloged

1. "**No data**" — generic. Always.
2. "**Oops!**" — forbidden phrasing.
3. "**Something went wrong**" — forbidden phrasing. Name the failure.
4. **Multiple primary CTAs in empty states** — pick ONE recovery.
5. **Decorative illustrations for filter-empty** — wastes polish budget; this isn't a peak moment.
6. **"Sorry, ..." preambles** — apologetic voice forbidden.
7. **Sad/broken-themed error illustrations** — set the wrong emotional tone; users feel worse.
8. **Same empty-state copy across true-empty and filter-empty** — they're different states with different recoveries.
9. **No recovery action** — every recoverable error names recovery.
10. **Recovery action that doesn't actually recover** — "Try again" on a permission error fails again. Be honest.

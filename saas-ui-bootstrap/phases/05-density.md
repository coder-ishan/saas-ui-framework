# Phase 5 — Density

**Purpose:** Establish the information-density posture — grid, spacing scale, padding rules, type rhythm. Density is the structural decision that determines whether the product feels cramped, breathy, or balanced. Enterprise SaaS is dense by nature; the question is how disciplined the density is.

**Produces:** `.saas-ui/CONSTITUTION.md` (Density section) + `.saas-ui/PRINCIPLES.md` (Aesthetic-Usability section finalized)

**Estimated time:** 10–15 min

## Interview prompts

### 1. Grid system
> What's your base spacing unit? Typical SaaS uses 4px or 8px.
> - If your design system uses Tailwind, this is your spacing scale (already in INVENTORY.md).
> - If custom, state it explicitly.
>
> What scale? (e.g., 0, 1, 2, 3, 4, 6, 8, 12, 16, 24, 32, 48, 64 × base unit)

Don't proceed without nailing this. Inconsistent spacing is the single biggest source of "feels unpolished."

### 2. Density posture
> Three options:
> - **Comfortable** — spacious, breathing room, fewer items visible per viewport. Examples: Notion, Coda. Suits creative/document apps.
> - **Balanced** — medium density, considered whitespace. Examples: Linear, Stripe. Suits most enterprise SaaS.
> - **Dense** — maximum info, minimal whitespace. Examples: Bloomberg terminal, Excel, Airtable's compact view. Suits power-user data apps.
>
> Pick one. We'll use this as the default; individual modules can lean denser or roomier per polish budget.

### 3. Padding scale
For each container type, set a rule:
> - **Page container padding:** outer padding from viewport edge (e.g., 24px / 32px / 48px)
> - **Card padding:** internal padding of card-like surfaces (e.g., 16px / 20px / 24px)
> - **Dialog padding:** modal/sheet internal padding (e.g., 24px / 32px)
> - **Form group spacing:** gap between form fields (e.g., 16px / 20px / 24px)
> - **List item padding:** internal padding of list rows (e.g., 8px-vert / 12px-horiz)
> - **Section spacing:** gap between major sections within a view (e.g., 32px / 48px / 64px)

These become the canonical spacing rules. Composites SPECs reference them by name.

### 4. Typography rhythm
> What's your type scale? Names and sizes. Examples:
> - `display`: 32-48px (rare, only hero)
> - `h1`: 24-28px
> - `h2`: 20-22px
> - `h3`: 16-18px
> - `body`: 14-15px (most dense SaaS uses 14)
> - `sm`: 12-13px (metadata, captions, labels)
> - `xs`: 11px (use sparingly; accessibility floor)
>
> What's your line-height rule? (1.4-1.5 for body, 1.2-1.3 for headings, common)
> What's your letter-spacing rule? (usually 0, slight negative on display)

### 5. Hierarchy without dividers
> When you have nested content (a card with sections, a row with sub-rows), what creates hierarchy?
> - Whitespace alone (cleanest, hardest to read at high density)
> - Subtle dividers (lines)
> - Background tints (zebra, region grouping)
> - All three at different levels
>
> Most balanced enterprise SaaS uses *whitespace primary, dividers secondary, tints rarely*. Confirm or override.

### 6. Visual weight rules
> What gets *visual weight*? In order of priority:
> - Primary action (button, link)
> - Active selection
> - Anomaly / urgent indicator
> - Hover/focus
> - Everything else
>
> The Von Restorff Effect says: only one thing per screen should be the most prominent. Confirm there's a clear primacy order.

### 7. Polish floor (finalize Aesthetic-Usability)
Reference back to phase 1 polish floor question.
> Now that we have spacing and type set, here's the polish floor for this product:
> - Every screen has 8px-aligned spacing (no random 11px padding, no 17px gaps)
> - Every text uses a defined type scale value (no `font-size: 13.5px`)
> - Every interactive element has hover + focus + active states defined
> - Every spacing decision can be traced to the scale; ad-hoc values must be justified
>
> Confirm this floor or adjust.

## What to write

### CONSTITUTION.md — Density section

```markdown
## Density

**Posture:** comfortable | balanced | dense
  Reason: <user's rationale>

**Grid:**
- Base unit: <4px | 8px>
- Scale: <full scale, e.g., 0, 4, 8, 12, 16, 20, 24, 32, 40, 48, 64>

**Padding rules:**
- Page outer: <px>
- Card: <px>
- Dialog: <px>
- Form group spacing: <px>
- List item: <vert>px / <horiz>px
- Section: <px>

**Typography:**
- Scale: <name: px, name: px, ...>
- Line-height: body <1.x>, headings <1.x>
- Letter-spacing: <values>

**Hierarchy mechanism:**
- Primary: <whitespace | dividers | tints>
- Secondary: <whitespace | dividers | tints>
- Tertiary: <whitespace | dividers | tints | "not used">

**Visual weight order (Von Restorff):**
1. Primary action
2. Active selection
3. Anomaly / urgent indicator
4. Hover/focus
5. Everything else
Reason: <user's rationale, especially if order is reordered>

**Polish floor (non-negotiable):**
- All spacing is multiples of <base unit>; ad-hoc values forbidden
- All text uses scale values; ad-hoc font-sizes forbidden
- Every interactive element has defined hover, focus, active states
- <any user-added rules>
```

### PRINCIPLES.md — Aesthetic-Usability section (finalize)

```markdown
## 6. Aesthetic-Usability Effect

**Polish floor for this product:**
- Spacing rhythm: <restated from constitution>
- Typography rhythm: <restated>
- Color usage discipline: <e.g., "color used only for: status, primary action, anomaly. No decorative color.">
- Alignment: <e.g., "all column edges align to the grid; nested content aligns to parent's inner edge">

**Minimum standard:** below this, no screen ships.
```

## Success criteria

Phase 5 is complete when:
1. CONSTITUTION.md Density section is fully populated.
2. PRINCIPLES.md Aesthetic-Usability section is finalized.
3. The polish floor is explicit and non-negotiable.
4. STATE.md updated: `phase_5_complete: true`, `current_phase: 6`.

## What NOT to do

- Don't proceed without a defined spacing scale. Vague density kills polish.
- Don't accept "11px feels right somewhere" — every value comes from the scale.
- Don't allow visual weight to be assigned to >1 "primary" thing per screen. Force the ordering.
- Don't write density rules without `Reason:` lines.

## Checkpoint

When phase 5 completes, ask: "Density rules set. Continue to phase 6 (copy), or pause here?"

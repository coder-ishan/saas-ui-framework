# Canonical flow patterns

> Quick reference. Each flow in `FLOWS.md` is classified as one of these patterns. The pattern governs progress-indicator choice, persistence strategy, and per-flow checks.

---

## 1. Linear flow

**Shape:** A → B → C → D → success.

One path. No decision points. The user advances step-by-step toward a known outcome.

**Examples:**
- Invoice creation wizard (4 steps + review)
- Account onboarding (welcome → profile → workspace → invite)
- Refund flow (verify → reason → amount → confirm)

**Module-level concerns:**
- **Goal-Gradient:** progress indicator visible at every step
- **Final-step discipline:** primary affordance is the outcome verb, not "Next"
- **Back is always available:** explicit back, not just browser back

**Common LLM failures:**
- Step 5 of 5 introduces 8 new required fields (acceleration rug-pulled)
- No back from step 3 → step 2 (user trapped)
- "Next" button instead of "Submit invoice" on final step (vague)

---

## 2. Branching flow

**Shape:** A → decision → B-path | C-path | D-path → success-A | success-C | success-D.

Decision points split the path. The user picks among visible alternatives.

**Examples:**
- Permission grant (allow read | allow read+write | deny | customize)
- Refund processing (full refund | partial refund | decline)
- Onboarding industry split (tech | finance | healthcare | other — different downstream questions)

**Module-level concerns:**
- **All branches visible at decision point** — no progressive reveal that hides options
- **Recommended choice is visually primary** (Von Restorff at composite level applied to the decision)
- **Cancel/back always available**

**Common LLM failures:**
- Decisions disguised as wizards — "Continue" → silently picks the default for the user
- Default branch hidden until user clicks "More options"
- Cancel/back missing at decision points (user trapped in a branch)

---

## 3. Save-and-resume flow

**Shape:** A → B → [pause] → ... → B (resumed) → C → success.

The user can leave mid-flow and return without losing work. Multi-day flows, long forms, support tickets.

**Examples:**
- Filing a support ticket (with file uploads)
- Tax return wizard (multi-session)
- Large data import flow (with mapping + validation)

**Module-level concerns:**
- **Persistence contract is explicit:** what's saved, when, where
- **Resume entry point is durable:** URL or notification, not "the page you were on"
- **Resume UI drops user at the right step:** not step 1
- **Stale-resume handling:** if the world changed (entity deleted, validation rules updated), surface inline, don't fail

**Common LLM failures:**
- Auto-save without telling the user (no sync indicator)
- Resume forces user to re-traverse completed steps
- Drafts expire silently after 30 days

---

## 4. Single-action flow

**Shape:** Single click/keystroke → success.

One-off actions where the goal is reached in one interaction. Often have keyboard primary affordances.

**Examples:**
- Mark inbox item read
- Star a record
- Toggle a status
- Approve a request

**Module-level concerns:**
- **Keyboard primary affordance:** most single-action flows are best as shortcuts
- **Optimistic UI:** single actions should feel instant (per CONSTITUTION → State Model)
- **Undo affordance:** for destructive single actions (5s undo toast)

**Common LLM failures:**
- Confirmation modal for undoable single actions (Forbidden Pattern #2)
- No keyboard shortcut (mouse-only)
- No undo affordance for destructive single actions

---

## 5. Continuous flow

**Shape:** Same action repeated many times in one session.

The user is in the module doing the same kind of thing over and over. Triaging, reviewing, classifying.

**Examples:**
- Triaging inbox (read, archive, snooze, reply, repeat)
- Reviewing a queue of items (approve / reject each)
- Tagging photos for an album

**Module-level concerns:**
- **Fatigue management:** no captcha, no friction per action
- **Batch actions when patterns emerge:** "Selected 12 items: Archive all?"
- **Natural rest points:** no infinite scroll without scroll-position memory
- **Selective Attention:** the queue is the one winner; everything else (sidebar, banners) quiets

**Common LLM failures:**
- Confirmation dialog on every action (cumulative fatigue)
- No bulk actions when the user is obviously batching
- Tour overlays / new-feature pulses that interrupt the queue

---

## Pattern selection cheat

| User scenario | Likely pattern |
|---|---|
| Step-by-step creation | Linear |
| Multiple distinct outcomes from one decision | Branching |
| Long form that might cross sessions | Save-and-resume |
| Toggle, mark, approve in one click | Single-action |
| Same action many times in a session | Continuous |

A flow that doesn't fit any of these is suspicious — usually it's two flows glued together. Split it.

---

## Cross-pattern rules

Regardless of pattern:

- **Pre-conditions** are documented (what must be true to start)
- **Error exits per step** are documented (not just "generic error handling")
- **Keyboard path** is documented (or N/A explained for genuinely mouse-only)
- **Success exit copy** follows CONSTITUTION → Copy → Confirmation structure
- **Abandon exit** is graceful (no trap)

# Phase 3 — Manual-review surfacing

**Purpose:** Surface checks that require subjective judgment. The audit can't auto-pass/fail these — it asks the user (or a human reviewer) to make the call.

**Produces:** REVIEW-REQUIRED items added to the findings list.

---

## What goes here

Some rules can't be checked programmatically without strong false-positive risk:

- **Peak-End — is the peak's polish investment proportional to the dominant flow's importance?** (Subjective.)
- **Prägnanz — is the simplest interpretation of the composite the true one?** (Subjective.)
- **Voice — does empty-state copy feel "direct + warm" per CONSTITUTION → Copy → Voice?** (Subjective.)
- **Decision affordance — is the recommended choice in a branching flow visually primary enough?** (Subjective.)
- **Common Region / Uniform Connectedness (Gestalt) — is the visual grouping clear without conscious thought?** (Subjective — needs a real visual check.)

The audit surfaces these in the scorecard's "Review required" section. The user (or reviewer) marks each as PASS / FAIL / ARGUED-DEVIATION after review.

---

## Per-check surfacing

For each manual check:

```markdown
### [REVIEW-NEEDED] <Check name>

**Category:** <constitution / principle / composite-law / module-law>
**Rule:** <source rule>
**Question for reviewer:** <one specific question to answer>
**Where to look:** <which files / sections to read>
**Pass criterion:** <what "good" looks like>
```

---

## The 8 canonical manual checks

### 1. Peak-End polish proportionality

For each module: review `modules/<name>/LAWS.md` → Peak-End.

**Question:** "Does the polish investment at the peak match the dominant flow's importance in this module?"

**Where to look:** the peak moment description + the implementation (if code mode).

**Pass criterion:** an outside reviewer reading the LAWS.md entry agrees the peak is "the moment the user feels success."

### 2. Prägnanz (simplest interpretation)

For each composite: review SPEC.md → Anatomy.

**Question:** "Does the composite read as one coherent thing, or does it require constructing a multi-part mental model?"

**Pass criterion:** describable in one sentence without "and."

### 3. Voice consistency

For each module: review `modules/<name>/EMPTY-STATES.md` copy + CONSTITUTION → Copy → Voice.

**Question:** "Do these strings sound like the same product writing them?"

**Pass criterion:** read aloud, the voice is consistent across true-empty, filter-empty, error-recoverable, error-terminal.

### 4. Decision affordance visual hierarchy

For branching flows: review FLOWS.md → flow specs + composite SPEC's Action Inventory.

**Question:** "Is the recommended choice the visually primary affordance at the decision point?"

**Pass criterion:** in a screenshot, the eye lands on the recommended choice first.

### 5. Gestalt: Common Region

For each composite: review SPEC.md → Anatomy + the implementation.

**Question:** "Do the bounded regions in the composite correspond to functional groupings?"

**Pass criterion:** removing the borders/shadows would lose meaningful grouping signal.

### 6. Gestalt: Proximity

**Question:** "Is within-cluster spacing measurably less than between-cluster spacing?"

**Pass criterion:** check spacing values in code; within < between.

### 7. Gestalt: Uniform Connectedness

**Question:** "When the row context menu trigger appears on hover, does it visually attach to the row (not float over it)?"

**Pass criterion:** the trigger inherits row background or is otherwise visually unified with the row.

### 8. Posture appropriateness

**Question:** "Does the current posture match what the team is actually doing?"

**Where to look:** EXECUTION-POSTURE.md → re-evaluation trigger; recent commit history; team's stated next milestone.

**Pass criterion:** the committed posture matches the team's actual pace and quality target.

If FAIL: recommend `/saas-ui-bootstrap` phase 10 to re-commit.

---

## How manual checks integrate into the scorecard

Manual checks appear under their own section:

```markdown
## Review required

These checks require human judgment. Mark each as PASS / FAIL / ARGUED-DEVIATION after review.

| # | Check | Question | Status (fill in) |
|---|---|---|---|
| 1 | Peak-End polish | ... | [ ] |
| 2 | Prägnanz | ... | [ ] |
| ... | | | |
```

The scorecard ships with `[ ]` placeholders. A subsequent audit run with `--review-completed <date>.md` can take the user-filled review back and produce a final scored audit.

---

## What NOT to do in phase 3

- **Don't try to programmatically resolve subjective checks.** False positives erode trust in the audit; better to surface for review.
- **Don't surface too many manual checks.** Cap at 8 per audit run. If more apply, prioritize by category (functional > constitution > law > pathology).
- **Don't write fail conclusions.** Manual checks output REVIEW-NEEDED, never FAIL or PASS — those come from the reviewer's response.

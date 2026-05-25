# Contributing to saas-ui-framework

Thanks for considering a contribution. This framework is small but opinionated; please read this before opening a PR.

---

## What we want

1. **New pathologies in `common-llm-ui-mistakes.md`** — if you've observed an LLM ship a UI failure mode that isn't on the list of 17, file an issue with: example output (sanitized), why it's a pathology, and a proposed prevention rule that's enforceable by audit.
2. **Phase refinements** — questions that don't elicit useful answers; checkpoint dialogs that confuse; success criteria that are too lax or too strict. PRs welcome with the before/after.
3. **Reference docs** — annotated screenshots from real products that exemplify a CONSTITUTION rule. Include a one-line attribution and a one-line annotation of *what* to look at.
4. **Battle-testing reports** — file an issue describing what you used the framework for, what worked, what didn't. These shape the roadmap more than feature requests.

---

## What we DON'T want

1. **Stack ports without discussion.** Forking the framework for Remix, SvelteKit, Astro, or anything else is a big choice. Open an issue first — the right answer might be a separate repo rather than parallel branches here.
2. **Generic "best practices" PRs.** Adding "follow accessibility guidelines" as a CONSTITUTION rule is not actionable. Rules must be specific enough that `saas-ui-audit` can check them.
3. **Rules without `Reason:` lines.** Every rule in CONSTITUTION carries the *why*. Edge cases are judged against the reason, not the letter.
4. **Removing the inventory-as-constraint mechanism.** This is load-bearing. It's the only thing preventing atom invention.

---

## How to add a new pathology

1. File an issue first to discuss whether it's a *pathology* (something that ships repeatedly) vs an *edge case* (something that happens sometimes).
2. If accepted, the PR adds:
   - An entry in `saas-ui-bootstrap/references/common-llm-ui-mistakes.md` with: Pathology description, Why LLMs ship this, Prevention rules across CONSTITUTION / SPEC / Audit layers.
   - A corresponding rule in `saas-ui-bootstrap/templates/CONSTITUTION.md` → Forbidden Patterns section.
   - (Eventually) an audit check in `saas-ui-audit` once that skill ships.
3. The pathology number is the next available integer. Don't renumber existing entries.

---

## How to add a new phase to an existing skill

1. Open an issue first. Phase changes have downstream effects across templates and references.
2. The PR adds:
   - A new file in `<skill>/phases/<NN>-<slug>.md` following the existing phase structure (Purpose, Produces, Estimated time, Interview prompts, What to write, Success criteria, What NOT to do, Checkpoint).
   - An entry in the skill's `SKILL.md` phase table.
   - If the phase produces a new artifact, a corresponding template in `<skill>/templates/`.

---

## How to add a new skill

1. Open an issue. The framework's coherence depends on the skill set staying small (5 planned). Adding a 6th needs argument.
2. If accepted, follow the structure of `saas-ui-composite/`:
   - `SKILL.md` (orchestrator) with frontmatter `name` + `description`. Description must describe WHEN to invoke, not WHAT it does. See Anthropic's skill-writing guidance.
   - `phases/` directory with one file per phase.
   - `templates/` directory with one template per artifact produced.
   - `references/` directory with cheat-sheets and catalogs.

---

## Style

- **Prose, not code.** Skill files are read by both humans and LLMs. Pseudocode allowed in state machines only.
- **Tables for enumerations.** Rules, atoms, states, shortcuts → tables.
- **Reasons for rules.** Every rule has a `Reason:` line. No bare rules.
- **Cross-reference, don't restate.** If something is in CONSTITUTION, reference it. Restating creates drift.
- **Markdown only.** No HTML, no MDX, no embedded images (link out instead).

---

## Commit conventions

- Atomic commits per logical change
- Conventional-ish prefix: `add:`, `fix:`, `update:`, `docs:`, `refactor:`
- Subject ≤ 70 chars
- Body explains WHY when not obvious from the diff

---

## Testing a skill change

Before opening a PR:

1. Copy the modified skill into `~/.claude/skills/`
2. Run it on a real (or convincing dummy) SaaS project
3. Verify each phase produces sensible output
4. Document any rough edges in the PR description

There is no automated test suite (yet). The skills are prose; the test is whether they produce good artifacts in real use.

---

## License

By contributing, you agree your contributions are licensed under the MIT License, the same as the rest of the project.

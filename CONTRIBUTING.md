# Contributing to automation-qa-skills

Thanks for helping build a high-quality, community-driven QA skills library!
Every skill should be focused, reusable, and safe to drop into any AI coding agent
(Claude Code, Cursor, Codex, etc.).

## Golden rules

1. **One responsibility per skill.** Prefer several focused skills over one giant skill.
2. **Every skill ships with examples.** Real input → real output.
3. **Be tool-agnostic where possible.** Name concrete tools only when the skill is about that tool (e.g. `selenium-java`).
4. **Never commit secrets or real customer data.** Use synthetic data (`test-data-generator`).

## Adding a new skill

1. Copy the template:
   ```bash
   cp templates/SKILL_TEMPLATE.md skills/<your-skill>/SKILL.md
   mkdir -p skills/<your-skill>/examples
   ```
2. Fill in the frontmatter (`name`, `description`) and every section.
3. Add at least one runnable example under `skills/<your-skill>/examples/`.
4. Validate your Markdown (headings, code fences, links).
5. Add your skill to the **Available Skills** table in `README.md`.
6. Add a `CHANGELOG.md` entry under **Unreleased**.

## SKILL.md required sections

- Frontmatter: `name`, `description`
- **Purpose** — what the skill produces
- **When to use / When not to use**
- **Inputs** — what the agent needs from the user
- **Workflow** — numbered, deterministic steps
- **Output format** — the exact structure to emit
- **Quality checklist** — self-review gate before finishing

## Pull request checklist

- [ ] New skill folder contains `SKILL.md` + `examples/`
- [ ] Frontmatter present and valid
- [ ] Example input/output included
- [ ] README skills table updated
- [ ] CHANGELOG updated
- [ ] No secrets, credentials, or real PII committed

## Versioning

We follow [Semantic Versioning](https://semver.org/):
- **MAJOR** — breaking changes to skill contracts.
- **MINOR** — new skills or backward-compatible improvements.
- **PATCH** — fixes and clarifications.


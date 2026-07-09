# Getting Started

`automation-qa-skills` is a library of focused, reusable **QA skills** for AI coding agents
(Claude Code, Cursor, Codex, and others). Each skill is a self-contained `SKILL.md` that tells
the agent how to perform one QA task well.

## Anatomy of a skill

```
skills/<skill-name>/
├── SKILL.md      # frontmatter (name, description) + Purpose, Workflow, Output, Checklist
└── examples/     # real input → output samples
```

## Using a skill with an AI agent

1. Point your agent at the repo (or copy the `SKILL.md` into your agent's skills directory).
2. Reference the skill by name, e.g. *"Use the `test-case-generator` skill for this story."*
3. The agent follows the skill's workflow and emits output in the documented format.
4. Review against the skill's **Quality checklist** before you rely on the result.

### Claude Code

Copy a skill folder into `~/.claude/skills/<skill-name>/` (user-level) or
`.claude/skills/<skill-name>/` (project-level), then invoke it by name.

## Composing skills

Skills are designed to chain:

- `requirement-analysis` → `test-case-generator` → `ai-test-generator` → `playwright`
- `jira-bug-writer` → `root-cause-analysis` → `bug-triage`
- `test-plan` → execute (`smoke` / `regression`) → `qa-documentation` → `deployment-guide`

## Contributing

See [`CONTRIBUTING.md`](../CONTRIBUTING.md) and start from
[`templates/SKILL_TEMPLATE.md`](../templates/SKILL_TEMPLATE.md).

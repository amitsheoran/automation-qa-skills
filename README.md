# automation-qa-skills

**Production-ready QA Skills for Claude Code, Cursor, Codex and AI coding agents.**

A curated, open-source library of focused, reusable **QA skills**. Each skill is a
self-contained `SKILL.md` that teaches an AI coding agent how to perform one QA task
well — from writing test cases and Jira bugs to automating Selenium/Playwright, API,
performance, accessibility, and security testing.

> Design principle: **one responsibility per skill**, every skill ships with a workflow,
> a strict output format, and a self-review checklist.

---

## Available Skills

### Manual & QA process
| Skill | What it does |
|-------|--------------|
| [test-case-generator](skills/test-case-generator/SKILL.md) | Enterprise-grade positive/negative/boundary/edge test cases from a requirement |
| [requirement-analysis](skills/requirement-analysis/SKILL.md) | Check requirements for testability, ambiguity, and missing criteria |
| [test-plan](skills/test-plan/SKILL.md) | IEEE-829-style plan: scope, approach, entry/exit criteria, risks |
| [test-strategy](skills/test-strategy/SKILL.md) | Long-lived team strategy: pyramid, automation, quality gates, metrics |
| [exploratory-testing](skills/exploratory-testing/SKILL.md) | Charter- and heuristic-driven session-based exploration |
| [jira-bug-writer](skills/jira-bug-writer/SKILL.md) | Clear, reproducible, triage-ready bug reports |
| [root-cause-analysis](skills/root-cause-analysis/SKILL.md) | 5-Whys, evidence-based root cause + prevention |
| [bug-triage](skills/bug-triage/SKILL.md) | Prioritize/route a backlog with severity, priority, action |
| [qa-documentation](skills/qa-documentation/SKILL.md) | Summary reports, sign-offs, runbooks, KB articles |

### Test levels & types
| Skill | What it does |
|-------|--------------|
| [smoke-testing](skills/smoke-testing/SKILL.md) | Fast build-verification suite of critical paths |
| [sanity-testing](skills/sanity-testing/SKILL.md) | Focused check that a fix works and nearby areas hold |
| [regression-testing](skills/regression-testing/SKILL.md) | Risk-based regression selection via impact analysis |

### Automation frameworks
| Skill | What it does |
|-------|--------------|
| [selenium-java](skills/selenium-java/SKILL.md) | Page Object Model, explicit waits, TestNG/JUnit |
| [playwright](skills/playwright/SKILL.md) | Role-based locators, web-first assertions, fixtures |
| [cypress](skills/cypress/SKILL.md) | `data-cy` selectors, custom commands, `cy.intercept` |
| [robot-framework](skills/robot-framework/SKILL.md) | Keyword-driven suites with resource files |
| [appium](skills/appium/SKILL.md) | Cross-platform mobile (Android/iOS) automation |

### Visual & mobile web
| Skill | What it does |
|-------|--------------|
| [visual-regression-testing](skills/visual-regression-testing/SKILL.md) 🆕 | Screenshot-diff UI regressions against a reviewed baseline |
| [mobile-web-testing](skills/mobile-web-testing/SKILL.md) 🆕 | Responsive/touch testing of web apps on mobile browsers |

### API & data
| Skill | What it does |
|-------|--------------|
| [api-testing](skills/api-testing/SKILL.md) | REST/GraphQL: status, schema, auth, negative, integrity |
| [sql-testing](skills/sql-testing/SKILL.md) | Data integrity, constraints, reconciliation, migrations |
| [contract-testing](skills/contract-testing/SKILL.md) 💡 | Consumer-driven contracts (Pact) for microservices |
| [test-data-generator](skills/test-data-generator/SKILL.md) 💡 | PII-safe synthetic valid/invalid/boundary data |

### Non-functional
| Skill | What it does |
|-------|--------------|
| [performance-testing](skills/performance-testing/SKILL.md) | Load/stress/spike/soak with SLOs (k6/JMeter/Gatling examples) |
| [accessibility-testing](skills/accessibility-testing/SKILL.md) | WCAG 2.2 AA: axe + keyboard/screen-reader/contrast |
| [security-testing](skills/security-testing/SKILL.md) | Authorized, defensive OWASP Top 10 testing |

### Delivery & AI
| Skill | What it does |
|-------|--------------|
| [deployment-guide](skills/deployment-guide/SKILL.md) | Release runbook: pre-deploy, verify, monitor, rollback |
| [ai-test-generator](skills/ai-test-generator/SKILL.md) | Generate tests from code/spec with a hardening review |

💡 = **Suggested additions** beyond the original spec (see below). 🆕 = **Added in v2.0**
(visual regression + mobile web).

---

## Suggested additions

Beyond the requested skill set, two extra skills were added because they close common gaps
in modern QA:

- **[contract-testing](skills/contract-testing/SKILL.md)** — as teams move to microservices,
  consumer-driven contract tests (Pact) catch breaking API changes *before* deploy, far faster
  and less flaky than full end-to-end integration runs.
- **[test-data-generator](skills/test-data-generator/SKILL.md)** — most weak or flaky tests
  come from weak data. This generates realistic, boundary-aware, **PII-safe** synthetic data
  for every other testing skill.

---

## Installation / Usage

Each skill is a plain `SKILL.md`, so any AI agent can use it.

**Claude Code** — copy a skill into your skills directory and invoke it by name:

```bash
git clone https://github.com/amitsheoran/automation-qa-skills.git
cp -r automation-qa-skills/skills/test-case-generator ~/.claude/skills/
# then in Claude Code: "Use the test-case-generator skill for this user story"
```

**Cursor / Codex / others** — reference or paste the relevant `SKILL.md` into your agent's
context and ask it to follow the skill.

See [`docs/getting-started.md`](docs/getting-started.md) for composing skills into workflows.

---

## Repository structure

```text
automation-qa-skills/
├── skills/         # one folder per skill (SKILL.md + examples/)
├── templates/      # SKILL_TEMPLATE.md for new skills
├── checklists/     # reusable QA checklists (e.g. release sign-off)
├── snippets/       # reusable CI / config snippets
├── examples/       # cross-skill example scenarios
├── docs/           # getting-started and guides
└── assets/         # images / diagrams
```

---

## Roadmap

- **v1.0** — Manual, automation, API/SQL, non-functional, delivery & AI skills.
- **v1.1** — Per-skill `examples/` for every skill.
- **v2.0** — Visual regression, mobile web, more framework snippets (Gatling, JMeter plans) *(current)*.
- **Still open from v1.x** — Cross-skill worked examples, spell/lint CI.
- **v3.0** — MCP integration, Jira/Azure DevOps API skills, automatic test-data pipelines.

---

## Contributing

Contributions welcome! See [`CONTRIBUTING.md`](CONTRIBUTING.md). Start from
[`templates/SKILL_TEMPLATE.md`](templates/SKILL_TEMPLATE.md), keep each skill focused, and
include examples.

## License

[MIT](LICENSE) © 2026 Amit Sheoran

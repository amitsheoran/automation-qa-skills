# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2026-11-07

### Added
- **visual-regression-testing** skill — screenshot-diff UI regressions against a
  reviewed baseline (Playwright `toHaveScreenshot`, with Percy/Chromatic noted as
  cloud alternatives).
- **mobile-web-testing** skill — responsive/touch testing of web apps on mobile
  browsers via Playwright device emulation, kept distinct from `appium` (native apps).
- JMeter and Gatling worked examples added to `performance-testing/examples/`,
  alongside the existing k6 example — the skill's description already named all three
  tools but only shipped one.

## [1.1.0] - 2026-11-06

### Added
- A full worked `examples/` input/output sample for every skill (previously only
  `test-case-generator` had one, despite `CONTRIBUTING.md` requiring it for all).
- A `## Example` section in every `SKILL.md` pointing to its new example file.

## [1.0.0] - 2026-07-10

### Added
- Initial public release of **automation-qa-skills**.
- **Manual & process skills:** test-case-generator, jira-bug-writer, root-cause-analysis,
  bug-triage, qa-documentation, requirement-analysis, test-plan, test-strategy,
  exploratory-testing, smoke-testing, sanity-testing, regression-testing.
- **Automation skills:** selenium-java, playwright, cypress, robot-framework, appium.
- **API & data skills:** api-testing, sql-testing, contract-testing, test-data-generator.
- **Non-functional skills:** performance-testing, accessibility-testing, security-testing.
- **Delivery & AI skills:** deployment-guide, ai-test-generator.
- Repository scaffolding: README, LICENSE (MIT), CONTRIBUTING, `.gitignore`,
  and `templates/`, `checklists/`, `snippets/`, `examples/`, `docs/`, `assets/` folders.
- Reusable `templates/SKILL_TEMPLATE.md` and shared QA checklists.

### Suggested (added beyond the original spec)
- **contract-testing** — consumer-driven contract testing (Pact / Spring Cloud Contract).
- **test-data-generator** — synthetic, boundary and PII-safe test data generation.

[1.0.0]: https://github.com/amitsheoran/automation-qa-skills/releases/tag/v1.0.0

---
name: robot-framework
description: Generate keyword-driven Robot Framework test suites (Browser/SeleniumLibrary, RequestsLibrary) with resource files, variables, and readable Gherkin-style keywords.
---

# Purpose

Produce maintainable, keyword-driven Robot Framework suites with reusable resource files and
clear, business-readable test cases.

## When to use

- Teams standardizing on Robot Framework for web or API automation.

## When NOT to use

- Code-first TS/JS teams → `playwright` / `cypress`.

## Inputs

- The flow, target URL/API, and preferred library (Browser vs SeleniumLibrary).

## Workflow

1. Split into **`*.robot`** suite + **`resource.robot`** (keywords) + **variables**.
2. Write **high-level, readable keywords** that hide low-level steps.
3. Prefer the **Browser** library (Playwright-based) for modern apps; SeleniumLibrary otherwise.
4. Use **Setup/Teardown** for state; tag tests (`[Tags]`) for selective runs.
5. Data-drive with **Test Templates** for many similar cases.

## Output format

```robotframework
*** Settings ***
Library           Browser
Resource          resource.robot
Suite Setup       New Browser    chromium    headless=True
Test Teardown     Close Context

*** Variables ***
${BASE_URL}       https://staging.example.com

*** Test Cases ***
Valid Login Redirects To Dashboard
    [Tags]    smoke    login
    Given User Is On Login Page
    When User Logs In As    user@x.com    Passw0rd!
    Then Dashboard Should Be Visible

*** Keywords ***
User Logs In As
    [Arguments]    ${email}    ${password}
    Fill Text      [data-testid=email]       ${email}
    Fill Secret    [data-testid=password]    ${password}
    Click          [data-testid=login-submit]
```

## Quality checklist

- [ ] Keywords are readable and reused via resource files.
- [ ] Variables/config externalized (no hardcoded URLs in cases).
- [ ] Setup/Teardown manage state; tags enable selective runs.
- [ ] Data-driven cases use Test Templates where appropriate.

## Example

See `examples/search-suite.md` for a full worked example.

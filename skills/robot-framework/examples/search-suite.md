# Example — Product search suite

## Input

> Automate: on the staging site, searching "wireless mouse" shows at least one result
> containing "mouse" in the title. Use the Browser library.

## Output

```robotframework
*** Settings ***
Library           Browser
Resource          resource.robot
Suite Setup       New Browser    chromium    headless=True
Test Teardown     Close Context

*** Variables ***
${BASE_URL}       https://staging.example.com

*** Test Cases ***
Searching Wireless Mouse Returns Matching Result
    [Tags]    smoke    search
    Given User Is On The Homepage
    When User Searches For    wireless mouse
    Then At Least One Result Title Should Contain    mouse

*** Keywords ***
User Is On The Homepage
    New Context    baseURL=${BASE_URL}
    New Page       ${BASE_URL}

User Searches For
    [Arguments]    ${query}
    Fill Text      [data-testid=search-input]     ${query}
    Click          [data-testid=search-submit]

At Least One Result Title Should Contain
    [Arguments]    ${text}
    ${titles}=    Get Text    [data-testid=result-title]    all=True
    List Should Contain Value    ${titles}    ${text}    ignore_case=True
```

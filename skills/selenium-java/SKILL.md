---
name: selenium-java
description: Generate maintainable Selenium WebDriver tests in Java using the Page Object Model, explicit waits, TestNG/JUnit, and stable locators.
---

# Purpose

Produce production-grade Selenium + Java UI automation: Page Objects, explicit waits, data-driven
tests, and clean assertions — no `Thread.sleep`, no brittle XPaths.

## When to use

- Automating web UI flows in a Java stack (Maven/Gradle, TestNG/JUnit).

## When NOT to use

- Modern JS-heavy app where auto-waiting is preferred → `playwright` / `cypress`.
- API-level checks → `api-testing`.

## Inputs

- The flow to automate, target URLs, stable locators (prefer `id`/`data-testid`), test data.

## Workflow

1. Scaffold: Maven/Gradle deps (selenium-java, testng/junit, webdrivermanager).
2. Create a **Page Object** per screen: locators + action methods, no assertions inside.
3. Use **explicit waits** (`WebDriverWait` + `ExpectedConditions`); never `Thread.sleep`.
4. Keep tests thin: arrange data → call page methods → assert.
5. Externalize config (base URL, browser) and data (data providers).
6. Add reporting (Allure/Extent) and screenshots on failure.

## Output format — Page Object + Test

```java
public class LoginPage {
    private final WebDriver driver;
    private final WebDriverWait wait;
    private final By email = By.cssSelector("[data-testid='email']");
    private final By password = By.cssSelector("[data-testid='password']");
    private final By submit = By.cssSelector("[data-testid='login-submit']");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    public DashboardPage loginAs(String user, String pass) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(email)).sendKeys(user);
        driver.findElement(password).sendKeys(pass);
        driver.findElement(submit).click();
        return new DashboardPage(driver);
    }
}

@Test
public void validLoginRedirectsToDashboard() {
    DashboardPage dash = new LoginPage(driver).loginAs("user@x.com", "Passw0rd!");
    Assert.assertTrue(dash.isLoaded(), "Dashboard should load after valid login");
}
```

## Quality checklist

- [ ] Page Object per screen; no assertions inside page methods.
- [ ] Explicit waits only — zero `Thread.sleep`.
- [ ] Stable locators (`id`/`data-testid`/css over brittle absolute XPath).
- [ ] Config and data externalized; failure screenshots captured.

## Example

See `examples/search-flow.md` for a full worked example.

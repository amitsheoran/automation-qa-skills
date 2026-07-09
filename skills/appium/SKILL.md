---
name: appium
description: Generate cross-platform mobile automation with Appium (Android/iOS) using the Page Object Model, accessibility-id locators, explicit waits, and capability config.
---

# Purpose

Produce robust mobile UI automation with Appium for native/hybrid apps, using stable
accessibility locators, Page Objects, and proper desired capabilities.

## When to use

- Automating native or hybrid mobile apps (Android/iOS).

## When NOT to use

- Mobile web only → a web framework (`playwright`/`selenium-java`) may be simpler.

## Inputs

- App under test (apk/ipa or bundle id), platform/device, and the flows to automate.

## Workflow

1. Define **desired capabilities** per platform (deviceName, app, automationName: UiAutomator2/XCUITest).
2. Prefer **`accessibility id`** locators (contentDescription / accessibilityLabel); avoid brittle xpath.
3. Use **Page Objects** (screens) with explicit **`WebDriverWait`**.
4. Handle **gestures** (scroll/swipe) via W3C Actions or mobile commands.
5. Externalize config; run against emulator/simulator locally and a device cloud in CI.

## Output format — Java (Appium)

```java
DesiredCapabilities caps = new DesiredCapabilities();
caps.setCapability("platformName", "Android");
caps.setCapability("appium:automationName", "UiAutomator2");
caps.setCapability("appium:app", "/apps/app-staging.apk");
AndroidDriver driver = new AndroidDriver(new URL("http://127.0.0.1:4723"), caps);

// LoginScreen (Page Object)
By email = AppiumBy.accessibilityId("login-email");
By submit = AppiumBy.accessibilityId("login-submit");

new WebDriverWait(driver, Duration.ofSeconds(15))
    .until(ExpectedConditions.visibilityOfElementLocated(email))
    .sendKeys("user@x.com");
driver.findElement(submit).click();
```

## Quality checklist

- [ ] Capabilities set correctly per platform (UiAutomator2 / XCUITest).
- [ ] `accessibility id` locators preferred over xpath.
- [ ] Page Objects + explicit waits; no fixed sleeps.
- [ ] Config externalized; CI targets emulator/device cloud.

# Example — Android login flow

## Input

> Automate the Android login screen on the staging build: fields with accessibility ids
> `login-email`, `login-password`, button `login-submit`. On success the Home screen
> shows `home-title`.

## Output

```java
DesiredCapabilities caps = new DesiredCapabilities();
caps.setCapability("platformName", "Android");
caps.setCapability("appium:automationName", "UiAutomator2");
caps.setCapability("appium:app", "/apps/app-staging.apk");
AndroidDriver driver = new AndroidDriver(new URL("http://127.0.0.1:4723"), caps);

// LoginScreen (Page Object)
public class LoginScreen {
    private final AndroidDriver driver;
    private final By email = AppiumBy.accessibilityId("login-email");
    private final By password = AppiumBy.accessibilityId("login-password");
    private final By submit = AppiumBy.accessibilityId("login-submit");

    public LoginScreen(AndroidDriver driver) { this.driver = driver; }

    public HomeScreen loginAs(String user, String pass) {
        new WebDriverWait(driver, Duration.ofSeconds(15))
            .until(ExpectedConditions.visibilityOfElementLocated(email)).sendKeys(user);
        driver.findElement(password).sendKeys(pass);
        driver.findElement(submit).click();
        return new HomeScreen(driver);
    }
}

@Test
public void validLoginShowsHomeScreen() {
    HomeScreen home = new LoginScreen(driver).loginAs("user@x.com", "Passw0rd!");
    By title = AppiumBy.accessibilityId("home-title");
    Assert.assertTrue(
        new WebDriverWait(driver, Duration.ofSeconds(10))
            .until(ExpectedConditions.visibilityOfElementLocated(title)).isDisplayed());
}
```

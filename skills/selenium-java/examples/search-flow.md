# Example — Searching on the staging site

## Input

> Automate search on the staging site: enter "wireless mouse" in the search box
> (`data-testid=search-input`), submit, and verify at least one result title contains "mouse".

## Output

```java
public class SearchPage {
    private final WebDriver driver;
    private final WebDriverWait wait;
    private final By searchInput = By.cssSelector("[data-testid='search-input']");
    private final By searchSubmit = By.cssSelector("[data-testid='search-submit']");
    private final By resultTitles = By.cssSelector("[data-testid='result-title']");

    public SearchPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    public SearchPage searchFor(String query) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(searchInput)).sendKeys(query);
        driver.findElement(searchSubmit).click();
        return this;
    }

    public List<String> resultTitles() {
        wait.until(ExpectedConditions.presenceOfAllElementsLocatedBy(resultTitles));
        return driver.findElements(resultTitles).stream().map(WebElement::getText).toList();
    }
}

@Test
public void searchingWirelessMouseReturnsMatchingResult() {
    List<String> titles = new SearchPage(driver).searchFor("wireless mouse").resultTitles();
    Assert.assertTrue(titles.stream().anyMatch(t -> t.toLowerCase().contains("mouse")),
        "Expected at least one result title to contain 'mouse'");
}
```

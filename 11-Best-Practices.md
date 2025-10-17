# 📘 BEST PRACTICES GUIDE

## 🎯 MỤC ĐÍCH FILE NÀY

File này là **reference handbook** cho developers trong quá trình implementation test automation framework, cung cấp:
- **Coding standards** và naming conventions chuẩn
- **Best practices** đã được verify qua thực tế
- **Anti-patterns** cần tránh để code maintainable
- **Checklists** để self-review trước khi commit

**Khi nào dùng file này:**
- Trong quá trình Step 3 (Implementation) - file 08-Process-Step-3-Overview.md (và 08A-M3, 08B-M4, 08C-M5)
- Khi code review cho team members
- Khi refactor existing test code
- Trước khi commit code lên repository

---

## 1️⃣ NAMING CONVENTIONS

### 1.1 Classes

| Class Type | Convention | Examples | ❌ Avoid |
|------------|-----------|----------|----------|
| **Page Objects** | `{Entity}Page` | `LoginPage`, `CheckoutPage`, `ProductListPage` | `Login`, `PageLogin`, `LoginPO` |
| **Test Classes** | `{Feature}Test` | `LoginTest`, `CheckoutFlowTest` | `TestLogin`, `LoginTestCase` |
| **Utilities** | `{Purpose}Helper` | `WaitHelper`, `DataHelper`, `ScreenshotHelper` | `Utils`, `Utility`, `Tool` |
| **Listeners** | `{Event}Listener` | `TestListener`, `RetryListener`, `ReportListener` | `MyListener`, `CustomListener` |
| **Base Classes** | `Base{Type}` | `BasePage`, `BaseTest`, `BaseHelper` | `AbstractPage`, `PageBase` |
| **Config** | `{Domain}Config` | `WebDriverConfig`, `DatabaseConfig` | `Configuration`, `Settings` |

**Lý do:**
- Consistency giúp dễ navigate trong large codebase
- IDE autocomplete hiệu quả hơn
- Team onboarding nhanh hơn

### 1.2 Methods

| Method Type | Convention | Examples | ❌ Avoid |
|-------------|-----------|----------|----------|
| **Page Actions** | `verb + Object` | `fillEmail()`, `clickSubmit()`, `selectCountry()` | `email()`, `submit()`, `action1()` |
| **Test Methods** | `test + Scenario` | `testValidLogin()`, `testEmptyEmailError()` | `test1()`, `loginTest()`, `myTest()` |
| **Getters** | `get + Property` | `getErrorMessage()`, `getSuccessText()` | `errorMessage()`, `error()` |
| **Validators** | `is/has + Condition` | `isVisible()`, `hasError()`, `isEnabled()` | `visible()`, `checkError()` |
| **Wait Methods** | `waitFor + Condition` | `waitForPageLoad()`, `waitForElementVisible()` | `wait()`, `sleep()` |

**Examples:**
```java
// ✅ GOOD - Clear intent
public void fillEmailField(String email) {
    emailField.sendKeys(email);
}

public boolean isLoginButtonVisible() {
    return loginButton.isDisplayed();
}

// ❌ BAD - Unclear intent
public void email(String s) {
    emailField.sendKeys(s);
}

public boolean check() {
    return loginButton.isDisplayed();
}
```

### 1.3 Variables

| Variable Type | Convention | Examples | ❌ Avoid |
|---------------|-----------|----------|----------|
| **WebElements** | camelCase descriptive | `emailField`, `submitButton`, `errorMessageLabel` | `e1`, `elem`, `element` |
| **Constants** | UPPER_SNAKE_CASE | `BASE_URL`, `MAX_TIMEOUT`, `DEFAULT_USERNAME` | `baseUrl`, `timeout` |
| **Test Data** | camelCase | `validEmail`, `invalidPassword` | `data1`, `testdata` |
| **Collections** | plural noun | `testCases`, `errorMessages`, `products` | `testCase`, `list1` |

**Examples:**
```java
// ✅ GOOD - Self-documenting
public class LoginPage extends BasePage {
    private static final String PAGE_TITLE = "Login - MyApp";
    private static final int LOGIN_TIMEOUT = 10;

    @FindBy(id = "email")
    private WebElement emailInputField;

    @FindBy(css = ".error-message")
    private WebElement emailErrorLabel;

    private List<WebElement> availableCountries;
}

// ❌ BAD - Cryptic names
public class LoginPage extends BasePage {
    private static final String t = "Login - MyApp";
    private static final int to = 10;

    @FindBy(id = "email")
    private WebElement e1;

    @FindBy(css = ".error-message")
    private WebElement err;

    private List<WebElement> list;
}
```

---

## 2️⃣ CODE ORGANIZATION

### 2.1 Package Structure

```
src/test/java/
├── config/                    ← Configuration classes
│   ├── WebDriverConfig.java   
│   ├── DatabaseConfig.java
│   └── ApplicationConfig.java
│
├── listeners/                 ← TestNG listeners
│   ├── TestListener.java
│   ├── RetryAnalyzer.java
│   └── SuiteListener.java
│
├── pages/                     ← Page Objects ONLY
│   ├── BasePage.java
│   ├── LoginPage.java
│   ├── HomePage.java
│   └── CheckoutPage.java
│
├── tests/                     ← Test classes ONLY
│   ├── BaseTest.java
│   ├── LoginTest.java
│   └── CheckoutTest.java
│
└── utils/                     ← Reusable utilities
    ├── WaitHelper.java
    ├── ScreenshotHelper.java
    ├── DataHelper.java
    └── LogHelper.java
```

**Rules:**
- **pages/** chỉ chứa Page Objects (không có test logic)
- **tests/** chỉ chứa Test classes (không có page logic)
- **utils/** chứa reusable components (không depend vào pages/tests)
- **config/** chứa configuration và setup (Spring beans)

### 2.2 File Structure Template

Mỗi Java class nên follow structure này:

```java
// 1. Package declaration
package pages;

// 2. Imports - Grouped và sorted
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import utils.WaitHelper;

// 3. Class declaration với JavaDoc
/**
 * Page Object for Login functionality
 * URL: https://app.example.com/login
 */
public class LoginPage extends BasePage {

    // 4. Constants - Static final
    private static final String PAGE_TITLE = "Login";
    private static final int TIMEOUT = 10;

    // 5. Dependencies - Injected via constructor
    private final WaitHelper waitHelper;

    // 6. WebElements - Private, @FindBy
    @FindBy(id = "email")
    private WebElement emailField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(css = "button[type='submit']")
    private WebElement submitButton;

    // 7. Constructor - Initialize dependencies
    public LoginPage(WebDriver driver) {
        super(driver);
        this.waitHelper = new WaitHelper(driver);
        PageFactory.initElements(driver, this);
    }

    // 8. Public methods - Page actions
    public void fillEmail(String email) {
        waitHelper.waitForElementVisible(emailField);
        emailField.clear();
        emailField.sendKeys(email);
    }

    public void fillPassword(String password) {
        passwordField.clear();
        passwordField.sendKeys(password);
    }

    public void clickSubmit() {
        submitButton.click();
    }

    // 9. Public getters - For assertions
    public String getPageTitle() {
        return driver.getTitle();
    }

    public boolean isSubmitButtonEnabled() {
        return submitButton.isEnabled();
    }

    // 10. Private helper methods - Internal logic
    private void highlightElement(WebElement element) {
        // Helper logic
    }
}
```

---

## 3️⃣ PAGE OBJECT MODEL BEST PRACTICES

### 3.1 Locator Strategy Priority

**Priority order** (từ tốt nhất → worst):

1. **id** - Fastest, most stable
   ```java
   @FindBy(id = "email")
   ```

2. **name** - Good for form fields
   ```java
   @FindBy(name = "username")
   ```

3. **linkText / partialLinkText** - Good for links
   ```java
   @FindBy(linkText = "Forgot Password?")
   ```

4. **cssSelector** - Flexible và fast
   ```java
   @FindBy(css = "button.btn-primary")
   ```

5. **xpath** - Last resort, slowest
   ```java
   @FindBy(xpath = "//button[@type='submit']")
   ```

**Why?**
- id/name là unique và fast
- CSS selector nhanh hơn XPath
- XPath phức tạp dễ break khi UI thay đổi

### 3.2 Locator Best Practices

```java
// ✅ GOOD - Specific và stable
@FindBy(id = "loginEmail")  // Unique ID
@FindBy(css = "button[data-testid='submit']")  // Test-specific attribute
@FindBy(css = ".form-control.email-input")  // Multiple classes

// ❌ BAD - Fragile locators
@FindBy(xpath = "//div/div/div[3]/input")  // Too specific path
@FindBy(css = "body > div:nth-child(5) > input")  // Position-dependent
@FindBy(xpath = "//*[contains(text(), 'Login')]")  // Text-dependent
```

### 3.3 Page Object Template (Complete)

```java
package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import utils.WaitHelper;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
 * Page Object for {Feature} Page
 * Handles all interactions with {feature} UI elements
 */
public class {Entity}Page extends BasePage {

    private static final Logger logger = LoggerFactory.getLogger({Entity}Page.class);
    private final WaitHelper waitHelper;

    // Locators - Private, @FindBy
    @FindBy(id = "fieldId")
    private WebElement inputField;

    @FindBy(css = "button.submit")
    private WebElement submitButton;

    @FindBy(css = ".error-message")
    private WebElement errorMessageLabel;

    // Constructor - Initialize PageFactory
    public {Entity}Page(WebDriver driver) {
        super(driver);
        this.waitHelper = new WaitHelper(driver);
        PageFactory.initElements(driver, this);
        logger.debug("{Entity}Page initialized");
    }

    // Actions - Public methods returning void or next page
    public void fillInputField(String value) {
        logger.info("Filling input field with: {}", value);
        waitHelper.waitForElementVisible(inputField);
        inputField.clear();
        inputField.sendKeys(value);
    }

    public HomePage clickSubmit() {
        logger.info("Clicking submit button");
        waitHelper.waitForElementClickable(submitButton);
        submitButton.click();
        return new HomePage(driver);  // Return next page
    }

    // Getters - For assertions in tests
    public String getErrorMessage() {
        waitHelper.waitForElementVisible(errorMessageLabel);
        return errorMessageLabel.getText();
    }

    public boolean isErrorMessageDisplayed() {
        try {
            return errorMessageLabel.isDisplayed();
        } catch (Exception e) {
            return false;
        }
    }

    // Validations - Return boolean
    public boolean isPageLoaded() {
        try {
            waitHelper.waitForElementVisible(submitButton);
            return true;
        } catch (Exception e) {
            logger.error("Page not loaded", e);
            return false;
        }
    }
}
```

### 3.4 Page Object Anti-Patterns

```java
// ❌ ANTI-PATTERN 1: Assertions in Page Objects
public void fillAndVerifyEmail(String email) {
    emailField.sendKeys(email);
    Assert.assertEquals(emailField.getAttribute("value"), email);  // ❌ NO!
}

// ✅ CORRECT: Assertions in Test
// Page Object:
public void fillEmail(String email) {
    emailField.sendKeys(email);
}

// Test:
loginPage.fillEmail(email);
Assert.assertEquals(loginPage.getEmailFieldValue(), email);  // ✅ YES


// ❌ ANTI-PATTERN 2: Test logic in Page Objects
public void loginWithValidation(String user, String pass) {
    fillUsername(user);
    if (pass.length() < 8) {  // ❌ Business logic doesn't belong here
        throw new IllegalArgumentException("Password too short");
    }
    fillPassword(pass);
    clickLogin();
}

// ✅ CORRECT: Test logic in Test class


// ❌ ANTI-PATTERN 3: Hard-coded waits
public void clickButton() {
    Thread.sleep(2000);  // ❌ NEVER!
    button.click();
}

// ✅ CORRECT: Explicit waits
public void clickButton() {
    waitHelper.waitForElementClickable(button);
    button.click();
}


// ❌ ANTI-PATTERN 4: Returning WebDriver
public WebDriver getDriver() {  // ❌ Breaks encapsulation
    return driver;
}

// ✅ CORRECT: Expose specific behaviors only
public boolean isElementVisible() {
    return element.isDisplayed();
}
```

---

## 4️⃣ TEST WRITING GUIDELINES

### 4.1 AAA Pattern (Arrange-Act-Assert)

**Every test should follow this structure:**

```java
@Test
public void testValidLogin() {
    // ARRANGE - Setup test data và preconditions
    String validEmail = "test@example.com";
    String validPassword = "Password123!";
    LoginPage loginPage = new LoginPage(driver);

    // ACT - Perform the action being tested
    loginPage.fillEmail(validEmail);
    loginPage.fillPassword(validPassword);
    HomePage homePage = loginPage.clickLogin();

    // ASSERT - Verify expected outcomes
    Assert.assertTrue(homePage.isWelcomeMessageVisible(), 
        "Welcome message should be visible after successful login");
    Assert.assertEquals(homePage.getLoggedInUsername(), "Test User",
        "Logged in username should match");
}
```

**Why AAA?**
- Clear test structure
- Easy to understand intent
- Easy to debug failures

### 4.2 Test Independence

**Rule:** Mỗi test phải chạy độc lập, không depend vào:
- Thứ tự execution
- State từ test khác
- Shared data

```java
// ❌ BAD - Tests depend on order
@Test(priority = 1)
public void testCreateUser() {
    // Creates user in DB
}

@Test(priority = 2)
public void testLoginUser() {
    // Assumes user from testCreateUser exists
}

// ✅ GOOD - Each test independent
@Test
public void testCreateUser() {
    // Create user
    // Verify creation
    // Cleanup
}

@Test
public void testLoginUser() {
    // Setup: Create user first
    String userId = testDataHelper.createTestUser();

    // Test login
    loginPage.login(testUser);

    // Cleanup
    testDataHelper.deleteUser(userId);
}
```

### 4.3 Test Data Management

```java
// ✅ GOOD - Unique test data per test
@Test
public void testUserRegistration() {
    String uniqueEmail = "test_" + System.currentTimeMillis() + "@test.com";
    String uniqueUsername = "user_" + UUID.randomUUID().toString().substring(0, 8);

    registerPage.fillEmail(uniqueEmail);
    registerPage.fillUsername(uniqueUsername);
    // ...
}

// ✅ GOOD - Use constants for reusable data
private static final String VALID_PASSWORD = "Test123!@#";
private static final String BASE_TEST_EMAIL = "test+%s@example.com";

@Test
public void testLogin() {
    String testEmail = String.format(BASE_TEST_EMAIL, System.currentTimeMillis());
    // ...
}
```

### 4.4 Test Method Naming

```java
// ✅ GOOD - Descriptive names
@Test
public void testLoginWithValidCredentialsShouldSucceed() {
    // Test logic
}

@Test
public void testLoginWithInvalidEmailShouldShowError() {
    // Test logic
}

@Test
public void testEmptyPasswordFieldShouldDisableLoginButton() {
    // Test logic
}

// ❌ BAD - Unclear names
@Test
public void test1() {
    // What does this test?
}

@Test
public void loginTest() {
    // Which login scenario?
}
```

### 4.5 Assertions Best Practices

```java
// ✅ GOOD - Meaningful error messages
Assert.assertTrue(loginButton.isEnabled(), 
    "Login button should be enabled when all fields are filled");

Assert.assertEquals(actualTitle, expectedTitle,
    "Page title should match after navigation");

// ❌ BAD - No error message
Assert.assertTrue(loginButton.isEnabled());
Assert.assertEquals(actualTitle, expectedTitle);


// ✅ GOOD - Multiple specific assertions
@Test
public void testSuccessfulLogin() {
    homePage = loginPage.login(validUser, validPass);

    Assert.assertTrue(homePage.isWelcomeMessageVisible(), "Welcome message missing");
    Assert.assertEquals(homePage.getUsername(), "John Doe", "Username mismatch");
    Assert.assertTrue(homePage.isLogoutButtonVisible(), "Logout button missing");
}

// ❌ BAD - Single vague assertion
@Test
public void testSuccessfulLogin() {
    homePage = loginPage.login(validUser, validPass);
    Assert.assertNotNull(homePage);  // Too vague
}
```

### 4.6 Test Organization with TestNG

```java
@Test(groups = {"smoke", "login"}, priority = 1)
public void testValidLogin() {
    // High priority smoke test
}

@Test(groups = {"regression", "login"}, dependsOnMethods = "testValidLogin")
public void testLoginHistory() {
    // Runs after testValidLogin
}

@Test(enabled = false, description = "Disabled due to bug JIRA-123")
public void testPasswordReset() {
    // Temporarily disabled
}

@Test(timeOut = 10000)  // Fail if takes > 10 seconds
public void testFastLogin() {
    // Performance-sensitive test
}

@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlakyFeature() {
    // Auto-retry on failure
}
```

---

## 5️⃣ ERROR HANDLING

### 5.1 Specific Exception Handling

```java
// ✅ GOOD - Handle specific exceptions
public void clickElement(WebElement element) {
    try {
        element.click();
    } catch (ElementClickInterceptedException e) {
        logger.warn("Click intercepted, retrying with JavaScript click");
        javascriptExecutor.executeScript("arguments[0].click();", element);
    } catch (StaleElementReferenceException e) {
        logger.warn("Element stale, re-locating");
        element = relocateElement();
        element.click();
    }
}

// ❌ BAD - Generic exception catching
public void clickElement(WebElement element) {
    try {
        element.click();
    } catch (Exception e) {  // Too broad
        logger.error("Error occurred", e);
        throw e;
    }
}
```

### 5.2 Graceful Degradation

```java
// ✅ GOOD - Provide fallback behavior
public String getErrorMessage() {
    try {
        waitHelper.waitForElementVisible(errorLabel, 5);
        return errorLabel.getText();
    } catch (TimeoutException e) {
        logger.warn("Error message not found within timeout");
        return "";  // Return empty instead of crashing
    }
}

// ❌ BAD - Let exception propagate
public String getErrorMessage() {
    waitHelper.waitForElementVisible(errorLabel, 5);  // Throws exception
    return errorLabel.getText();
}
```

### 5.3 Custom Exceptions

```java
// Define custom exception
public class PageNotLoadedException extends RuntimeException {
    public PageNotLoadedException(String pageName) {
        super("Page not loaded: " + pageName);
    }
}

// Use in Page Object
public class LoginPage extends BasePage {
    public LoginPage(WebDriver driver) {
        super(driver);
        if (!isPageLoaded()) {
            throw new PageNotLoadedException("LoginPage");
        }
    }

    private boolean isPageLoaded() {
        try {
            return driver.getTitle().contains("Login");
        } catch (Exception e) {
            return false;
        }
    }
}
```

### 5.4 Meaningful Error Messages

```java
// ✅ GOOD - Detailed error messages
public void fillEmail(String email) {
    try {
        waitHelper.waitForElementVisible(emailField, 10);
        emailField.sendKeys(email);
        logger.info("Successfully filled email: {}", email);
    } catch (TimeoutException e) {
        String errorMsg = String.format(
            "Email field not visible after 10s. Current URL: %s, Page title: %s",
            driver.getCurrentUrl(),
            driver.getTitle()
        );
        logger.error(errorMsg, e);
        throw new RuntimeException(errorMsg, e);
    }
}

// ❌ BAD - Vague error messages
public void fillEmail(String email) {
    emailField.sendKeys(email);  // No error handling or logging
}
```

---

## 6️⃣ WAIT STRATEGY

### 6.1 Wait Hierarchy

```
Priority Order (Best → Worst):

1. Implicit Wait (setup once globally)
   ↓
2. Explicit Wait (preferred for specific conditions)
   ↓
3. Fluent Wait (for complex polling scenarios)
   ↓
4. Thread.sleep() - ❌ NEVER USE
```

### 6.2 Implicit Wait Setup

```java
// Setup once in BaseTest or WebDriverConfig
public void setupDriver() {
    driver = new ChromeDriver();
    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
    driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(30));
}
```

### 6.3 Explicit Wait Examples

```java
// ✅ GOOD - Explicit waits for specific conditions
public void waitForElementVisible(WebElement element) {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.visibilityOf(element));
}

public void waitForElementClickable(WebElement element) {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.elementToBeClickable(element));
}

public void waitForTextToBe(WebElement element, String text) {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.textToBePresentInElement(element, text));
}

// ❌ BAD - Hard-coded sleep
public void waitForElement() {
    Thread.sleep(2000);  // ❌ NEVER!
}
```

### 6.4 Custom Wait Conditions

```java
// Custom wait condition
public void waitForPageLoad() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
    wait.until(driver -> {
        JavascriptExecutor js = (JavascriptExecutor) driver;
        return js.executeScript("return document.readyState").equals("complete");
    });
}

public void waitForAjaxComplete() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(20));
    wait.until(driver -> {
        JavascriptExecutor js = (JavascriptExecutor) driver;
        return (Boolean) js.executeScript("return jQuery.active == 0");
    });
}

public void waitForElementAttributeContains(WebElement element, String attribute, String value) {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(driver -> element.getAttribute(attribute).contains(value));
}
```

### 6.5 Fluent Wait for Complex Scenarios

```java
// Fluent wait with polling and ignoring specific exceptions
public void waitForElementWithFluentWait(By locator) {
    Wait<WebDriver> fluentWait = new FluentWait<>(driver)
        .withTimeout(Duration.ofSeconds(30))
        .pollingEvery(Duration.ofSeconds(2))
        .ignoring(NoSuchElementException.class)
        .ignoring(StaleElementReferenceException.class);

    fluentWait.until(driver -> {
        WebElement element = driver.findElement(locator);
        return element.isDisplayed() && element.isEnabled();
    });
}
```

---

## 7️⃣ DATA MANAGEMENT

### 7.1 Test Data Location Strategy

| Data Complexity | Storage Location | Example |
|-----------------|------------------|---------|
| **Simple** | In test class as constants | `final String VALID_EMAIL = "test@test.com";` |
| **Medium** | Properties/YAML files | `config.properties`, `testdata.yml` |
| **Complex** | JSON/CSV files | `users.json`, `testcases.csv` |
| **Dynamic** | Database/API | Test data service |

### 7.2 Simple Data - Constants

```java
public class LoginTest extends BaseTest {
    // Test data as constants
    private static final String VALID_EMAIL = "test@example.com";
    private static final String VALID_PASSWORD = "Test123!";
    private static final String INVALID_EMAIL = "invalid-email";

    @Test
    public void testValidLogin() {
        loginPage.login(VALID_EMAIL, VALID_PASSWORD);
        Assert.assertTrue(homePage.isDisplayed());
    }
}
```

### 7.3 Medium Data - Properties Files

```properties
# src/test/resources/testdata.properties
valid.email=test@example.com
valid.password=Test123!
admin.email=admin@example.com
admin.password=Admin123!
```

```java
// Load properties
public class TestDataHelper {
    private static Properties props;

    static {
        try (InputStream input = TestDataHelper.class
                .getClassLoader()
                .getResourceAsStream("testdata.properties")) {
            props = new Properties();
            props.load(input);
        } catch (IOException e) {
            throw new RuntimeException("Failed to load test data", e);
        }
    }

    public static String getValidEmail() {
        return props.getProperty("valid.email");
    }
}
```

### 7.4 Complex Data - JSON Files

```json
// src/test/resources/users.json
{
  "validUser": {
    "email": "valid@test.com",
    "password": "Test123!",
    "firstName": "John",
    "lastName": "Doe"
  },
  "adminUser": {
    "email": "admin@test.com",
    "password": "Admin123!",
    "role": "ADMIN"
  }
}
```

```java
// Load JSON
public class JsonDataHelper {
    public static Map<String, Object> getUserData(String userType) {
        ObjectMapper mapper = new ObjectMapper();
        try {
            File jsonFile = new File("src/test/resources/users.json");
            Map<String, Map<String, Object>> allUsers = mapper.readValue(jsonFile, Map.class);
            return allUsers.get(userType);
        } catch (IOException e) {
            throw new RuntimeException("Failed to load user data", e);
        }
    }
}

// Usage
Map<String, Object> userData = JsonDataHelper.getUserData("validUser");
String email = (String) userData.get("email");
```

### 7.5 Sensitive Data Management

```java
// ❌ BAD - Hardcoded credentials
public void login() {
    loginPage.login("admin@company.com", "SuperSecret123");  // ❌ DON'T!
}

// ✅ GOOD - Environment variables
public void login() {
    String email = System.getenv("TEST_EMAIL");
    String password = System.getenv("TEST_PASSWORD");
    loginPage.login(email, password);
}

// ✅ GOOD - Config file (gitignored)
// .gitignore includes: src/test/resources/credentials.properties
public class CredentialHelper {
    private static Properties props;

    static {
        try (InputStream input = CredentialHelper.class
                .getClassLoader()
                .getResourceAsStream("credentials.properties")) {
            props = new Properties();
            props.load(input);
        } catch (IOException e) {
            throw new RuntimeException("Credentials file not found", e);
        }
    }

    public static String getAdminEmail() {
        return props.getProperty("admin.email");
    }

    public static String getAdminPassword() {
        return props.getProperty("admin.password");
    }
}
```

### 7.6 Test Data Cleanup

```java
// ✅ GOOD - Cleanup after test
@Test
public void testUserRegistration() {
    String userId = null;
    try {
        // Arrange
        String uniqueEmail = "test_" + System.currentTimeMillis() + "@test.com";

        // Act
        userId = registerPage.registerUser(uniqueEmail, "Test123!");

        // Assert
        Assert.assertNotNull(userId);
    } finally {
        // Cleanup - Always runs
        if (userId != null) {
            testDataHelper.deleteUser(userId);
        }
    }
}
```

---

## 8️⃣ LOGGING STRATEGY

### 8.1 Log Levels

| Level | When to Use | Example |
|-------|-------------|---------|
| **DEBUG** | Detailed flow information, variable values | `logger.debug("Email field value: {}", email)` |
| **INFO** | Important checkpoints, test milestones | `logger.info("Test started: {}", testName)` |
| **WARN** | Recoverable issues, unexpected but handled | `logger.warn("Element not found, retrying...")` |
| **ERROR** | Test failures, unrecoverable errors | `logger.error("Test failed: {}", e.getMessage())` |

### 8.2 Logging Best Practices

```java
public class LoginPage extends BasePage {
    private static final Logger logger = LoggerFactory.getLogger(LoginPage.class);

    public void login(String email, String password) {
        logger.info("Attempting login with email: {}", email);

        try {
            fillEmail(email);
            logger.debug("Email filled successfully");

            fillPassword(password);
            logger.debug("Password filled successfully");

            clickLoginButton();
            logger.info("Login button clicked");

        } catch (Exception e) {
            logger.error("Login failed for email: {}", email, e);
            throw e;
        }
    }

    private void fillEmail(String email) {
        logger.debug("Filling email field with: {}", email);
        waitHelper.waitForElementVisible(emailField);
        emailField.sendKeys(email);
    }
}
```

### 8.3 What to Log

```java
// ✅ GOOD - Log important actions
logger.info("Starting test: {}", testName);
logger.info("Navigating to URL: {}", url);
logger.info("Clicking button: {}", buttonName);
logger.info("Test completed: {}", testName);

// ✅ GOOD - Log errors with context
logger.error("Failed to click element. URL: {}, Element: {}", 
    driver.getCurrentUrl(), elementLocator, exception);

// ❌ BAD - Over-logging
logger.debug("Entering method");  // Too verbose
logger.debug("Exiting method");   // Not useful
logger.debug("i = {}", i);         // Low-level details
```

### 8.4 Structured Logging

```java
// ✅ GOOD - Structured log with context
public void clickElement(String elementName) {
    Map<String, Object> context = new HashMap<>();
    context.put("action", "click");
    context.put("element", elementName);
    context.put("url", driver.getCurrentUrl());
    context.put("timestamp", System.currentTimeMillis());

    logger.info("Element interaction: {}", context);
}
```

---

## 9️⃣ COMMON ANTI-PATTERNS

### Anti-Pattern 1: Hard-coded Waits

```java
// ❌ BAD
public void clickButton() {
    Thread.sleep(2000);  // Unreliable, slows tests
    button.click();
}

// ✅ GOOD
public void clickButton() {
    waitHelper.waitForElementClickable(button, 10);
    button.click();
}
```

**Why bad:**
- Fixed delay cho mọi môi trường (fast/slow)
- Tests chậm hơn cần thiết
- Vẫn có thể fail nếu element chưa ready

### Anti-Pattern 2: Test Logic in Page Objects

```java
// ❌ BAD - Assertions trong Page Object
public void fillEmailAndVerify(String email) {
    emailField.sendKeys(email);
    Assert.assertEquals(emailField.getAttribute("value"), email);  // ❌
}

// ✅ GOOD - Page Object chỉ có actions
public void fillEmail(String email) {
    emailField.sendKeys(email);
}

public String getEmailValue() {
    return emailField.getAttribute("value");
}

// Test class có assertions
@Test
public void testEmailInput() {
    loginPage.fillEmail(testEmail);
    Assert.assertEquals(loginPage.getEmailValue(), testEmail);
}
```

### Anti-Pattern 3: Duplicate Code (DRY Violation)

```java
// ❌ BAD - Repeated code
@Test
public void testLogin1() {
    driver.get("https://app.com/login");
    driver.findElement(By.id("email")).sendKeys("test@test.com");
    driver.findElement(By.id("password")).sendKeys("pass");
    driver.findElement(By.id("submit")).click();
}

@Test
public void testLogin2() {
    driver.get("https://app.com/login");
    driver.findElement(By.id("email")).sendKeys("admin@test.com");
    driver.findElement(By.id("password")).sendKeys("admin");
    driver.findElement(By.id("submit")).click();
}

// ✅ GOOD - Reusable method
public class LoginPage {
    public void login(String email, String password) {
        driver.get("https://app.com/login");
        emailField.sendKeys(email);
        passwordField.sendKeys(password);
        submitButton.click();
    }
}

@Test
public void testLogin1() {
    loginPage.login("test@test.com", "pass");
}

@Test
public void testLogin2() {
    loginPage.login("admin@test.com", "admin");
}
```

### Anti-Pattern 4: Returning WebDriver from Page Objects

```java
// ❌ BAD - Breaks encapsulation
public class LoginPage {
    public WebDriver getDriver() {
        return driver;
    }
}

// Test
WebDriver driver = loginPage.getDriver();
driver.findElement(By.id("something")).click();  // Bypasses Page Object

// ✅ GOOD - Expose specific behaviors only
public class LoginPage {
    public boolean isLoginButtonVisible() {
        return loginButton.isDisplayed();
    }

    public void clickLoginButton() {
        loginButton.click();
    }
}
```

### Anti-Pattern 5: Single Assertion per Test (Too Granular)

```java
// ❌ BAD - Over-granular
@Test
public void testLoginButtonExists() {
    Assert.assertTrue(loginPage.isLoginButtonPresent());
}

@Test
public void testLoginButtonVisible() {
    Assert.assertTrue(loginPage.isLoginButtonVisible());
}

@Test
public void testLoginButtonEnabled() {
    Assert.assertTrue(loginPage.isLoginButtonEnabled());
}

// ✅ GOOD - Logical grouping
@Test
public void testLoginButtonState() {
    Assert.assertTrue(loginPage.isLoginButtonPresent(), "Button should be present");
    Assert.assertTrue(loginPage.isLoginButtonVisible(), "Button should be visible");
    Assert.assertTrue(loginPage.isLoginButtonEnabled(), "Button should be enabled");
}
```

### Anti-Pattern 6: Overly Complex XPath

```java
// ❌ BAD - Fragile XPath
@FindBy(xpath = "//div[@class='container']/div[2]/form/div[3]/input[@type='text']")
private WebElement emailField;

// ✅ GOOD - Simple, reliable locator
@FindBy(id = "email")
private WebElement emailField;

// Or if no ID
@FindBy(css = "input[name='email']")
private WebElement emailField;
```

### Anti-Pattern 7: Test Dependencies

```java
// ❌ BAD - Tests depend on each other
@Test(priority = 1)
public void testCreateUser() {
    userId = registerPage.register("test@test.com");  // Sets class variable
}

@Test(priority = 2, dependsOnMethods = "testCreateUser")
public void testLoginUser() {
    loginPage.login(userId);  // Uses variable from previous test
}

// ✅ GOOD - Independent tests
@Test
public void testCreateUser() {
    String userId = registerPage.register("test@test.com");
    Assert.assertNotNull(userId);
    // Cleanup
    testDataHelper.deleteUser(userId);
}

@Test
public void testLoginUser() {
    // Setup own data
    String userId = testDataHelper.createTestUser();
    loginPage.login(userId);
    // Cleanup
    testDataHelper.deleteUser(userId);
}
```

---

## 🔟 CHECKLIST TRƯỚC KHI COMMIT

**Code Quality:**
- [ ] Code follows naming conventions (classes, methods, variables)
- [ ] No hard-coded values (URLs, credentials, timeouts)
- [ ] Proper exception handling với meaningful messages
- [ ] Meaningful variable và method names
- [ ] Comments cho complex logic only
- [ ] No commented-out code

**Test Quality:**
- [ ] All tests pass locally
- [ ] Tests are independent (no execution order dependency)
- [ ] Test data is unique hoặc cleaned up
- [ ] Assertions có error messages
- [ ] No Thread.sleep() anywhere

**Page Object Quality:**
- [ ] No assertions trong Page Objects
- [ ] No test logic trong Page Objects
- [ ] Proper wait strategies used
- [ ] Locators follow priority strategy

**Documentation:**
- [ ] JavaDoc cho public methods (nếu complex)
- [ ] README updated nếu thêm features mới
- [ ] Configuration changes documented

**Performance:**
- [ ] No unnecessary waits
- [ ] Tests run trong reasonable time (<2 phút per test)

**Logging:**
- [ ] Proper log levels used
- [ ] No sensitive data in logs
- [ ] Sufficient logging for debugging

---

## 📚 ADDITIONAL BEST PRACTICES

### Configuration Management

```java
// ✅ GOOD - Centralized configuration
@Configuration
public class TestConfig {

    @Bean
    public WebDriver webDriver() {
        String browser = System.getProperty("browser", "chrome");
        return WebDriverFactory.createDriver(browser);
    }

    @Bean
    public WaitHelper waitHelper(WebDriver driver) {
        int timeout = Integer.parseInt(
            System.getProperty("wait.timeout", "10")
        );
        return new WaitHelper(driver, timeout);
    }
}

// Usage
mvn test -Dbrowser=firefox -Dwait.timeout=15
```

### Parallel Execution

```xml
<!-- testng.xml for parallel execution -->
<suite name="Parallel Test Suite" parallel="methods" thread-count="3">
    <test name="Login Tests">
        <classes>
            <class name="tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

```java
// ThreadLocal WebDriver cho parallel tests
public class BaseTest {
    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    @BeforeMethod
    public void setup() {
        driver.set(new ChromeDriver());
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    @AfterMethod
    public void teardown() {
        getDriver().quit();
        driver.remove();
    }
}
```

### Screenshot Naming Conventions

```java
public class ScreenshotHelper {
    public static String capture(WebDriver driver, String testName) {
        String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String threadId = String.valueOf(Thread.currentThread().getId());
        String fileName = String.format("%s_%s_thread_%s.png", 
            testName, timestamp, threadId);

        File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
        File dest = new File("screenshots/" + fileName);
        FileUtils.copyFile(screenshot, dest);

        return dest.getAbsolutePath();
    }
}
```

### Report Customization

```java
public class ExtentManager {
    private static ExtentReports extent;

    public static ExtentReports getInstance() {
        if (extent == null) {
            String reportPath = "reports/extent-report-" + 
                new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date()) + ".html";

            ExtentSparkReporter spark = new ExtentSparkReporter(reportPath);
            spark.config().setDocumentTitle("Test Automation Report");
            spark.config().setReportName("Regression Test Suite");
            spark.config().setTheme(Theme.STANDARD);

            extent = new ExtentReports();
            extent.attachReporter(spark);
            extent.setSystemInfo("Environment", System.getProperty("env", "QA"));
            extent.setSystemInfo("Browser", System.getProperty("browser", "Chrome"));
            extent.setSystemInfo("Tester", System.getProperty("user.name"));
        }
        return extent;
    }
}
```

---

## 🎓 SUMMARY

**Key Takeaways:**

1. **Follow conventions** - Naming, structure, organization
2. **Maintain separation of concerns** - Page Objects vs Tests
3. **Use proper waits** - No Thread.sleep(), use explicit waits
4. **Write independent tests** - No dependencies between tests
5. **Handle errors properly** - Specific exceptions, meaningful messages
6. **Log appropriately** - INFO for milestones, DEBUG for details, ERROR for failures
7. **Manage test data** - Unique data, proper cleanup, no hardcoded credentials
8. **Avoid anti-patterns** - DRY, no assertions in POs, no test logic in POs
9. **Review before commit** - Use checklist, run tests locally
10. **Document when needed** - Complex logic, configuration changes

**When in doubt:**
- Check this guide
- Review existing good examples
- Ask team for code review
- Prioritize maintainability over cleverness

---

*Last updated: October 2025*
*Version: 1.0*
*Applies to: Framework Files 01-10*

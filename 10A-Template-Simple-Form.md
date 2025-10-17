# 📝 TEMPLATE A: SIMPLE FORM

**SEE ALSO**:
- [10-Template-Library-Index.md](10-Template-Library-Index.md) for template selection guide
- [04-Decision-Tree.md](04-Decision-Tree.md) for decision tree
- [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md) for feature selection

---

## Overview

**Use Case**: Form đơn giản với ít trường và validation cơ bản

**Characteristics**:
- Single-page form
- < 10 fields
- Validation rules: Required + format only
- No complex business logic

**Timeline**: 1-2 days

**Features Included**: 4/8 Level 2 Features
- ✅ Configuration Management
- ✅ Logging Framework
- ✅ Explicit Waits
- ✅ Screenshot on Failure
- ❌ Data-Driven Testing (Optional)
- ❌ ExtentReports (Optional)
- ❌ Retry Mechanism (Optional)
- ❌ Parallel Execution (Not needed)

---

## Architecture

```
SimpleFormProject/
├── src/
│   ├
│   │
│   └── test/                       # TAF
│       ├── java/
│       │   ├── config/
│       │   │   └── TestConfig.java
│       │   ├── listeners/
│       │   │   └── TestListener.java       # Basic listener
│       │   ├── pages/
│       │   │   ├── BasePage.java
│       │   │   └── {{PAGE_NAME}}Page.java
│       │   ├── tests/
│       │   │   ├── BaseTest.java
│       │   │   └── {{TEST_CLASS}}Test.java # 3-5 tests
│       │   └── utils/
│       │       ├── WaitHelper.java
│       │       └── ScreenshotHelper.java
│       └── resources/
│           ├── config/
│           │   └── config.properties
│           ├── logging/
│           │   └── logback.xml
│           └── testng/
│               └── testng.xml
```

---

## Milestones Breakdown

⚠️ **PREREQUISITES** (User responsibility - 0 days):

Before starting M3, verify the application ALREADY EXISTS and is accessible:

**Required Information**:
- ✅ Application URL: `https://staging.example.com/{{form-path}}` (NOT localhost)
- ✅ Form fields and their validation rules documented
- ✅ Test credentials (if authentication required)
- ✅ Expected success/error messages

**Verification Steps**:
1. Manually access the application URL in browser
2. Verify form is visible and functional
3. Test one manual submission (valid + invalid) to understand behavior
4. Document all field locators (id, name, css selector)

---

### M3: TAF Core (0.5 day)
**Deliverables**:
- BasePage, BaseTest
- Page Object với locators
- 3-5 basic tests
- Phase 1 Features (Config, Logging, Waits, Screenshot)

**Code Template - Page Object**:
```java
package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class {{PAGE_NAME}}Page extends BasePage {
    private static final Logger logger = LoggerFactory.getLogger({{PAGE_NAME}}Page.class);

    // Locators
    @FindBy(id = "field1")
    private WebElement field1Input;

    @FindBy(id = "email")
    private WebElement emailInput;

    @FindBy(id = "field3")
    private WebElement field3Input;

    @FindBy(css = "button[type='submit']")
    private WebElement submitButton;

    @FindBy(css = ".alert-success")
    private WebElement successMessage;

    @FindBy(css = ".alert-danger")
    private WebElement errorMessage;

    @FindBy(css = ".text-danger")
    private WebElement validationError;

    // Constructor
    public {{PAGE_NAME}}Page(WebDriver driver) {
        super(driver);
        PageFactory.initElements(driver, this);
        logger.debug("Initialized {{PAGE_NAME}}Page");
    }

    // Actions
    public void fillField1(String value) {
        waitHelper.waitForElementVisible(field1Input);
        field1Input.clear();
        field1Input.sendKeys(value);
        logger.debug("Filled field1: {}", value);
    }

    public void fillEmail(String email) {
        waitHelper.waitForElementVisible(emailInput);
        emailInput.clear();
        emailInput.sendKeys(email);
        logger.debug("Filled email: {}", email);
    }

    public void fillField3(String value) {
        waitHelper.waitForElementVisible(field3Input);
        field3Input.clear();
        field3Input.sendKeys(value);
        logger.debug("Filled field3: {}", value);
    }

    public void clickSubmit() {
        waitHelper.waitForElementClickable(submitButton);
        submitButton.click();
        logger.debug("Clicked submit button");
    }

    // Getters for verification
    public String getSuccessMessage() {
        waitHelper.waitForElementVisible(successMessage);
        return successMessage.getText();
    }

    public String getErrorMessage() {
        waitHelper.waitForElementVisible(errorMessage);
        return errorMessage.getText();
    }

    public String getValidationError() {
        waitHelper.waitForElementVisible(validationError);
        return validationError.getText();
    }

    public boolean isSuccessMessageDisplayed() {
        try {
            return successMessage.isDisplayed();
        } catch (Exception e) {
            return false;
        }
    }
}
```

**Code Template - Test Class**:
```java
package tests;

import config.TestConfig;
import org.testng.Assert;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;
import pages.{{PAGE_NAME}}Page;

public class {{TEST_CLASS}}Test extends BaseTest {
    private {{PAGE_NAME}}Page page;
    private String baseUrl;

    @BeforeClass
    public void setupTest() {
        baseUrl = TestConfig.get("app.url"); // Real URL: https://staging.example.com
        page = new {{PAGE_NAME}}Page(driver);
        logger.info("Test setup completed for {{TEST_CLASS}}Test");
    }

    @Test(priority = 1, description = "Verify form submission with valid data")
    public void testValidSubmission() {
        logger.info("Starting testValidSubmission");

        // Arrange
        page.navigateTo(baseUrl + "/{{form-path}}"); // e.g., https://staging.example.com/registration
        String field1Value = "Valid Value";
        String emailValue = "test@example.com";
        String field3Value = "123";

        // Act
        page.fillField1(field1Value);
        page.fillEmail(emailValue);
        page.fillField3(field3Value);
        page.clickSubmit();

        // Assert
        Assert.assertTrue(page.isSuccessMessageDisplayed(),
            "Success message should be displayed");
        String successMsg = page.getSuccessMessage();
        Assert.assertTrue(successMsg.contains("successful"),
            "Success message should contain 'successful'");

        logger.info("✅ testValidSubmission PASSED");
    }

    @Test(priority = 2, description = "Verify error when required field is empty")
    public void testMissingRequiredField() {
        logger.info("Starting testMissingRequiredField");

        // Arrange
        page.navigateTo(baseUrl + "/{{form-path}}");

        // Act - Leave field1 empty
        page.fillEmail("test@example.com");
        page.fillField3("123");
        page.clickSubmit();

        // Assert
        String validationError = page.getValidationError();
        Assert.assertTrue(validationError.contains("required"),
            "Validation error should mention 'required'");

        logger.info("✅ testMissingRequiredField PASSED");
    }

    @Test(priority = 3, description = "Verify error for invalid email format")
    public void testInvalidEmailFormat() {
        logger.info("Starting testInvalidEmailFormat");

        // Arrange
        page.navigateTo(baseUrl + "/{{form-path}}");

        // Act
        page.fillField1("Valid Value");
        page.fillEmail("invalid-email"); // Invalid format
        page.fillField3("123");
        page.clickSubmit();

        // Assert
        String validationError = page.getValidationError();
        Assert.assertTrue(validationError.contains("email") ||
                         validationError.contains("Invalid"),
            "Validation error should mention email format issue");

        logger.info("✅ testInvalidEmailFormat PASSED");
    }
}
```

---

### M4: Skip (Template A không cần Level 2 Features)

### M5: Documentation (0.5 day)
**Deliverables**:
- Test cases Excel file
- README.md
- Final test report

---

## Common Patterns cho Template A

### Pattern 1: Simple Validation Testing
```java
@Test
public void testFieldValidation() {
    // Test required field
    page.clickSubmit(); // Submit empty form
    Assert.assertTrue(page.getValidationError().contains("required"));

    // Test format validation
    page.fillEmail("invalid");
    page.clickSubmit();
    Assert.assertTrue(page.getValidationError().contains("format"));

    // Test length validation
    page.fillField1("ab"); // Too short
    Assert.assertTrue(page.getValidationError().contains("3-50"));
}
```

### Pattern 2: Form Reset Testing
```java
@Test
public void testFormReset() {
    // Fill form
    page.fillField1("Value");
    page.fillEmail("test@example.com");

    // Navigate away and back
    page.navigateTo(baseUrl + "/other-page");
    page.navigateTo(baseUrl + "/{{form-path}}");

    // Verify form is cleared
    Assert.assertEquals(page.getField1Value(), "",
        "Form should be cleared after navigation");
}
```

---

## Real-World Considerations for Template A

### 1. Application Access
**Requirement**: Application MUST be deployed and accessible before testing
- ✅ Valid URL: `https://staging.example.com/form` (NOT `http://localhost:8080`)
- ✅ Network access: VPN required? Firewall rules configured?
- ✅ Authentication: Test user credentials available?

### 2. Environment Configuration
**config.properties Example**:
```properties
app.url=https://staging.company.com
test.username=testuser@company.com
test.password=TestPass123
environment=staging
```

### 3. Test Data Strategy
**For Simple Forms**:
- Use unique identifiers: `"test_" + System.currentTimeMillis() + "@example.com"`
- Avoid hardcoded values that might conflict with real data
- Consider cleanup: Delete test data after test completion

### 4. Common Real-World Scenarios
| Scenario | Solution |
|----------|----------|
| Form behind login | Add authentication step in @BeforeClass |
| CAPTCHA present | Request test environment without CAPTCHA |
| Rate limiting | Add delays between test executions |
| Dynamic field IDs | Use relative XPath or data-testid attributes |

### 5. Validation Message Matching
**Problem**: Real apps may have inconsistent error messages

**Solution**:
```java
// Instead of exact match:
Assert.assertEquals(error, "Email is required");

// Use contains:
Assert.assertTrue(error.toLowerCase().contains("email"));
Assert.assertTrue(error.toLowerCase().contains("required"));
```

---

## Pitfalls & Solutions

### Pitfall 1: Over-engineering
**Problem**: Trying to add all 8 features to simple form

**Solution**: Stick to Phase 1 features only. Simple forms don't need:
- Data-driven testing (few scenarios)
- Parallel execution (few tests)
- Retry mechanism (stable tests)

### Pitfall 2: Missing Basic Validation Tests
**Problem**: Only testing happy path

**Solution**: Always include:
- Missing required field test
- Invalid format test (email, number)
- Boundary value tests (min/max length)

### Pitfall 3: Hard-coded Test Data
**Problem**:
```java
page.fillEmail("test@example.com"); // Hard-coded
```

**Solution**:
```java
String email = TestConfig.get("test.email"); // From config
page.fillEmail(email);
```

---

# 📝 TEMPLATE B: COMPLEX FORM

**SEE ALSO**:
- [10-Template-Library-Index.md](10-Template-Library-Index.md) for template selection guide
- [04-Decision-Tree.md](04-Decision-Tree.md) for decision tree
- [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md) for feature selection

---

## Overview

**Use Case**: Form phức tạp với nhiều trường và validation logic nâng cao

**Characteristics**:
- Single-page form
- > 10 fields OR complex validation
- Business logic validation
- Cross-field validation
- Dependent fields

**Timeline**: 2-4 weeks

**Features Included**: 8/8 Level 2 Features (FULL)
- ✅ Configuration Management
- ✅ Logging Framework
- ✅ Explicit Waits
- ✅ Screenshot on Failure
- ✅ Data-Driven Testing
- ✅ ExtentReports
- ✅ Retry Mechanism
- ✅ Parallel Execution

---

## Architecture

```
ComplexFormProject/
├── src/
│   │
│   │
│   └── test/                       # TAF - FULL FEATURES
│       ├── java/
│       │   ├── config/
│       │   │   └── TestConfig.java
│       │   ├── listeners/
│       │   │   ├── TestListener.java            # ExtentReports integration
│       │   │   ├── RetryAnalyzer.java
│       │   │   └── SuiteListener.java
│       │   ├── pages/
│       │   │   ├── BasePage.java
│       │   │   └── {{PAGE_NAME}}Page.java       # Complex page object
│       │   ├── tests/
│       │   │   ├── BaseTest.java
│       │   │   └── {{TEST_CLASS}}Test.java      # 15-20 tests
│       │   └── utils/
│       │       ├── WaitHelper.java
│       │       ├── ScreenshotHelper.java
│       │       ├── DataHelper.java              # JSON/CSV/Excel
│       │       ├── DateHelper.java              # Date calculations
│       │       └── ExtentManager.java
│       └── resources/
│           ├── config/
│           │   └── config.properties
│           ├── logging/
│           │   └── logback.xml
│           ├── testdata/
│           │   ├── valid_data.json
│           │   ├── invalid_data.csv
│           │   └── boundary_data.xlsx
│           └── testng/
│               ├── testng.xml
│               └── testng-parallel.xml
```

---

## Milestones Breakdown

⚠️ **PREREQUISITES** (User responsibility - 0 days):

Before starting M3, verify the COMPLEX application ALREADY EXISTS and is accessible:

**Required Information**:
- ✅ Application URL: `https://staging.example.com/{{complex-form-path}}`
- ✅ Complete field list (15-20 fields) with:
  - Field names and types (text, date, dropdown, etc.)
  - Validation rules (required, pattern, min/max, unique constraints)
  - Cross-field dependencies (e.g., "If Status=PENDING, Reason required")
  - Business logic rules (e.g., "Age must be 18+", "Email must be unique")
- ✅ Test credentials (authentication usually required for complex forms)
- ✅ Expected error messages for each validation rule
- ✅ Sample valid data for testing

**Verification Steps**:
1. Manually test the form with VALID data → Note success message
2. Manually test with INVALID data → Document all error messages
3. Test cross-field validations (dependent fields, date ranges, etc.)
4. Verify unique constraints (email, ID, etc.) by submitting duplicate data
5. Document all field locators (id, name, CSS selectors)

**Business Logic Documentation Example**:
| Rule | Description | Expected Error Message |
|------|-------------|------------------------|
| Age 18+ | Birth date must be 18+ years ago | "Must be at least 18 years old" |
| Email unique | Email cannot be duplicate | "Email already registered" |
| Dependent field | If Status=PENDING, Reason required | "Reason is required for PENDING status" |

---

### M3: TAF Core (2 days)
**Deliverables**:
- BasePage, BaseTest với Phase 1 features
- Complex Page Object với 15-20 locators
- 5-7 basic tests (happy + major errors)
- All Phase 1 features integrated

**Example - Complex Page Object**: See detailed code in original documentation

---

### M4: Level 2 Features (2-3 days)
**Deliverables**:
- Data-Driven Testing với JSON/CSV/Excel
- ExtentReports integration
- Retry Mechanism
- Parallel Execution config

**Example - Data-Driven Test**: See detailed code in original documentation

**Example - Test Data Files**:

**valid_data.json**:
```json
[
  {
    "field1": "SV123456",
    "email": "user1@test.com",
    "phone": "0123456789",
    "status": "ACTIVE"
  },
  {
    "field1": "SV234567",
    "email": "user2@test.com",
    "phone": "0987654321",
    "status": "INACTIVE"
  }
]
```

**invalid_data.csv**:
```csv
field1,email,expectedError
ABC123,test@test.com,SV followed by 6 digits
SV12345,test@test.com,SV followed by 6 digits
SV123456,invalid-email,Invalid email
```

---

### M5: Documentation (1 day)
**Deliverables**:
- Comprehensive test cases Excel (15-20 cases)
- README with setup instructions
- Final ExtentReports
- Troubleshooting guide

---

## Common Patterns cho Template B

### Pattern 1: Cross-field Validation Testing
```java
@Test
public void testCrossFieldValidation() {
    // Test: End date must be after start date
    page.fillStartDate("2024-12-31");
    page.fillEndDate("2024-01-01"); // Before start
    page.clickSubmit();

    Assert.assertTrue(page.getErrorMessage().contains("after start date"));
}
```

### Pattern 2: Unique Constraint Testing
```java
@Test
public void testUniqueConstraint() {
    // First submission
    page.fillForm(uniqueValue);
    page.clickSubmit();
    Assert.assertTrue(page.isSuccessMessageDisplayed());

    // Duplicate submission
    page.navigateTo(formUrl);
    page.fillForm(uniqueValue); // Same value
    page.clickSubmit();
    Assert.assertTrue(page.getErrorMessage().contains("already exists"));
}
```

### Pattern 3: Dynamic Dropdown Testing
```java
@Test
public void testDependentDropdown() {
    // Select province
    page.selectProvince("Hanoi");

    // Wait for district dropdown to update
    waitHelper.waitForElementVisible(page.getDistrictDropdown());

    // Verify districts for Hanoi are loaded
    List<String> districts = page.getDistrictOptions();
    Assert.assertTrue(districts.contains("Ba Dinh"));
    Assert.assertTrue(districts.contains("Hoan Kiem"));
}
```

---

## Real-World Considerations for Template B

### 1. Business Logic Documentation is CRITICAL
**Before Writing Tests**:
- Request detailed business rules documentation from BA/Product Owner
- Create a validation rules matrix (see Prerequisites example)
- Verify edge cases: What happens when Age = exactly 18? Is 17 years 364 days old valid?

### 2. Unique Constraint Testing Strategy
**Problem**: Testing unique constraints (email, ID) in shared staging environment

**Solutions**:
```java
// Strategy 1: Unique test data per run
String uniqueEmail = "test_" + System.currentTimeMillis() + "@example.com";
String uniqueField1 = "SV" + ThreadLocalRandom.current().nextInt(100000, 999999);

// Strategy 2: Cleanup after test
@AfterMethod
public void cleanup() {
    if (createdEntityId != null) {
        // Call delete API or reset data
        deleteTestEntity(createdEntityId);
    }
}
```

### 3. Dependent Field Dynamics
**Challenge**: Fields that show/hide based on other field values

**Solution**:
```java
// WRONG: Assume field is always present
dependentField.sendKeys("value"); // Fails if field hidden

// CORRECT: Check visibility first
if (page.isDependentFieldVisible()) {
    page.fillDependentField("value");
}
```

### 4. Complex Form Performance
**Real-World Issue**: Forms with 15-20 fields may have:
- Slow validation (AJAX calls to backend)
- Loading spinners between sections
- Auto-save features

**Solution**:
```java
// Wait for validation to complete
waitHelper.waitForAjaxComplete();
waitHelper.waitForElementNotVisible(loadingSpinner);

// Wait for auto-save confirmation
waitHelper.waitForElementVisible(autoSaveIndicator);
Assert.assertEquals(autoSaveIndicator.getText(), "Saved");
```

### 5. Error Message Consistency
**Real-World Problem**: Different error types show in different locations
- Client-side validation: Red text under field
- Server-side validation: Alert banner at top
- Business logic errors: Modal dialog

**Solution**:
```java
public String getAnyErrorMessage() {
    // Check all possible error locations
    if (isFieldErrorVisible()) return getFieldError();
    if (isAlertErrorVisible()) return getAlertError();
    if (isModalErrorVisible()) return getModalError();
    return "";
}
```

### 6. Environment-Specific Configurations
**config.properties for Staging**:
```properties
app.url=https://staging.company.com
environment=staging
# Complex forms often need longer timeouts
explicit.wait=20
# Unique constraint testing
test.email.prefix=staging_test
test.data.cleanup=true
```

---

## Pitfalls & Solutions

### Pitfall 1: Timing Issues với Complex Forms
**Problem**: Elements chưa load khi test chạy

**Solution**:
```java
// Use explicit waits với custom conditions
waitHelper.waitForElementVisible(element);
waitHelper.waitForTextPresent(element, "Expected text");

// Wait cho AJAX calls complete
wait.until(driver ->
    ((JavascriptExecutor) driver).executeScript("return jQuery.active == 0"));
```

### Pitfall 2: Test Data Conflicts
**Problem**: Parallel tests conflict vì duplicate data

**Solution**:
```java
// Generate unique test data per test
String uniqueEmail = "test_" + System.currentTimeMillis() + "@test.com";
String uniqueField1 = "SV" + ThreadLocalRandom.current().nextInt(100000, 999999);
```

### Pitfall 3: Complex Validation Order
**Problem**: Không biết validation nào trigger trước

**Solution**:
```java
// Test từng validation riêng biệt
@Test
public void testValidation_Field1_Required() { /* Only test field1 required */ }

@Test
public void testValidation_Field1_Format() { /* Only test field1 format */ }

@Test
public void testValidation_Email_Unique() { /* Only test email unique */ }
```

### Pitfall 4: Over-complicated Page Objects
**Problem**: Page Object class quá lớn (>500 lines)

**Solution**:
```java
// Break into multiple page objects hoặc use Page Fragments
public class BasicInfoSection extends BasePage { /* Section 1 */ }
public class AdditionalInfoSection extends BasePage { /* Section 2 */ }

// Main page composes fragments
public class {{PAGE_NAME}}Page extends BasePage {
    private BasicInfoSection basicInfo;
    private AdditionalInfoSection additionalInfo;

    public {{PAGE_NAME}}Page(WebDriver driver) {
        super(driver);
        this.basicInfo = new BasicInfoSection(driver);
        this.additionalInfo = new AdditionalInfoSection(driver);
    }
}
```

---

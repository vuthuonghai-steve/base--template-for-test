# 📝 TEMPLATE C: MULTI-STEP FORM

**SEE ALSO**:
- [10-Template-Library-Index.md](10-Template-Library-Index.md) for template selection guide
- [04-Decision-Tree.md](04-Decision-Tree.md) for decision tree
- [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md) for feature selection

---

## Overview

**Use Case**: Wizard/multi-page form với 2+ steps

**Characteristics**:
- 2-5 steps/pages
- Navigation between steps (Next, Previous, Submit)
- State persistence giữa steps
- Progress indicator
- Optional step validation

**Timeline**: 3-5 days

**Features Included**: 6/8 Level 2 Features
- ✅ Configuration Management
- ✅ Logging Framework
- ✅ Explicit Waits
- ✅ Screenshot on Failure
- ✅ Data-Driven Testing (Optional)
- ✅ ExtentReports
- ✅ Retry Mechanism (Optional)
- ❌ Parallel Execution (Not recommended - state management phức tạp)

---

## Architecture

```
MultiStepFormProject/
├──
│   │
│   └── test/
│       ├── java/
│       │   ├── pages/
│       │   │   ├── BasePage.java
│       │   │   ├── Step1Page.java             # Page Object for Step 1
│       │   │   ├── Step2Page.java             # Page Object for Step 2
│       │   │   ├── Step3Page.java             # Page Object for Step 3
│       │   │   └── ReviewPage.java            # Page Object for Review
│       │   └── tests/
│       │       └── WizardTest.java            # Multi-step flow tests
│       └── resources/
│           └── testdata/
│               └── wizard_data.json           # Complete wizard data
```

---

## Milestones Breakdown

⚠️ **PREREQUISITES** (User responsibility - 0 days):

Before starting M3, verify the MULTI-STEP wizard ALREADY EXISTS and is accessible:

**Required Information**:
- ✅ Application URL (Step 1): `https://staging.example.com/wizard/step1`
- ✅ Number of steps in the wizard (2-5 steps typical)
- ✅ Fields for EACH step
- ✅ Navigation flow
- ✅ State persistence mechanism
- ✅ Validation rules
- ✅ Success criteria

**Wizard Flow Documentation Example**:
| Step | URL | Fields | Validation | Navigation |
|------|-----|--------|------------|------------|
| 1 | /wizard/step1 | firstName, lastName, email | Required, email format | Next → Step 2 |
| 2 | /wizard/step2 | street, city, zipCode | Required | Previous ← Step 1, Next → Step 3 |
| 3 | /wizard/step3 | phone, preferences | Phone format | Previous ← Step 2, Next → Review |
| Review | /wizard/review | (Display all data) | None | Back ← Step 3, Submit → Success |

---

### M3: TAF với Multi-Step Testing (1-2 days)
**Deliverables**:
- Page Objects cho mỗi step
- Tests cho complete flow
- Tests cho navigation (back/forward)
- Tests cho state persistence

---

### M4: Enhancement Features (Optional - 1 day)
**Deliverables**:
- ExtentReports cho multi-step flow
- Data-driven tests với complete wizard data
- Retry mechanism

---

### M5: Documentation (0.5 day)
**Deliverables**:
- Test cases cho complete flow + navigation + validation
- README
- Test reports

---

## Common Patterns cho Template C

### Pattern 1: Save and Resume
```java
@Test
public void testSaveAndResume() {
    // Complete Step 1
    step1.fillAndNext();

    // Get session ID hoặc bookmark URL
    String bookmarkUrl = driver.getCurrentUrl();

    // Close browser (simulate leaving)
    driver.quit();

    // Reopen và resume
    driver = new ChromeDriver();
    driver.get(bookmarkUrl);

    // Verify at correct step với data preserved
    Assert.assertTrue(driver.getCurrentUrl().contains("step2"));
}
```

### Pattern 2: Skip Optional Steps
```java
@Test
public void testSkipOptionalStep() {
    // Step 1 (required)
    step1.fillAndNext();

    // Step 2 (optional) - Click "Skip"
    step2.clickSkip();

    // Should go directly to Step 3
    Assert.assertTrue(driver.getCurrentUrl().contains("step3"));
}
```

### Pattern 3: Edit Previous Step from Review
```java
@Test
public void testEditFromReview() {
    // Complete wizard to review
    completeWizardToReview();

    // Click "Edit Step 2"
    reviewPage.clickEditStep2();

    // Should be on Step 2
    Assert.assertTrue(driver.getCurrentUrl().contains("step2"));

    // Make changes
    step2.fillCity("Boston"); // Change from original

    // Return to review
    step2.clickNext();
    step3.clickNext();

    // Verify change reflected
    Assert.assertEquals(reviewPage.getReviewedCity(), "Boston");
}
```

---

## Pitfalls & Solutions

### Pitfall 1: Session Timeout
**Problem**: Session expires giữa các steps, mất data

**Solution**:
```properties
# application.properties
server.servlet.session.timeout=30m
```

### Pitfall 2: Direct URL Access
**Problem**: User truy cập Step 3 trực tiếp mà chưa complete Step 1, 2

**Solution**: Implement server-side check to redirect to Step 1 if previous steps not completed

### Pitfall 3: Browser Back Button
**Problem**: User dùng browser back button, state mismatch

**Solution**: Disable browser back hoặc handle properly with JavaScript

---

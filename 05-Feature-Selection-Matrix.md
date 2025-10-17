# 🔍 FEATURE SELECTION MATRIX - MA TRẬN CHỌN FEATURES

## MỤC ĐÍCH FILE NÀY

Sau khi đã chọn **Template** (từ File 04), sử dụng file này để xác định chính xác **Level 2 Features nào cần implement** và **thứ tự ưu tiên**.

---

## 🎯 8 LEVEL 2 FEATURES

| # | Feature | Purpose | Implementation Effort |
|:--|:--------|:--------|:---------------------:|
| 1 | **Data-Driven Testing** | Chạy 1 test với nhiều bộ data | ⭐⭐ |
| 2 | **Screenshot on Failure** | Auto capture khi test fail | ⭐ |
| 3 | **Logging Framework** | Structured logging (SLF4J + Logback) | ⭐ |
| 4 | **Retry Mechanism** | Auto retry flaky tests | ⭐ |
| 5 | **ExtentReports** | Professional HTML reports | ⭐⭐ |
| 6 | **Configuration Management** | Externalized config (properties file) | ⭐ |
| 7 | **Explicit Waits** | Dynamic waits thay Thread.sleep() | ⭐⭐ |
| 8 | **Parallel Execution** | Run tests song song | ⭐⭐ |

**Legend:**
- ⭐ = Easy (< 1 hour)
- ⭐⭐ = Medium (1-3 hours)
- ⭐⭐⭐ = Hard (> 3 hours)

---

## FEATURE SELECTION MATRIX

### Template A: Simple Form

| Feature | Status | Priority | Rationale |
|:--------|:------:|:--------:|:----------|
| Data-Driven Testing | ❌ Optional | LOW | Simple form thường có ít scenarios |
| Screenshot on Failure | ✅ Required | HIGH | Essential cho debugging |
| Logging Framework | ✅ Required | HIGH | Better than console logs |
| Retry Mechanism | ❌ Optional | LOW | Simple tests ít flaky |
| ExtentReports | ❌ Optional | MEDIUM | Console reports đủ, nhưng HTML đẹp hơn |
| Configuration Management | ✅ Required | HIGH | Avoid hard-coded URLs |
| Explicit Waits | ✅ Required | HIGH | Better than Thread.sleep() |
| Parallel Execution | ❌ Not Needed | LOW | Ít tests, sequential OK |

**Summary**: 4/8 Required, 2/8 Optional, 2/8 Not Needed

**Timeline**: 1-2 days (with required features only)

---

### Template B: Complex Form

| Feature | Status | Priority | Rationale |
|:--------|:------:|:--------:|:----------|
| Data-Driven Testing | ✅ Required | HIGH | Nhiều scenarios cần nhiều data sets |
| Screenshot on Failure | ✅ Required | HIGH | Critical cho complex debugging |
| Logging Framework | ✅ Required | HIGH | Track complex test flow |
| Retry Mechanism | ✅ Required | MEDIUM | Complex tests dễ flaky |
| ExtentReports | ✅ Required | HIGH | Professional reports needed |
| Configuration Management | ✅ Required | HIGH | Multiple environments |
| Explicit Waits | ✅ Required | HIGH | Complex UI needs proper waits |
| Parallel Execution | ✅ Required | MEDIUM | Speed up execution time |

**Summary**: 8/8 Required

**Timeline**: 2-4 weeks (full features implementation)

---

### Template C: Multi-Step Form

| Feature | Status | Priority | Rationale |
|:--------|:------:|:--------:|:----------|
| Data-Driven Testing | ✅ Required | MEDIUM | Test different paths through wizard |
| Screenshot on Failure | ✅ Required | HIGH | Debug which step failed |
| Logging Framework | ✅ Required | HIGH | Track wizard navigation |
| Retry Mechanism | ✅ Optional | MEDIUM | Navigation có thể flaky |
| ExtentReports | ✅ Required | HIGH | Visualize multi-step flow |
| Configuration Management | ✅ Required | HIGH | Different wizard configurations |
| Explicit Waits | ✅ Required | HIGH | Wait for step transitions |
| Parallel Execution | ❌ Not Recommended | LOW | State management phức tạp |

**Summary**: 6/8 Required, 1/8 Optional, 1/8 Not Recommended

**Timeline**: 3-5 days

---

### Template D: CRUD Operations

| Feature | Status | Priority | Rationale |
|:--------|:------:|:--------:|:----------|
| Data-Driven Testing | ✅ Required | HIGH | Test CRUD với nhiều entities |
| Screenshot on Failure | ✅ Required | HIGH | Debug database state |
| Logging Framework | ✅ Required | HIGH | Track database operations |
| Retry Mechanism | ✅ Required | MEDIUM | DB operations có thể timeout |
| ExtentReports | ✅ Required | HIGH | Comprehensive test report |
| Configuration Management | ✅ Required | HIGH | Multiple DB environments |
| Explicit Waits | ✅ Required | HIGH | Wait for DB operations |
| Parallel Execution | ❌ Not Recommended | LOW | DB state conflicts |

**Summary**: 7/8 Required, 1/8 Not Recommended

**Timeline**: 2-3 weeks

---

### Template E: Search & Filter

| Feature | Status | Priority | Rationale |
|:--------|:------:|:--------:|:----------|
| Data-Driven Testing | ✅ Required | HIGH | Test nhiều search queries |
| Screenshot on Failure | ✅ Required | HIGH | Capture search results |
| Logging Framework | ✅ Required | HIGH | Log search queries & results |
| Retry Mechanism | ✅ Optional | MEDIUM | Search có thể timeout |
| ExtentReports | ✅ Required | HIGH | Show search performance |
| Configuration Management | ✅ Required | HIGH | Different search endpoints |
| Explicit Waits | ✅ Required | HIGH | Wait for search results |
| Parallel Execution | ✅ Optional | MEDIUM | Speed up if many scenarios |

**Summary**: 6/8 Required, 2/8 Optional

**Timeline**: 1-2 weeks

---

## FEATURE IMPLEMENTATION PRIORITY

### Phase 1: FOUNDATION (Always implement first)
**Priority: CRITICAL**

```
1. Configuration Management
   - File: config.properties + TestConfig.java
   - Why: Avoid hard-coded values
   - Time: 30 mins

2. Logging Framework
   - Files: logback.xml + SLF4J usage
   - Why: Better than System.out.println()
   - Time: 30 mins

3. Explicit Waits
   - File: WaitHelper.java
   - Why: Stable tests
   - Time: 1 hour

4. Screenshot on Failure
   - Files: ScreenshotHelper.java + TestListener.java
   - Why: Essential debugging tool
   - Time: 1 hour
```

**Total Phase 1**: 3 hours

---

### Phase 2: ENHANCEMENT (Implement based on template)
**Priority: HIGH for Templates B, C, D, E**

```
5. ExtentReports
   - Files: ExtentManager.java + TestListener integration
   - Why: Professional HTML reports
   - Time: 2 hours

6. Data-Driven Testing
   - Files: DataHelper.java + test data files
   - Why: Reuse test logic with multiple data sets
   - Time: 2-3 hours
```

**Total Phase 2**: 4-5 hours

---

### Phase 3: OPTIMIZATION (Implement if needed)
**Priority: MEDIUM**

```
7. Retry Mechanism
   - File: RetryAnalyzer.java
   - Why: Handle flaky tests
   - Time: 1 hour

8. Parallel Execution
   - File: testng-parallel.xml
   - Why: Faster test execution
   - Time: 1-2 hours
   - Note: Requires thread-safe implementation
```

**Total Phase 3**: 2-3 hours

---

## FEATURE DEPENDENCIES

### Dependency Graph

```
Configuration Management (independent)
    ↓
Logging Framework (independent)
    ↓
Explicit Waits (independent)
    ↓
Screenshot on Failure (depends on Logging)
    ↓
ExtentReports (depends on Screenshot)
    ↓
Data-Driven Testing (depends on Configuration)
    ↓
Retry Mechanism (depends on Logging)
    ↓
Parallel Execution (depends on all above)
```

**Implementation Order**: Top to bottom

---

## CUSTOMIZATION GUIDE

### Scenario 1: Tight Timeline

**Problem**: User chỉ có 2 days cho Template B (normally 2-4 weeks)

**Solution**: Implement Phase 1 only
```
Reduce scope:
- ✅ Keep: Config, Logging, Waits, Screenshot (3 hours)
- ❌ Skip: ExtentReports, Data-driven, Retry, Parallel
- Trade-off: Functional tests work, but less features
```

---

### Scenario 2: Learning Project

**Problem**: User muốn học tất cả features dù Template A

**Solution**: Upgrade to full feature set
```
Implement all 8 features cho Template A:
- Timeline: Extend from 1-2 days to 3-4 days
- Benefit: Comprehensive learning experience
- Trade-off: Overkill cho simple form, nhưng educational value cao
```

---

### Scenario 3: Production Project

**Problem**: Dự án production cần high quality

**Solution**: Full Phase 1 + 2, selective Phase 3
```
Must-have:
- ✅ All Phase 1 (Foundation)
- ✅ All Phase 2 (Enhancement)
- ✅ Retry Mechanism (handle production flakiness)
- ⚠️ Parallel Execution (nếu CI/CD pipeline cần speed)
```

---

## FEATURE SPECIFICATION DETAILS

### 1. Data-Driven Testing

**What it does**: Run same test method với multiple data sets

**Files needed**:
- `utils/DataHelper.java` - JSON/CSV/Excel parser
- `testdata/*.json|csv|xlsx` - Test data files

**Example**:
```java
@DataProvider(name = "validUsers")
public Object[][] getValidUsers() {
    return DataHelper.readJSON("users.json");
}

@Test(dataProvider = "validUsers")
public void testValidRegistration(String id, String name, String email) {
    // Test runs once per data row
}
```

**When to use**:
- ✅ Need to test nhiều combinations
- ✅ Same test logic, different data
- ❌ Chỉ test 1-2 scenarios

---

### 2. Screenshot on Failure

**What it does**: Tự động chụp màn hình khi test fail

**Files needed**:
- `utils/ScreenshotHelper.java` - Capture logic
- `listeners/TestListener.java` - @AfterMethod hook

**Example**:
```java
@Override
public void onTestFailure(ITestResult result) {
    String path = ScreenshotHelper.capture(driver, result.getName());
    logger.error("Test failed, screenshot: {}", path);
}
```

**Output**: `screenshots/TestName_20250110_143022.png`

---

### 3. Logging Framework

**What it does**: Structured logging thay System.out.println()

**Files needed**:
- `resources/logback.xml` - Log configuration
- Use SLF4J API in code

**Example**:
```java
private static final Logger logger = LoggerFactory.getLogger(MyTest.class);

logger.info("Test started: {}", testName);
logger.debug("Input data: {}", data);
logger.error("Validation failed", exception);
```

**Output**: `logs/automation.log` với timestamp, level, message

---

### 4. Retry Mechanism

**What it does**: Auto retry failed tests (max 2 times)

**Files needed**:
- `listeners/RetryAnalyzer.java` - Implements IRetryAnalyzer

**Example**:
```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlakyScenario() {
    // If fails, auto retry 2 times before marking as failed
}
```

**When to use**:
- ✅ Network issues
- ✅ Timing issues
- ❌ Genuine bugs (sẽ waste time retry)

---

### 5. ExtentReports

**What it does**: Generate beautiful HTML test report

**Files needed**:
- `utils/ExtentManager.java` - Report manager
- `listeners/TestListener.java` - Integration

**Output**: `reports/ExtentReport.html` với:
- Test summary dashboard
- Pass/Fail pie chart
- Screenshots attached
- Test execution timeline

---

### 6. Configuration Management

**What it does**: Externalize config values

**Files needed**:
- `resources/config.properties` - Key-value pairs
- `config/TestConfig.java` - Reader class

**Example config.properties**:
```properties
base.url=http://localhost:8080
browser=chrome
timeout.explicit=30
```

**Usage**:
```java
String url = TestConfig.get("base.url");
```

---

### 7. Explicit Waits

**What it does**: Dynamic waits thay Thread.sleep()

**Files needed**:
- `utils/WaitHelper.java` - Wrapper cho WebDriverWait

**Example**:
```java
waitHelper.waitForElementVisible(element);
waitHelper.waitForElementClickable(button);
waitHelper.waitForTextPresent(element, "Success");
```

**Why better than Thread.sleep()**:
- ✅ Wait exactly as long as needed (not fixed time)
- ✅ Fail fast if condition never met
- ✅ More stable tests

---

### 8. Parallel Execution

**What it does**: Run tests song song để save time

**Files needed**:
- `testng-parallel.xml` - Parallel configuration

**Example**:
```xml
<suite name="Parallel Suite" parallel="methods" thread-count="3">
    <!-- Tests run 3x faster -->
</suite>
```

**Considerations**:
- ⚠️ Each thread needs own WebDriver instance
- ⚠️ Shared resources must be thread-safe
- ⚠️ Database tests có thể conflict

---

## AI CHATBOT WORKFLOW

### Sau khi chọn Template:

**Step 1**: Show feature matrix cho template đã chọn
```
Với Template [X], đây là các features recommend:

REQUIRED (Must implement):
- [List required features]

OPTIONAL (Nice to have):
- [List optional features]

NOT NEEDED:
- [List not needed features]
```

**Step 2**: Discuss timeline impact
```
Implementation Timeline Estimate:

Phase 1 (Foundation): [X] hours - REQUIRED
Phase 2 (Enhancement): [Y] hours - [REQUIRED/OPTIONAL based on template]
Phase 3 (Optimization): [Z] hours - OPTIONAL

TOTAL: [X+Y+Z] hours = [N] days

Bạn có constraint về timeline không?
```

**Step 3**: Allow customization
```
Bạn có muốn:
1. Follow exact recommendation → Tiếp tục
2. Add thêm features (upgrade) → Specify which
3. Remove bớt features (downgrade) → Specify which + accept trade-offs
```

**Step 4**: Finalize feature list
```
FINAL FEATURE LIST cho dự án này:

✅ Will implement:
- [Feature 1] - [Rationale]
- [Feature 2] - [Rationale]

❌ Will skip:
- [Feature 3] - [Reason]

Bạn confirm list này chứ?
```

**Step 5**: Move to Process Guide
```
Great! Bây giờ chúng ta bắt đầu Process 4 bước.
→ File 06-Process-Step-1-Analysis.md
```

---

## VALIDATION CHECKLIST

Trước khi move sang File 06, verify:

- [ ] Features cho template đã được list rõ ràng
- [ ] Timeline estimate đã discuss
- [ ] Customization (nếu có) đã được approve
- [ ] Final feature list đã được user confirm
- [ ] Implementation order đã được hiểu

---

**⏭️ SAU KHI FINALIZE FEATURES: [06-Process-Step-1-Analysis.md](../LAYER-3-PROCESS/06-Process-Step-1-Analysis.md)**

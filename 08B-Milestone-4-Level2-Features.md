# 📝 MILESTONE 4: Level 2 Features - Enhancement (SELECTIVE)

**SEE ALSO**:
- [08-Process-Step-3-Overview.md](08-Process-Step-3-Overview.md) for principles
- [08A-Milestone-3-TAF-Core.md](08A-Milestone-3-TAF-Core.md) for M3 foundation

---

**Duration**: Varies (0 hours to 8 hours based on template and selected features)

**🎯 MỤC TIÊU M4**: Mở rộng khả năng và tối ưu hiệu suất của framework với **Phase 2 & 3 features**

**⚠️ QUAN TRỌNG**: M4 CHỈ implement các features đã được chọn trong Feature Selection Matrix (File 05)

---

## M4 - Decision Point

**Trước khi bắt đầu M4, AI PHẢI:**

```
🔍 M4 DECISION CHECKLIST:

1. Kiểm tra Template đã chọn (A/B/C/D/E)

2. Review Feature Selection Matrix cho template đó:
   - Data-Driven Testing: Required/Optional/Skip?
   - ExtentReports: Required/Optional/Skip?
   - Retry Mechanism: Required/Optional/Skip?
   - Parallel Execution: Required/Optional/Skip?

3. Xác nhận với user:
   "Dựa trên Template [X], các features recommend cho M4:

   ✅ REQUIRED (Must implement):
   - [List]

   ⭕ OPTIONAL (Nice to have):
   - [List]

   ❌ SKIP (Not needed):
   - [List]

   Bạn có muốn adjust list này không?"

4. Nếu ALL features = SKIP → SKIP M4 hoàn toàn, go to M5
```

---

## M4 - Implementation Strategy

**Process**: Implement **từng feature một**, test feature đó, rồi move next

**Thứ tự ưu tiên implement**:
1. ExtentReports (nếu Required/Optional)
2. Data-Driven Testing (nếu Required/Optional)
3. Retry Mechanism (nếu Required/Optional)
4. Parallel Execution (nếu Required/Optional)

---

## M4 - Feature 1: ExtentReports (Phase 2)

**When to implement**: Templates B, C, D, E (Required hoặc Optional)

**Duration**: 2 hours

**Files to create**:
- `utils/ExtentManager.java`
- Update `listeners/TestListener.java`

### Step 1: ExtentManager.java

```java
package utils;
import com.aventstack.extentreports.*;
import com.aventstack.extentreports.reporter.*;
import com.aventstack.extentreports.reporter.configuration.Theme;

public class ExtentManager {
    private static ExtentReports extent;
    private static ThreadLocal<ExtentTest> test = new ThreadLocal<>();

    public static ExtentReports getInstance() {
        if (extent == null) {
            ExtentSparkReporter sparkReporter = new ExtentSparkReporter("reports/ExtentReport.html");
            sparkReporter.config().setTheme(Theme.DARK);
            sparkReporter.config().setDocumentTitle("Test Automation Report");
            sparkReporter.config().setReportName("Functional Test Results");

            extent = new ExtentReports();
            extent.attachReporter(sparkReporter);
            extent.setSystemInfo("Environment", "QA");
            extent.setSystemInfo("Browser", "Chrome");
        }
        return extent;
    }

    public static ExtentTest getTest() { return test.get(); }
    public static void setTest(ExtentTest extentTest) { test.set(extentTest); }
    public static void flush() { if (extent != null) extent.flush(); }
}
```

### Step 2: Update TestListener.java

```java
// Add to TestListener.java

@Override
public void onStart(ITestContext context) {
    ExtentManager.getInstance();
    logger.info("Test suite started: {}", context.getName());
}

@Override
public void onTestStart(ITestResult result) {
    ExtentTest test = ExtentManager.getInstance()
        .createTest(result.getMethod().getMethodName());
    ExtentManager.setTest(test);
    logger.info("Test started: {}", result.getName());
}

@Override
public void onTestSuccess(ITestResult result) {
    ExtentManager.getTest().pass("Test passed");
    logger.info("Test passed: {}", result.getName());
}

@Override
public void onTestFailure(ITestResult result) {
    ExtentManager.getTest().fail(result.getThrowable());

    Object testClass = result.getInstance();
    WebDriver driver = ((tests.BaseTest) testClass).getDriver();

    if (driver != null) {
        String screenshotPath = ScreenshotHelper.capture(driver, result.getName());
        try {
            ExtentManager.getTest().addScreenCaptureFromPath(screenshotPath);
        } catch (Exception e) {
            logger.error("Failed to attach screenshot to report", e);
        }
    }
    logger.error("Test failed: {}", result.getName());
}

@Override
public void onFinish(ITestContext context) {
    ExtentManager.flush();
    logger.info("Test suite finished: {}", context.getName());
}
```

### Step 3: Test & Demo
- Run tests: `mvn test`
- Verify report generated: `reports/ExtentReport.html`
- Show user the beautiful HTML report
- Get approval

---

## M4 - Feature 2: Data-Driven Testing (Phase 2)

**When to implement**: Templates B, C, D, E

**Duration**: 2-3 hours

**Files to create**:
- `utils/DataHelper.java`
- `testdata/[entity]_testdata.json` (or .csv, .xlsx)

### Step 1: DataHelper.java

```java
package utils;
import com.fasterxml.jackson.databind.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.File;
import java.io.IOException;
import java.util.*;

public class DataHelper {
    private static final Logger logger = LoggerFactory.getLogger(DataHelper.class);

    public static Object[][] readJSON(String fileName) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            JsonNode rootNode = mapper.readTree(new File("src/test/resources/testdata/" + fileName));

            List<Object[]> dataList = new ArrayList<>();
            for (JsonNode node : rootNode) {
                Object[] row = parseJsonNode(node);
                dataList.add(row);
            }
            return dataList.toArray(new Object[0][]);
        } catch (IOException e) {
            logger.error("Failed to read JSON file: {}", fileName, e);
            return new Object[0][0];
        }
    }

    private static Object[] parseJsonNode(JsonNode node) {
        // Implement based on your test data structure
        // Example: return new Object[]{node.get("field1").asText(), node.get("field2").asText()};
        return new Object[]{};
    }
}
```

### Step 2: Create test data file

```json
// testdata/users_testdata.json
[
  {
    "name": "John Doe",
    "email": "john@example.com",
    "age": 25
  },
  {
    "name": "Jane Smith",
    "email": "jane@example.com",
    "age": 30
  }
]
```

### Step 3: Use in Test

```java
@DataProvider(name = "userData")
public Object[][] getUserData() {
    return DataHelper.readJSON("users_testdata.json");
}

@Test(dataProvider = "userData")
public void testValidSubmission(String name, String email, int age) {
    page.fillName(name);
    page.fillEmail(email);
    page.fillAge(String.valueOf(age));
    page.clickSubmit();
    Assert.assertTrue(page.getSuccessMessage().contains("successful"));
}
```

### Step 4: Test & Demo
- Run tests with multiple data sets
- Show test runs multiple times with different data
- Get approval

---

## M4 - Feature 3: Retry Mechanism (Phase 3)

**When to implement**: Templates B, D (Required), C, E (Optional)

**Duration**: 1 hour

**Files to create**: `listeners/RetryAnalyzer.java`

### Implementation

```java
package listeners;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {
    private static final Logger logger = LoggerFactory.getLogger(RetryAnalyzer.class);
    private int retryCount = 0;
    private static final int MAX_RETRY = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < MAX_RETRY) {
            retryCount++;
            logger.warn("Retrying test '{}' - Attempt {}/{}",
                result.getName(), retryCount, MAX_RETRY);
            return true;
        }
        return false;
    }
}
```

### Usage in Test

```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlakyScenario() {
    // Test will auto retry up to 2 times if fails
}
```

---

## M4 - Feature 4: Parallel Execution (Phase 3)

**When to implement**: Template B (Required), E (Optional)

**Duration**: 1-2 hours

**Files to create**: `testng-parallel.xml`

### Implementation

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Parallel Test Suite" parallel="methods" thread-count="3">
    <test name="Parallel Tests">
        <classes>
            <class name="tests.[TEST_CLASS]Test"/>
        </classes>
    </test>
</suite>
```

**Important**: Ensure WebDriver instances are thread-safe (use ThreadLocal in BaseTest)

---

## M4 - CHECKPOINT (Per Feature)

**Sau mỗi feature implementation:**

```
✅ FEATURE [X] HOÀN THÀNH: [Feature Name]

Deliverable:
✓ [Files created]

📸 DEMO:
[Show feature working]

🧪 TESTING:
[Instructions to verify]

---

❓ CHECKPOINT:

1. Feature có hoạt động đúng không?
2. Có breaking changes không?
3. Approve để tiếp tục feature tiếp theo?

✅ Reply "Approved" để continue
🔄 Request changes nếu cần
```

**Loop cho từng feature trong M4 list**

---

## M4 - FINAL CHECKPOINT

**Sau khi TẤT CẢ M4 features hoàn thành:**

```
✅ MILESTONE 4 HOÀN THÀNH: Level 2 Features

Features implemented:
✓ [List of features đã implement]

Total files added: [X] files

---

❓ FINAL M4 CHECKPOINT:

1. Tất cả features có hoạt động tốt cùng nhau không?
2. Tests vẫn pass với new features?
3. Performance có acceptable không?
4. Ready to move to M5 (Documentation)?

---

⚠️ **STOP COMMAND (CHO AI CHATBOT)**:

**DỪNG LẠI NGAY TẠI ĐÂY**.

Không được viết bất kỳ dòng code nào cho Milestone 5 (M5) cho đến khi nhận được xác nhận **"APPROVED"** chính thức từ người dùng.

---

✅ Reply "Approved" để move to M5
🔄 Request fixes nếu cần
```

---

**⏭️ NEXT**: After approval, proceed to [08C-Milestone-5-Documentation.md](08C-Milestone-5-Documentation.md)

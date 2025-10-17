# 📝 MILESTONE 3: TAF Core - Foundation

**SEE ALSO**: [08-Process-Step-3-Overview.md](08-Process-Step-3-Overview.md) for principles and framework

---

## ⚠️ PREREQUISITES - USER MUST PROVIDE (BEFORE M3)

**MANDATORY GATE: Before starting Milestone 3, verify user has provided:**

### 1. Application Access
- [ ] Deployed application URL (https://staging.example.com/...)
- [ ] Environment type (Staging/UAT/QA/Production)
- [ ] VPN requirements (if applicable)
- [ ] Test account credentials (username + password)
- [ ] Confirmation that URL is accessible

### 2. Form/Page Details
- [ ] Screenshot of target page/form
- [ ] HTML snippet (Right-click → Inspect → Copy outerHTML)
- [ ] List of all fields with IDs/names/classes
- [ ] Validation rules with **EXACT** error message text
- [ ] Expected behavior for valid/invalid inputs

### 3. Technical Context
- [ ] Authentication required? (Yes/No)
  - If YES: Login flow documented (URL, fields, credentials)
- [ ] Any dynamic elements? (AJAX loading, dropdowns populated from API)
- [ ] Network considerations (VPN, slow network, CORS)
- [ ] Browser requirements (Chrome/Firefox/Edge/Safari)

### 4. Test Scope Confirmation
- [ ] Test coverage level (Happy path only / Happy + errors / Full coverage)
- [ ] Timeline expectations (1 week / 2 weeks / flexible)
- [ ] Reporting requirements (Console logs / HTML reports / CI/CD integration)

---

### ❌ IF USER DOES NOT HAVE THESE:

**AI MUST DECLINE POLITELY:**
```
❌ Xin lỗi, tôi không thể bắt đầu implementation lúc này.

Tôi CHỈ build Test Automation Framework cho ứng dụng CÓ SẴN.
Tôi KHÔNG phải application developer.

Để tiếp tục, bạn cần:
1. Có ứng dụng/website đã deployed tại URL thực
2. Cung cấp thông tin access (URL, credentials, VPN)
3. Cung cấp chi tiết form/page cần test

Sau khi có đầy đủ thông tin trên, quay lại và tôi sẽ giúp bạn build TAF.

→ STOP - Không tiếp tục Milestone 3
```

---

### ✅ IF USER HAS ALL PREREQUISITES:

**AI ACKNOWLEDGES:**
```
✅ Tốt! Bạn đã có application sẵn sàng.

Thông tin nhận được:
- Application URL: [URL_provided]
- Environment: [staging/UAT/...]
- Authentication: [Required/Not required]
- Target form/page: [Description]

Tôi sẽ bắt đầu build Test Automation Framework.

→ Tiếp tục MILESTONE 3: TAF Core - Foundation
```

---

## MILESTONE 3: TAF Core - Foundation

**Duration**: 0.5-2 days (Core Classes) + 3 hours (Phase 1 Features)

**🎯 MỤC TIÊU M3**: Xây dựng **Test Automation Framework cốt lõi ổn định và có thể debug**

**Deliverables - Chia làm 2 nhóm:**

**Nhóm A: Phase 1 Features (FOUNDATION - BẮT BUỘC)**
- ✅ **Configuration Management**: `config/TestConfig.java`, `resources/config/config.properties`
- ✅ **Logging Framework**: `resources/logging/logback.xml`, SLF4J usage
- ✅ **Explicit Waits**: `utils/WaitHelper.java`
- ✅ **Screenshot on Failure**: `utils/ScreenshotHelper.java`, `listeners/TestListener.java`

**Nhóm B: Core Classes (POM + Test Structure)**
- BasePage.java, BaseTest.java
- [PAGE_NAME]Page.java, [PAGE_NAME]Test.java
- testng.xml configuration

**Tổng số files M3**: 10-12 files

---

### M3 - Phase 1: Planning

**AI announces**:
```
🎯 MILESTONE 3: TAF Core - Foundation

Tôi sẽ tạo:

NHÓM A: Phase 1 Features (FOUNDATION)
1. Configuration Management: config/TestConfig.java, config.properties
2. Logging Framework: logback.xml + SLF4J
3. Explicit Waits: utils/WaitHelper.java
4. Screenshot on Failure: utils/ScreenshotHelper.java, listeners/TestListener.java

NHÓM B: Core Classes
5. pages/BasePage.java
6. tests/BaseTest.java
7. pages/[PAGE_NAME]Page.java
8. tests/[TEST_CLASS]Test.java
9. testng.xml

Starting implementation...
```

---

### M3 - Phase 2: Implementation

**Step 1: Configuration Management**

`config/TestConfig.java`:
```java
package config;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class TestConfig {
    private static Properties properties = new Properties();
    static {
        try {
            FileInputStream fis = new FileInputStream("src/test/resources/config/config.properties");
            properties.load(fis);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static String get(String key) { return properties.getProperty(key); }
    public static int getInt(String key) { return Integer.parseInt(properties.getProperty(key)); }
}
```

`resources/config/config.properties`:
```properties
# Application URLs (REAL deployed URLs - NOT localhost)
app.base.url=https://staging.example.com
app.login.path=/login
app.target.path=/registration

# Test Credentials
test.username=testuser@example.com
test.password=Test@1234

# Browser & Timeouts
browser=chrome
headless=false
timeout.explicit=30
timeout.implicit=0
timeout.page.load=60

# Screenshots
screenshot.on.failure=true
screenshot.path=screenshots/

# Environment
environment=staging
```

---

**Step 2: Logging Framework**

`resources/logging/logback.xml`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>logs/automation.log</file>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

---

**Step 3: Explicit Waits**

`utils/WaitHelper.java`:
```java
package utils;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import config.TestConfig;
import java.time.Duration;

public class WaitHelper {
    private static final Logger logger = LoggerFactory.getLogger(WaitHelper.class);
    private WebDriverWait wait;

    public WaitHelper(WebDriver driver) {
        int timeout = TestConfig.getInt("timeout.explicit");
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(timeout));
    }

    public void waitForElementVisible(WebElement element) {
        logger.debug("Waiting for element to be visible");
        wait.until(ExpectedConditions.visibilityOf(element));
    }

    public void waitForElementClickable(WebElement element) {
        logger.debug("Waiting for element to be clickable");
        wait.until(ExpectedConditions.elementToBeClickable(element));
    }

    public void waitForTextPresent(WebElement element, String text) {
        logger.debug("Waiting for text '{}' in element", text);
        wait.until(ExpectedConditions.textToBePresentInElement(element, text));
    }
}
```

---

**Step 4: Screenshot on Failure**

`utils/ScreenshotHelper.java`:
```java
package utils;
import org.openqa.selenium.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import config.TestConfig;
import java.io.File;
import java.io.IOException;
import java.nio.file.*;
import java.text.SimpleDateFormat;
import java.util.Date;

public class ScreenshotHelper {
    private static final Logger logger = LoggerFactory.getLogger(ScreenshotHelper.class);

    public static String capture(WebDriver driver, String testName) {
        if (!TestConfig.get("screenshot.on.failure").equals("true")) return null;

        String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String fileName = testName + "_" + timestamp + ".png";
        String path = TestConfig.get("screenshot.path") + fileName;

        try {
            File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
            Files.createDirectories(Paths.get(TestConfig.get("screenshot.path")));
            Files.copy(screenshot.toPath(), Paths.get(path));
            logger.info("Screenshot captured: {}", path);
            return path;
        } catch (IOException e) {
            logger.error("Failed to capture screenshot", e);
            return null;
        }
    }
}
```

`listeners/TestListener.java`:
```java
package listeners;
import org.openqa.selenium.WebDriver;
import org.testng.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import utils.ScreenshotHelper;

public class TestListener implements ITestListener {
    private static final Logger logger = LoggerFactory.getLogger(TestListener.class);

    @Override
    public void onTestStart(ITestResult result) {
        logger.info("Test started: {}", result.getName());
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        logger.info("Test passed: {}", result.getName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        logger.error("Test failed: {}", result.getName());
        Object testClass = result.getInstance();
        WebDriver driver = ((tests.BaseTest) testClass).getDriver();
        if (driver != null) {
            String screenshotPath = ScreenshotHelper.capture(driver, result.getName());
            logger.info("Screenshot saved at: {}", screenshotPath);
        }
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        logger.warn("Test skipped: {}", result.getName());
    }
}
```

---

**Step 5: BasePage.java**

`pages/BasePage.java`:
```java
package pages;
import org.openqa.selenium.WebDriver;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import utils.WaitHelper;

public abstract class BasePage {
    protected static final Logger logger = LoggerFactory.getLogger(BasePage.class);
    protected WebDriver driver;
    protected WaitHelper waitHelper;

    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.waitHelper = new WaitHelper(driver);
        logger.debug("Initialized {}", this.getClass().getSimpleName());
    }

    public void navigateTo(String url) {
        logger.info("Navigating to: {}", url);
        driver.get(url);
    }

    public String getCurrentUrl() { return driver.getCurrentUrl(); }
    public String getTitle() { return driver.getTitle(); }
}
```

---

**Step 6: BaseTest.java**

`tests/BaseTest.java`:
```java
package tests;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.testng.annotations.*;
import config.TestConfig;
import listeners.TestListener;

@Listeners(TestListener.class)
public abstract class BaseTest {
    protected static final Logger logger = LoggerFactory.getLogger(BaseTest.class);
    protected WebDriver driver;

    @BeforeClass
    public void setup() {
        logger.info("Setting up test environment");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        logger.info("Browser launched: Chrome");
    }

    @AfterClass
    public void teardown() {
        if (driver != null) {
            logger.info("Closing browser");
            driver.quit();
        }
    }

    public WebDriver getDriver() { return driver; }
}
```

---

**Step 7: Page Object Example**

`pages/[PAGE_NAME]Page.java`:
```java
public class [PAGE_NAME]Page extends BasePage {
    @FindBy(id = "field1") private WebElement field1Input;
    @FindBy(id = "field2") private WebElement field2Input;
    @FindBy(css = "button[type='submit']") private WebElement submitButton;
    @FindBy(css = ".alert-success") private WebElement successMessage;

    public [PAGE_NAME]Page(WebDriver driver) {
        super(driver);
        PageFactory.initElements(driver, this);
    }

    public void fillField1(String value) {
        waitHelper.waitForElementVisible(field1Input);
        field1Input.clear();
        field1Input.sendKeys(value);
    }

    public void clickSubmit() {
        waitHelper.waitForElementClickable(submitButton);
        submitButton.click();
    }

    public String getSuccessMessage() {
        waitHelper.waitForElementVisible(successMessage);
        return successMessage.getText();
    }
}
```

`tests/[PAGE_NAME]Test.java`:
```java
public class [PAGE_NAME]Test extends BaseTest {
    private [PAGE_NAME]Page page;

    @BeforeClass
    public void setup() {
        driver = new ChromeDriver();
        page = new [PAGE_NAME]Page(driver);
        page.navigateTo(TestConfig.get("base.url") + "/[form-path]");
    }

    @Test(priority = 1)
    public void testValidSubmission() {
        page.fillField1("value1");
        page.fillField2("value2");
        page.clickSubmit();
        Assert.assertEquals(page.getSuccessMessage(), "Registration successful!");
    }

    @Test(priority = 2)
    public void testMissingRequiredField() {
        page.fillField2("value2");
        page.clickSubmit();
        String error = page.getErrorMessage("field1");
        Assert.assertTrue(error.contains("required"));
    }
}
```

---

### M3 - Phase 3: Self-QA

**Checklist for AI to verify**:
```
M3 Self-QA Checklist:

NHÓM A - Phase 1 Features:
[ ] Config.properties được load thành công
[ ] Logs xuất hiện trong logs/automation.log
[ ] WaitHelper methods hoạt động
[ ] Screenshot captured khi test fail
[ ] TestListener hooks executing correctly

NHÓM B - Core Classes & Tests:
[ ] TAF project compiles (mvn clean compile)
[ ] Remote application accessible at provided URL
[ ] VPN connected if required
[ ] Tests run successfully (mvn test)
[ ] At least 2-3 tests pass (>80% pass rate)
[ ] No hard-coded values (URLs, timeouts from config)

CRITICAL VERIFICATION:
[ ] Tất cả 4 Phase 1 features implemented và working
[ ] Framework core ổn định và debuggable
[ ] Connection to remote application working
[ ] Authentication flow working (if required)
```

---

### M3 - CHECKPOINT

```
✅ MILESTONE 3 HOÀN THÀNH: TAF Core - Foundation

Deliverables:

NHÓM A - Phase 1 Features (FOUNDATION):
✓ Configuration Management (TestConfig.java, config.properties)
✓ Logging Framework (logback.xml, SLF4J integrated)
✓ Explicit Waits (WaitHelper.java)
✓ Screenshot on Failure (ScreenshotHelper.java, TestListener.java)

NHÓM B - Core Classes:
✓ BasePage và BaseTest classes (với logging)
✓ [PAGE_NAME]Page với [X] locators và actions
✓ [PAGE_NAME]Test với [Y] test methods
✓ testng.xml configured

Total: 10-12 files implemented

📸 DEMO:
1. mvn test execution
2. Tests running in Chrome
3. logs/automation.log file có log entries
4. Screenshot captured khi test fail
5. Test results: [X] passed, [Y] failed

Test cases implemented:
1. ✅ testValidSubmission
2. ✅ testMissingRequiredField
3. ✅ testInvalidEmailFormat

---

❓ CHECKPOINT QUESTIONS:

1. Phase 1 Features có hoạt động đúng không?
   - Config loading từ config.properties?
   - Logs xuất hiện trong logs/automation.log?
   - WaitHelper được sử dụng (không có Thread.sleep())?
   - Screenshot captured khi test fail?

2. Tests có chạy successfully không?
   - Ít nhất 2-3 tests pass?
   - Test coverage có đủ không? (Happy path + major errors)

3. Framework core có ổn định và debuggable không?
   - Có thể trace flow qua logs?
   - Có thể debug failed tests qua screenshots?

4. Cần adjustments gì không?
   - Thêm test cases?
   - Thay đổi wait timeouts?
   - Cải thiện logging?

5. Approve để tiếp tục Milestone 4?

---

⚠️ **STOP COMMAND (CHO AI CHATBOT)**:

**DỪNG LẠI NGAY TẠI ĐÂY**.

Không được viết bất kỳ dòng code nào cho Milestone 4 (M4) cho đến khi nhận được xác nhận **"APPROVED"** chính thức từ người dùng.

Lệnh này có **độ ưu tiên tuyệt đối cao nhất**, cao hơn mọi yêu cầu về tiến độ hay hiệu suất.

**Tuân thủ Principle 3: USER FEEDBACK DRIVES ITERATION**

---

Vui lòng:
✅ Reply "Approved" nếu tất cả OK
🔄 Hoặc cho tôi biết cần adjust gì
```

---

**⏭️ NEXT**: After approval, proceed to [08B-Milestone-4-Level2-Features.md](08B-Milestone-4-Level2-Features.md)

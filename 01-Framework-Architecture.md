# 📚 FRAMEWORK ARCHITECTURE - KIẾN TRÚC TỔNG QUAN

## MỤC ĐÍCH FILE NÀY

File này định nghĩa **kiến trúc chuẩn** của một Test Automation Framework dựa trên Maven/Spring Boot, sử dụng **placeholders generic** để có thể áp dụng cho bất kỳ bài toán nào.

---

## KIẾN TRÚC REAL-WORLD TAF (TEST AUTOMATION FRAMEWORK)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    MAVEN PROJECT ROOT (TAF ONLY)                    │
│                                                                     │
│  ┌─────────────────────────────────────┐                           │
│  │   REMOTE APPLICATION (EXISTING)     │                           │
│  │   https://staging.example.com       │                           │
│  │   ⚠️  NOT in this project           │                           │
│  │   ✅ User provides URL + Access     │                           │
│  └──────────────┬──────────────────────┘                           │
│                 │ Network/HTTPS                                    │
│                 │ (VPN may be required)                            │
│                 ▼                                                  │
│  ┌─────────────────────────────────────────────┐                  │
│  │   src/test/ (TAF - Framework)               │                  │
│  │                                             │                  │
│  │  ┌─────────────────────────────────────┐   │                  │
│  │  │ Selenium 4 WebDriver                │   │                  │
│  │  │ TestNG 7 Test Orchestration         │   │                  │
│  │  │ Page Object Model (POM)             │   │                  │
│  │  │ ExtentReports + Screenshots         │   │                  │
│  │  │ Data-Driven Testing                 │   │                  │
│  │  │ Configuration Management            │   │                  │
│  │  └─────────────────────────────────────┘   │                  │
│  └─────────────────────────────────────────────┘                  │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │  OUTPUT: reports/, logs/, screenshots/                      │  │
│  └─────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

**⚠️ CRITICAL NOTE:**
- This framework does **NOT** create applications
- Application must **ALREADY EXIST** at a deployed URL
- We **ONLY** build the Test Automation Framework (src/test/)
- Application runs on **REMOTE** server (staging/UAT/production)

---

## CẤU TRÚC THƯ MỤC GENERIC (TAF ONLY)

⚠️ **NOTE:** This structure contains **ONLY** Test Automation Framework code.
**NO** application code (src/main/) is present in this project.

```
{{PROJECT_NAME}}/
├── reports/                            # Test Reports Output
│   ├── ExtentReport.html
│   └── testng-results.xml
│
├── logs/                               # Log Files Output
│   └── automation.log
│
├── screenshots/                        # Failed Test Screenshots
│   └── {{TEST_NAME}}_{{TIMESTAMP}}.png
│
├── src/
│   └── test/                          # ═══════════════════════
│       │                              # TEST AUTOMATION FRAMEWORK
│       │                              # ═══════════════════════
│       ├── java/
│       │   ├── config/
│       │   │   └── TestConfig.java             # Configuration loader
│       │   │
│       │   ├── listeners/                      # TestNG listeners
│       │   │   ├── RetryAnalyzer.java          # Auto retry flaky tests
│       │   │   ├── TestListener.java           # ExtentReports integration
│       │   │   └── SuiteListener.java          # Suite-level hooks
│       │   │
│       │   ├── pages/                          # Page Object Model
│       │   │   ├── BasePage.java               # Base page class
│       │   │   ├── LoginPage.java              # Login page (if auth required)
│       │   │   └── {{PAGE_NAME}}Page.java      # Page objects for target forms
│       │   │
│       │   ├── tests/                          # Test classes
│       │   │   ├── BaseTest.java               # Base test setup/teardown
│       │   │   └── {{TEST_CLASS}}Test.java     # Actual test scenarios
│       │   │
│       │   └── utils/                          # Helper utilities
│       │       ├── WaitHelper.java             # Explicit waits wrapper
│       │       ├── DataHelper.java             # Test data parsing (JSON/CSV/Excel)
│       │       ├── ScreenshotHelper.java       # Screenshot capture
│       │       ├── DateHelper.java             # Date/time utilities
│       │       └── ExtentManager.java          # ExtentReports manager
│       │
│       └── resources/
│           ├── config/
│           │   └── config.properties           # App URLs, browser config
│           │
│           ├── logging/
│           │   └── logback.xml                 # SLF4J logging config
│           │
│           ├── testdata/                       # Test data files
│           │   ├── {{DATA_FILE}}.json          # JSON test data
│           │   ├── {{DATA_FILE}}.csv           # CSV test data
│           │   └── {{DATA_FILE}}.xlsx          # Excel test data
│           │
│           └── testng/
│               ├── testng.xml                  # Sequential test execution
│               └── testng-parallel.xml         # Parallel test execution
│
├── pom.xml                             # Maven dependencies (TAF only)
├── README.md                           # Setup & execution guide
└── .gitignore
```

**Key Changes from Traditional Setup:**
- ❌ **NO src/main/** directory - Application runs remotely
- ✅ **ONLY src/test/** directory - TAF code only
- ✅ **config.properties** points to REMOTE URLs (https://staging.example.com)
- ✅ **LoginPage.java** often required for authentication
- ✅ **pom.xml** contains ONLY test framework dependencies

---

## PLACEHOLDERS VÀ CÁCH SỬ DỤNG

| Placeholder | Ý nghĩa | Ví dụ thay thế |
|:------------|:--------|:---------------|
| `{{PROJECT_NAME}}` | Tên dự án | MyTestAutomation, EcommerceTestFramework |
| `{{ENTITY}}` | Đối tượng nghiệp vụ | Student, User, Product, Order |
| `{{FORM_NAME}}` | Tên form cần test | registration, login, checkout, search |
| `{{PAGE_NAME}}` | Tên page object | Registration, Login, Dashboard |
| `{{TEST_CLASS}}` | Tên test class | Registration, Login, Checkout |
| `{{DATA_FILE}}` | Tên file test data | students, users, products |

### Quy tắc đặt tên:
- **Controller/Service/Repository**: `{{ENTITY}}` + suffix → `UserController`, `ProductService`
- **Page Objects**: `{{PAGE_NAME}}` + "Page" → `RegistrationPage`, `LoginPage`
- **Test Classes**: `{{TEST_CLASS}}` + "Test" → `RegistrationTest`, `LoginTest`
- **Data Files**: Số nhiều của entity → `students.json`, `users.csv`

---

## MÔ TẢ CHỨC NĂNG CÁC PACKAGE

⚠️ **NOTE:** We do NOT build application layers (Controller, Model, Repository, Service).
Those exist in the REMOTE application. We ONLY build the Test Automation Framework packages below.

### SRC/TEST - TEST AUTOMATION FRAMEWORK

#### 1. Config Package
**Vai trò**: Configuration Management  
**File**: `TestConfig.java`  
**Chức năng**: Đọc config.properties, cung cấp typed methods

#### 2. Listeners Package
**Vai trò**: TestNG Event Hooks  
**Files**: 
- `RetryAnalyzer.java` - Auto retry flaky tests
- `TestListener.java` - ExtentReports + Screenshot capture
- `SuiteListener.java` - Suite-level initialization

#### 3. Pages Package (Page Object Model)
**Vai trò**: Abstraction cho UI elements  
**Base Class**: `BasePage.java`  
**Page Classes**: Một class cho mỗi page cần test

**Template BasePage**:
```java
public abstract class BasePage {
    protected WebDriver driver;
    protected WaitHelper waitHelper;
    
    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.waitHelper = new WaitHelper(driver, 30);
    }
    
    // Common actions: navigate, getCurrentUrl, getTitle
}
```

**Template Page Object**:
```java
public class {{PAGE_NAME}}Page extends BasePage {
    // Locators
    @FindBy(id = "{{field_id}}")
    private WebElement {{fieldName}}Field;
    
    // Constructor
    public {{PAGE_NAME}}Page(WebDriver driver) {
        super(driver);
        PageFactory.initElements(driver, this);
    }
    
    // Actions
    public void fill{{FieldName}}(String value) { ... }
    public void clickSubmit() { ... }
    
    // Getters for verification
    public String getSuccessMessage() { ... }
}
```

#### 4. Tests Package
**Vai trò**: Test Scenarios  
**Base Class**: `BaseTest.java`  
**Test Classes**: Một class cho mỗi feature/form

**Template BaseTest**:
```java
public abstract class BaseTest {
    protected WebDriver driver;
    protected {{PAGE_NAME}}Page page;
    
    @BeforeClass
    public void setup() {
        driver = new ChromeDriver();
        page = new {{PAGE_NAME}}Page(driver);
    }
    
    @AfterClass
    public void teardown() {
        if (driver != null) driver.quit();
    }
}
```

**Template Test Class**:
```java
public class {{TEST_CLASS}}Test extends BaseTest {
    @Test(priority = 1)
    public void testValid{{Action}}() {
        // Arrange
        // Act
        // Assert
    }
    
    @Test(priority = 2, dataProvider = "invalidData")
    public void testInvalid{{Action}}(String input) {
        // Test với invalid data
    }
}
```

#### 5. Utils Package
**Vai trò**: Reusable Utilities  
**Files**:
- `WaitHelper.java` - Explicit waits wrapper
- `DataHelper.java` - JSON/CSV/Excel parsing
- `ScreenshotHelper.java` - Screenshot capture
- `ExtentManager.java` - ExtentReports manager

---

## OUTPUT DIRECTORIES

### Reports/
Chứa test reports:
- `ExtentReport.html` - ExtentReports HTML report (beautiful UI)
- `testng-results.xml` - TestNG native XML report

### Logs/
Chứa application logs:
- `automation.log` - SLF4J logs (INFO, DEBUG, ERROR levels)

### Screenshots/
Chứa screenshots của failed tests:
- Format: `{{TestName}}_{{Timestamp}}.png`
- Tự động capture bởi TestListener

---

## MAVEN POM.XML STRUCTURE (TAF ONLY)

⚠️ **CRITICAL:** This pom.xml contains **ONLY** Test Automation Framework dependencies.
**NO** Spring Boot, H2, or application dependencies.

### Dependencies Categories

**1. Test Framework Core** ⭐ MANDATORY
```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.16.1</version>
</dependency>
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.6.2</version>
</dependency>
```

**2. ExtentReports** ⭐ HIGHLY RECOMMENDED
```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.1.1</version>
</dependency>
```

**3. Logging** ⭐ RECOMMENDED
```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.9</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.4.14</version>
</dependency>
```

**4. Data-Driven Testing** (Optional - based on Feature Selection Matrix)
```xml
<!-- JSON -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.16.0</version>
</dependency>

<!-- CSV -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.10.0</version>
</dependency>

<!-- Excel -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.2.5</version>
</dependency>
```

**5. API Testing** (Optional - if testing REST APIs)
```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>5.4.0</version>
    <scope>test</scope>
</dependency>
```

**❌ REMOVED (Not needed for TAF):**
- ~~Spring Boot starters~~ (Application runs remotely)
- ~~H2 database~~ (Database is on remote server)
- ~~Thymeleaf~~ (Frontend already exists)
- ~~Spring Data JPA~~ (We don't manage database)

---

## WORKFLOW EXECUTION DIAGRAM

```
┌─────────────────────────────────────────────────────────────┐
│              TEST EXECUTION LIFECYCLE                       │
└─────────────────────────────────────────────────────────────┘

1. TestNG Suite Start
   │
   ├─► [SuiteListener] @BeforeSuite
   │   ├─ Initialize ExtentReports
   │   ├─ Load config.properties
   │   └─ Setup global resources
   │
   ▼
2. Test Class Initialization
   │
   ├─► [BaseTest] @BeforeClass
   │   ├─ Initialize WebDriver
   │   ├─ Setup WaitHelper
   │   └─ Load test data (DataHelper)
   │
   ▼
3. Test Method Execution
   │
   ├─► [TestListener] @BeforeMethod
   │   ├─ Start ExtentTest
   │   └─ Log test start
   │
   ├─► [{{TEST_CLASS}}Test] @Test
   │   ├─ Navigate to page (BasePage)
   │   ├─ Perform actions ({{PAGE_NAME}}Page)
   │   ├─ Apply explicit waits (WaitHelper)
   │   └─ Assert results
   │   │
   │   ├─ [IF FAIL] ──► [RetryAnalyzer]
   │   │                 └─ Retry test (max 2 times)
   │   │
   │   └─ [AFTER TEST] ──► [TestListener] @AfterMethod
   │                        ├─ Check test result
   │                        ├─ [IF FAIL] Capture screenshot
   │                        ├─ Log to SLF4J
   │                        └─ Update ExtentReports
   │
   ▼
4. Test Class Cleanup
   │
   ├─► [BaseTest] @AfterClass
   │   └─ Quit WebDriver
   │
   ▼
5. Suite Finalization
   │
   └─► [SuiteListener] @AfterSuite
       ├─ Flush ExtentReports
       ├─ Generate final report
       └─ Cleanup resources
```

---

## CUSTOMIZATION GUIDE

### Khi áp dụng cho bài toán mới:

⚠️ **IMPORTANT:** User must provide EXISTING application URL first.

**Bước 1**: Verify Prerequisites
- [ ] User has provided deployed application URL (https://...)
- [ ] Application is accessible (VPN connected if needed)
- [ ] Test credentials available (if login required)
- [ ] User confirmed application ALREADY EXISTS

**Bước 2**: Xác định placeholders từ EXISTING application
- `{{PAGE_NAME}}` = ? (inspect existing pages - ví dụ: Login, Checkout, Dashboard)
- `{{FORM_NAME}}` = ? (identify forms to test - ví dụ: login, registration)
- `{{ENTITY}}` = ? (business domain from existing app - ví dụ: User, Product)

**Bước 3**: Setup TAF project structure (src/test ONLY)
- Create Maven project with TAF dependencies only
- Create directory structure: pages/, tests/, utils/, config/
- NO src/main/ directory needed

**Bước 4**: Configure connection to REMOTE app
- Setup config.properties with REAL URLs:
  ```properties
  app.base.url=https://staging.example.com
  app.login.path=/login
  app.target.path=/registration
  browser=chrome
  explicit.wait=15
  ```
- Setup logback.xml for logging
- Setup testng.xml for test execution

**Bước 5**: Implement TAF (src/test) for EXISTING app
- Create LoginPage.java (if authentication required)
- Create {{PAGE_NAME}}Page.java for target pages
- Write Test classes with test scenarios
- Use Explicit Waits for network latency

**Bước 6**: Run tests against REMOTE application
- Ensure VPN connected (if required)
- Run: `mvn test -Dsurefire.suiteXmlFiles=testng.xml`
- Review ExtentReports for results

**❌ DO NOT:**
- Create src/main/ code
- Build application/controllers/entities
- Use localhost URLs

---

## INTEGRATION POINTS

### REMOTE APP ↔ TAF Communication

⚠️ **CRITICAL:** Application runs on REMOTE server, not localhost.

**Point 1: Application URL**
```
Remote App: https://staging.example.com/{{form-path}}
TAF config.properties:
    app.base.url=https://staging.example.com
    app.form.path=/{{form-path}}

TAF access:
    String fullUrl = TestConfig.getBaseUrl() + TestConfig.getFormPath();
    driver.get(fullUrl);
```

**Point 2: Authentication Flow** ⭐ NEW
```
Challenge: Most real-world apps require login first

TAF Solution:
1. Create LoginPage.java with login credentials
2. @BeforeClass: Login once per test class
3. Store session/cookies for subsequent tests

Example:
    @BeforeClass
    public void setup() {
        driver = new ChromeDriver();
        LoginPage loginPage = new LoginPage(driver);
        loginPage.login(username, password);
        // Now authenticated for all tests in this class
    }
```

**Point 3: HTML Elements**
```
Remote App HTML: <input id="{{field_id}}" name="{{field_name}}" />
TAF Page Object:
    @FindBy(id = "{{field_id}}")
    private WebElement field;

TAF locates element using Selenium WebDriver.
```

**Point 4: Validation Messages**
```
Remote App response: <div class="error">{{error_message}}</div>
TAF verification:
    String actual = page.getErrorMessage();
    Assert.assertEquals(actual, expected);
```

**Point 5: Network Latency** ⭐ NEW
```
Challenge: Remote app slower than localhost (network latency, VPN)

TAF Solution:
- Use Explicit Waits (NOT Thread.sleep())
- Example:
    waitHelper.waitForElementVisible(errorMessageElement, 10);

- Configure longer timeouts in config.properties:
    explicit.wait=15  # seconds
    page.load.timeout=30  # seconds
```

**Point 6: VPN/Network Access** ⭐ NEW
```
Challenge: Staging/UAT environments may require VPN

TAF Prerequisites:
1. User must connect to VPN BEFORE running tests
2. Document VPN requirements in README.md
3. Test should FAIL FAST if URL unreachable:

    try {
        driver.get(url);
    } catch (TimeoutException e) {
        throw new RuntimeException("Cannot access " + url +
            ". Check VPN connection and URL accessibility.");
    }
```

---

## LƯU Ý QUAN TRỌNG

### Khi đọc file này, AI Chatbot cần:

✅ **Hiểu rằng**: Đây là template GENERIC cho TAF (Test Automation Framework) ONLY
✅ **CRITICAL**: NEVER build src/main/ code - Application MUST exist remotely
✅ **Phải verify URL**: User MUST provide deployed application URL before starting
✅ **Thay thế placeholders** bằng tên từ EXISTING application (inspect real pages)
✅ **Không copy-paste** ví dụ Student/Registration - use REAL entity names from user's app
✅ **Authentication first**: If app requires login, implement LoginPage.java FIRST

### Khi customize:
- ❌ **NEVER create src/main/** directory
- ✅ **ONLY create src/test/** directory
- ✅ Giữ nguyên cấu trúc thư mục TAF
- ✅ Giữ nguyên naming conventions
- ✅ Thay placeholders bằng tên từ EXISTING application
- ✅ Use REAL URLs (https://staging...), NEVER localhost
- ✅ Implement authentication flow if required
- ✅ Use Explicit Waits for network latency

### Real-World vs Practice Mode:

| Aspect | ❌ Practice Mode (OLD) | ✅ Real-World Mode (CURRENT) |
|--------|------------------------|------------------------------|
| **Application** | Build from scratch | MUST exist remotely |
| **src/main/** | Created | NEVER created |
| **URLs** | localhost:8080 | https://staging.example.com |
| **Timeline** | 4-6 weeks (M1+M2+M3-M5) | 1-2 weeks (M3-M5 only) |
| **Prerequisites** | None | URL + Access + Credentials |
| **Authentication** | Optional | Usually required |
| **Network** | Local (fast) | Remote (use Explicit Waits) |

---

**⏭️ FILE TIẾP THEO: [02-Technology-Stack.md](02-Technology-Stack.md)**

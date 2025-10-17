# 🔧 TROUBLESHOOTING GUIDE

## 🎯 MỤC ĐÍCH FILE NÀY

File này là **solution catalog** toàn diện cho các lỗi thường gặp trong Test Automation Framework, giúp:
- **Giải quyết nhanh** các issues phổ biến trong setup và execution
- **Hướng dẫn debug** systematic khi gặp lỗi mới
- **Tham khảo** common solutions đã được verify
- **Tiết kiệm thời gian** troubleshooting

**Khi nào dùng file này:**
- Khi gặp error trong quá trình setup môi trường
- Khi tests fail với exceptions không rõ nguyên nhân
- Khi CI/CD pipeline fails
- Khi cần debug performance issues
- Trước khi escalate issue lên team lead

**Cách sử dụng file này:**
1. **Xác định category** của error (setup/runtime/CI/performance)
2. **Tìm section** tương ứng trong TOC
3. **Match error message** với danh sách issues
4. **Follow solution** step-by-step
5. **Verify fix** bằng verification commands
6. **Document** nếu gặp issue mới chưa có trong guide

---

## 📖 TABLE OF CONTENTS

### Quick Navigation by Error Phase

| Phase | Section | Issues Covered |
|-------|---------|----------------|
| **Setup** | [1. Setup & Environment](#1%EF%B8%8F⃣-setup--environment-issues) | 7 issues |
| **Build** | [2. Dependency & Build](#2%EF%B8%8F⃣-dependency--build-issues) | 8 issues |
| **Runtime** | [3. Runtime Errors](#3%EF%B8%8F⃣-runtime-errors) | 9 issues |
| **Application** | [4. Application Issues](#4%EF%B8%8F⃣-application-issues-aut) | 6 issues |
| **Test Execution** | [5. Test Execution](#5%EF%B8%8F⃣-test-execution-issues) | 8 issues |
| **Reporting** | [6. Reporting & Logging](#6%EF%B8%8F⃣-reporting--logging-issues) | 5 issues |
| **Performance** | [7. Performance](#7%EF%B8%8F⃣-performance-issues) | 4 issues |
| **Browser** | [8. Browser-Specific](#8%EF%B8%8F⃣-browser-specific-issues) | 5 issues |
| **Data** | [9. Data Management](#9%EF%B8%8F⃣-data-management-issues) | 3 issues |

### Advanced Sections

- [🔍 Troubleshooting Decision Tree](#-troubleshooting-decision-tree)
- [🛠️ Debugging Techniques](#️-debugging-techniques)
- [🚨 Emergency Troubleshooting Workflow](#-emergency-troubleshooting-workflow)
- [📚 Getting Help](#-getting-help)

### Quick Search Index

**Common Error Messages:**
- `NoSuchElementException` → [Issue 3.1](#issue-31-nosuchelementexception)
- `ElementClickInterceptedException` → [Issue 3.2](#issue-32-elementclickinterceptedexception)
- `StaleElementReferenceException` → [Issue 3.3](#issue-33-staleelementreferenceexception)
- `TimeoutException` → [Issue 3.4](#issue-34-timeoutexception)
- `WebDriverException: chrome not reachable` → [Issue 8.1](#issue-81-chrome-crashes-or-not-reachable)
- `Port already in use` → [Issue 4.1](#issue-41-port-already-in-use)
- `ClassNotFoundException` → [Issue 2.1](#issue-21-maven-dependency-not-resolved)
- `Test ignored` → [Issue 5.3](#issue-53-tests-run-but-all-ignored)

---

## 🔍 TROUBLESHOOTING DECISION TREE

```
┌─────────────────────────────────┐
│    ERROR OCCURRED?              │
└────────────┬────────────────────┘
             │
    ┌────────┴────────┐
    │  WHICH PHASE?   │
    └────────┬────────┘
             │
   ┌─────────┼─────────────────────────────────┐
   │         │                                  │
┌──▼──┐  ┌──▼────┐  ┌─────▼──────┐  ┌────▼────┐
│SETUP│  │ BUILD │  │  RUNTIME   │  │  CI/CD  │
└──┬──┘  └───┬───┘  └─────┬──────┘  └────┬────┘
   │         │             │               │
   │         │             │               │
┌──▼─────────────┐  ┌─────▼──────────┐  ┌▼─────────────┐
│ Java not found │  │ Element errors │  │ Tests fail   │
│ Maven errors   │  │ Timeout errors │  │ in pipeline  │
│ IDE issues     │  │ Click errors   │  │ only         │
│                │  │ Wait errors    │  │              │
│ → Section 1    │  │ → Section 3    │  │ → Section 5  │
└────────────────┘  └────────────────┘  └──────────────┘

┌──────────────────────────────────────────────────┐
│  DETAILED NAVIGATION BY SYMPTOM                  │
└──────────────────────────────────────────────────┘

ERROR SYMPTOM                          → SECTION
─────────────────────────────────────────────────────
Java/Maven not found                   → 1.1, 1.2
Project won't import in IDE            → 1.3
Dependencies won't download            → 2.1
WebDriver not found                    → 2.2, 2.3
Compilation errors                     → 2.4
Element not found                      → 3.1
Element not clickable                  → 3.2
Element becomes stale                  → 3.3
Wait timeouts                          → 3.4
Application won't start                → 4.1, 4.2
H2 console not accessible              → 4.2
Spring Boot errors                     → 4.3
Tests are flaky                        → 5.1
CI/CD failures                         → 5.2
Tests ignored/skipped                  → 5.3
Reports not generated                  → 6.1
Screenshots missing                    → 6.2
Logs missing                           → 6.3
Tests too slow                         → 7.1
Memory issues                          → 7.2
Chrome crashes                         → 8.1
Firefox issues                         → 8.2
Headless mode problems                 → 8.3
Test data not loading                  → 9.1
Database connection fails              → 9.2
```

---

## 1️⃣ SETUP & ENVIRONMENT ISSUES

### Issue 1.1: Java Not Found or Wrong Version
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
'java' is not recognized as an internal or external command
# Or
Error: Could not find or load main class
# Or
java.lang.UnsupportedClassVersionError: ... version 65.0
```

**Root Cause:**
- Java không được cài đặt
- Java không có trong PATH environment variable
- Sai version (cần Java 21, nhưng đang dùng Java 11/17)

**Solution:**

**Bước 1: Verify Java installation**
```bash
java -version
javac -version
```

**Bước 2: Install Java 21 nếu chưa có**

*Windows:*
```bash
# Download từ https://adoptium.net/temurin/releases/?version=21
# Install .msi file
# Verify installation
java -version
```

*Mac:*
```bash
brew install openjdk@21
# Add to PATH
echo 'export PATH="/opt/homebrew/opt/openjdk@21/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
java -version
```

*Linux:*
```bash
sudo apt update
sudo apt install openjdk-21-jdk
java -version
```

**Bước 3: Set JAVA_HOME environment variable**

*Windows:*
```bash
# System Properties → Environment Variables
JAVA_HOME = C:\Program Files\Eclipse Adoptium\jdk-21.0.1.12-hotspot
Path = %JAVA_HOME%\bin
```

*Mac/Linux:*
```bash
# Add to ~/.zshrc or ~/.bashrc
export JAVA_HOME=/Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
```

**Verification:**
```bash
java -version
# Should show: openjdk version "21.0.x"

javac -version
# Should show: javac 21.0.x

echo $JAVA_HOME
# Should show Java installation path
```

**Prevention:**
- Document Java version requirement in README
- Use `.sdkmanrc` file for team consistency
- Check Java version in CI/CD pipeline scripts

---

### Issue 1.2: Maven Not Found
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
'mvn' is not recognized as an internal or external command
# Or
mvn: command not found
```

**Root Cause:**
- Maven chưa được cài đặt
- Maven không có trong PATH

**Solution:**

**Option A: Install via Package Manager (Recommended)**

*Windows (Chocolatey):*
```bash
choco install maven
```

*Mac (Homebrew):*
```bash
brew install maven
```

*Linux:*
```bash
sudo apt install maven
```

**Option B: Manual Installation**

```bash
# Download from https://maven.apache.org/download.cgi
# Extract to C:\Program Files\Apache\maven (Windows) or /opt/maven (Linux)

# Set environment variables
MAVEN_HOME = C:\Program Files\Apache\maven
Path = %MAVEN_HOME%\bin

# Mac/Linux: Add to ~/.zshrc or ~/.bashrc
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH
```

**Verification:**
```bash
mvn -version
# Should show: Apache Maven 3.8.x or higher
```

**Prevention:**
- Include Maven in project setup documentation
- Use Maven Wrapper (mvnw) để avoid version conflicts
- Check Maven version in CI/CD

---

### Issue 1.3: IntelliJ/Eclipse Project Import Fails
**Severity:** 🟡 Medium

**Triệu chứng:**
- Project structure không hiển thị đúng
- Dependencies không được resolve
- "Cannot resolve symbol" errors everywhere
- Maven/Gradle buttons missing

**Root Cause:**
- Project không được import as Maven project
- IDE cache corrupted
- Wrong JDK configured in project

**Solution:**

**IntelliJ IDEA:**

**Bước 1: Re-import as Maven Project**
```
1. File → Close Project
2. File → Open → Select pom.xml (not project folder)
3. Choose "Open as Project"
4. Wait for Maven import to complete
```

**Bước 2: Configure JDK**
```
1. File → Project Structure (Ctrl+Alt+Shift+S)
2. Project → Project SDK → Select JDK 21
3. Project → Language Level → 21
4. Modules → Dependencies → Module SDK → 21
```

**Bước 3: Invalidate Caches**
```
1. File → Invalidate Caches
2. Check "Clear file system cache"
3. Restart IDE
```

**Bước 4: Reimport Maven Dependencies**
```
1. Right-click on pom.xml
2. Maven → Reload Project
3. Or: Click Maven tool window → Reload (↻)
```

**Eclipse:**

```
1. File → Import → Existing Maven Projects
2. Select project root directory
3. Ensure pom.xml is checked
4. Finish
5. Right-click project → Maven → Update Project
6. Check "Force Update of Snapshots/Releases"
```

**Verification:**
```bash
# In terminal within IDE
mvn clean compile
# Should compile successfully

# Run a test from IDE
# Should show run configuration options
```

**Prevention:**
- Always import via pom.xml, not as general project
- Keep IDE plugins updated
- Document IDE setup steps in README

---

### Issue 1.4: IDE Cannot Find TestNG/JUnit
**Severity:** 🟠 High

**Triệu chứng:**
```java
Cannot resolve symbol 'Test'
Cannot resolve symbol 'Assert'
No test runner found
```

**Root Cause:**
- TestNG dependency missing in pom.xml
- IDE plugin not installed
- Wrong scope in dependency

**Solution:**

**Bước 1: Verify pom.xml dependency**
```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.0</version>
    <scope>test</scope>  <!-- MUST be test scope -->
</dependency>
```

**Bước 2: Install IDE Plugin**

*IntelliJ:*
```
1. File → Settings → Plugins
2. Search "TestNG"
3. Install TestNG plugin
4. Restart IDE
```

*Eclipse:*
```
1. Help → Eclipse Marketplace
2. Search "TestNG"
3. Install TestNG for Eclipse
4. Restart Eclipse
```

**Bước 3: Reimport Maven**
```bash
mvn clean install
# Then in IDE: Reload Maven project
```

**Verification:**
```java
// Right-click on test class
// Should see "Run '{ClassName}'" option
// Should show TestNG icon (not JUnit)
```

**Prevention:**
- Always use correct scope (test) for test dependencies
- Keep TestNG version updated
- Document plugin requirements

---

### Issue 1.5: Git Clone Fails with SSL Error
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
fatal: unable to access 'https://...': SSL certificate problem
```

**Root Cause:**
- Corporate proxy blocking SSL
- Self-signed certificates
- Firewall restrictions

**Solution:**

**Option A: Disable SSL Verification (Temporary)**
```bash
git config --global http.sslVerify false
# Only use for corporate environments with self-signed certs
```

**Option B: Configure Proxy**
```bash
git config --global http.proxy http://proxy.company.com:8080
git config --global https.proxy http://proxy.company.com:8080
```

**Option C: Use SSH instead of HTTPS**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@company.com"

# Add to GitHub/GitLab
cat ~/.ssh/id_ed25519.pub

# Clone with SSH
git clone git@github.com:username/repo.git
```

**Verification:**
```bash
git clone <repository-url>
# Should clone successfully
```

**Prevention:**
- Use SSH keys for corporate environments
- Document proxy settings in README
- Provide .gitconfig template

---

### Issue 1.6: Wrong File Encoding Causing Compile Errors
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
error: unmappable character for encoding UTF-8
warning: [encoding] found encoding problems
```

**Root Cause:**
- Files saved with wrong encoding (Windows-1252, ISO-8859-1)
- Vietnamese characters không được encode đúng
- IDE default encoding không phải UTF-8

**Solution:**

**Bước 1: Configure Maven encoding**
```xml
<!-- In pom.xml -->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

**Bước 2: Set IDE default encoding**

*IntelliJ:*
```
1. File → Settings → Editor → File Encodings
2. Global Encoding: UTF-8
3. Project Encoding: UTF-8
4. Default encoding for properties files: UTF-8
5. Transparent native-to-ascii conversion: checked
```

*Eclipse:*
```
1. Window → Preferences → General → Workspace
2. Text file encoding: UTF-8
3. Apply and Close
```

**Bước 3: Convert existing files**
```bash
# Check file encoding
file -i *.java

# Convert if needed (Linux/Mac)
iconv -f ISO-8859-1 -t UTF-8 LoginTest.java -o LoginTest.java
```

**Verification:**
```bash
mvn clean compile
# Should compile without encoding warnings
```

**Prevention:**
- Set UTF-8 encoding in project setup
- Use EditorConfig file
- Add encoding check in CI/CD

---

### Issue 1.7: Firewall/Antivirus Blocking WebDriver
**Severity:** 🟠 High

**Triệu chứng:**
```bash
WebDriverException: Unable to start ChromeDriver
# Or
Connection refused
# Or
Access denied
```

**Root Cause:**
- Corporate antivirus blocking chromedriver.exe
- Firewall blocking WebDriver ports
- Security software quarantining driver

**Solution:**

**Bước 1: Add WebDriver to antivirus exclusions**
```
# Windows Defender
1. Settings → Update & Security → Windows Security
2. Virus & threat protection → Manage settings
3. Exclusions → Add exclusion
4. Add folder: C:\Users\{username}\.cache\selenium
5. Add folder: C:\{project}\drivers
```

**Bước 2: Unblock driver executable**
```powershell
# Windows: Right-click chromedriver.exe
# Properties → Check "Unblock" → Apply
```

**Bước 3: Configure firewall**
```bash
# Allow Java through firewall
# Control Panel → Windows Firewall → Allow an app
# Add: java.exe and javaw.exe
```

**Bước 4: Use WebDriverManager (Auto-download)**
```xml
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.6.2</version>
</dependency>
```

```java
@BeforeClass
public void setup() {
    WebDriverManager.chromedriver().setup();
    driver = new ChromeDriver();
}
```

**Verification:**
```java
// Run simple test
driver.get("https://www.google.com");
// Should open browser without errors
```

**Prevention:**
- Use WebDriverManager instead of manual drivers
- Document antivirus exclusions in README
- Test in clean environment

---

## 2️⃣ DEPENDENCY & BUILD ISSUES

### Issue 2.1: Maven Dependency Not Resolved
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
[ERROR] Failed to execute goal on project: Could not resolve dependencies
# Or
Could not find artifact org.seleniumhq.selenium:selenium-java:jar:4.16.1
# Or
Transfer failed for https://repo.maven.apache.org/...
```

**Root Cause:**
- Network/proxy blocking Maven Central
- Corrupted local Maven repository
- Wrong dependency coordinates
- Repository không available

**Solution:**

**Bước 1: Clear local Maven repository**
```bash
# Delete .m2 folder
# Windows
rmdir /s "%USERPROFILE%\.m2\repository"

# Mac/Linux
rm -rf ~/.m2/repository
```

**Bước 2: Force update**
```bash
mvn clean install -U
# -U forces update of snapshots/releases
```

**Bước 3: Check dependency coordinates**
```xml
<!-- Verify in pom.xml - correct coordinates -->
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.16.1</version>
</dependency>

<!-- Check on https://mvnrepository.com/ -->
```

**Bước 4: Configure proxy (if needed)**
```xml
<!-- ~/.m2/settings.xml -->
<settings>
    <proxies>
        <proxy>
            <id>company-proxy</id>
            <active>true</active>
            <protocol>http</protocol>
            <host>proxy.company.com</host>
            <port>8080</port>
        </proxy>
    </proxies>
</settings>
```

**Bước 5: Use alternative repository**
```xml
<!-- In pom.xml -->
<repositories>
    <repository>
        <id>central</id>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
```

**Verification:**
```bash
mvn dependency:tree
# Should show all dependencies resolved

mvn clean compile
# Should compile successfully
```

**Prevention:**
- Use dependency management section
- Lock versions to avoid SNAPSHOT changes
- Mirror Maven Central internally for企業

---

### Issue 2.2: ChromeDriver Version Mismatch
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
SessionNotCreatedException: This version of ChromeDriver only supports Chrome version 120
# Or
WebDriverException: unknown error: cannot find Chrome binary
```

**Root Cause:**
- ChromeDriver version không match với Chrome browser version
- Chrome auto-updated nhưng driver không update
- Wrong driver path

**Solution:**

**Option A: Use WebDriverManager (Recommended)**
```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.6.2</version>
</dependency>
```

```java
import io.github.bonigarcia.wdm.WebDriverManager;

@BeforeClass
public void setup() {
    WebDriverManager.chromedriver().setup();  // Auto-downloads matching version
    driver = new ChromeDriver();
}
```

**Option B: Manual Download**
```bash
# Check Chrome version
chrome://version

# Download matching ChromeDriver from
# https://chromedriver.chromium.org/downloads

# Extract and place in project/drivers folder
```

**Option C: Use Chrome for Testing**
```java
ChromeOptions options = new ChromeOptions();
options.setBrowserVersion("120");  // Specific version
driver = new ChromeDriver(options);
```

**Verification:**
```java
@Test
public void testBrowserLaunch() {
    driver.get("chrome://version");
    // Should launch without SessionNotCreatedException
}
```

**Prevention:**
- Always use WebDriverManager in framework
- Pin Chrome version in Docker/CI environments
- Monitor Chrome auto-updates

---

### Issue 2.3: Selenium Version Conflict
**Severity:** 🟠 High

**Triệu chứng:**
```bash
NoSuchMethodError: org.openqa.selenium.WebDriver.findElement
# Or
Incompatible selenium versions
# Or
AbstractMethodError
```

**Root Cause:**
- Multiple Selenium versions in classpath
- Dependency pulling old Selenium version
- Selenium 3 vs Selenium 4 incompatibilities

**Solution:**

**Bước 1: Check dependency tree**
```bash
mvn dependency:tree | grep selenium
# Look for multiple selenium versions
```

**Bước 2: Exclude transitive dependencies**
```xml
<dependency>
    <groupId>some-library</groupId>
    <artifactId>library-name</artifactId>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**Bước 3: Use dependency management**
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.16.1</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

**Bước 4: Force single version**
```bash
mvn clean install -U
```

**Verification:**
```bash
mvn dependency:tree | grep selenium
# Should show only one selenium version

mvn clean test
# Tests should run without NoSuchMethodError
```

**Prevention:**
- Use dependencyManagement
- Regular dependency updates
- Check for conflicts during code review

---

### Issue 2.4: TestNG Not Executing Tests
**Severity:** 🟠 High

**Triệu chứng:**
```bash
No tests were found
# Or
Total tests run: 0
# Or
Tests are not being picked up by Maven
```

**Root Cause:**
- Test class không có @Test annotation
- Maven Surefire plugin misconfigured
- testng.xml not found
- Test naming convention không match pattern

**Solution:**

**Bước 1: Verify Test annotations**
```java
import org.testng.annotations.Test;  // ✅ Correct import

@Test  // ✅ Class or method must have @Test
public void testLogin() {
    // Test code
}
```

**Bước 2: Configure Maven Surefire**
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.2.2</version>
            <configuration>
                <!-- Option 1: Use testng.xml -->
                <suiteXmlFiles>
                    <suiteXmlFile>testng.xml</suiteXmlFile>
                </suiteXmlFiles>

                <!-- Option 2: Use naming pattern -->
                <includes>
                    <include>**/*Test.java</include>
                    <include>**/Test*.java</include>
                </includes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**Bước 3: Create testng.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Test Suite">
    <test name="All Tests">
        <packages>
            <package name="tests.*"/>
        </packages>
    </test>
</suite>
```

**Bước 4: Follow naming convention**
```
✅ LoginTest.java
✅ TestLogin.java
❌ Login.java (won't be picked up)
❌ LoginTests.java (if using *Test.java pattern)
```

**Verification:**
```bash
mvn clean test
# Should show: Tests run: X, Failures: 0, Errors: 0, Skipped: 0
```

**Prevention:**
- Use standard naming conventions
- Always configure Surefire plugin
- Document test execution commands

---

### Issue 2.5: Spring Boot Dependencies Conflict
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
ClassNotFoundException: org.springframework.boot.SpringApplication
# Or
NoSuchBeanDefinitionException
# Or
BeanCreationException
```

**Root Cause:**
- Spring Boot version conflicts
- Missing spring-boot-starter dependencies
- Wrong Spring Framework version

**Solution:**

**Bước 1: Use Spring Boot BOM**
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.1</version>
</parent>
```

**Bước 2: Use starter dependencies**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <!-- No version needed - managed by parent -->
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**Bước 3: Check Spring Boot compatibility**
```
Spring Boot 3.2.x requires:
- Java 17 or higher
- Jakarta EE 9+ (not javax.*)
```

**Verification:**
```bash
mvn spring-boot:run
# Application should start without errors

curl http://localhost:8080/actuator/health
# Should return: {"status":"UP"}
```

**Prevention:**
- Always use spring-boot-starter-parent
- Keep Spring Boot versions consistent
- Use Spring Initializr for new projects

---

### Issue 2.6: Compilation Errors After Pull
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
error: cannot find symbol
# Or
error: package does not exist
# After git pull
```

**Root Cause:**
- New dependencies added by teammate
- Local Maven cache outdated
- IDE didn't refresh

**Solution:**

```bash
# Clean everything
mvn clean

# Update dependencies
mvn clean install -U

# In IDE: Reload Maven project
# IntelliJ: Maven tool window → Reload (↻)
# Eclipse: Right-click project → Maven → Update Project
```

**Verification:**
```bash
mvn clean compile
# Should compile successfully
```

**Prevention:**
- Always run `mvn clean install` after pull
- Configure IDE to auto-reload Maven changes
- Document in README

---

### Issue 2.7: OutOfMemoryError During Build
**Severity:** 🟠 High

**Triệu chứng:**
```bash
java.lang.OutOfMemoryError: Java heap space
# Or
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

**Root Cause:**
- Maven default memory too small
- Large test suite
- Memory leaks in tests

**Solution:**

**Bước 1: Increase Maven memory**
```bash
# Set MAVEN_OPTS environment variable
# Windows
set MAVEN_OPTS=-Xmx2048m -XX:MaxPermSize=512m

# Mac/Linux
export MAVEN_OPTS="-Xmx2048m -XX:MaxPermSize=512m"
```

**Bước 2: Configure in pom.xml**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.2</version>
    <configuration>
        <argLine>-Xmx2048m</argLine>
        <forkCount>1</forkCount>
        <reuseForks>true</reuseForks>
    </configuration>
</plugin>
```

**Bước 3: Run tests in batches**
```bash
mvn test -Dtest=LoginTest,RegisterTest
# Instead of running all tests at once
```

**Verification:**
```bash
mvn clean install
# Should complete without OutOfMemoryError
```

**Prevention:**
- Set MAVEN_OPTS globally
- Profile memory usage
- Close WebDriver properly in @AfterMethod

---

### Issue 2.8: Download Failures in CI/CD
**Severity:** 🟠 High

**Triệu chứng:**
```bash
Could not transfer artifact from/to central
# Or
Connection timeout
# Only in CI/CD environment
```

**Root Cause:**
- CI/CD server không có internet access
- Maven Central blocked by firewall
- Rate limiting

**Solution:**

**Option A: Use internal Maven repository**
```xml
<repositories>
    <repository>
        <id>company-nexus</id>
        <url>http://nexus.company.com/repository/maven-public/</url>
    </repository>
</repositories>
```

**Option B: Cache dependencies**
```yaml
# GitLab CI
cache:
  paths:
    - .m2/repository

before_script:
  - export MAVEN_OPTS="-Dmaven.repo.local=.m2/repository"
```

**Option C: Bundle dependencies**
```bash
# Create offline bundle
mvn dependency:go-offline

# Use in CI
mvn -o clean test  # -o = offline mode
```

**Verification:**
```bash
# In CI/CD pipeline
mvn clean test
# Should not download dependencies
```

**Prevention:**
- Setup internal Maven mirror
- Cache .m2 folder in CI/CD
- Use dependency caching strategies

---

## 3️⃣ RUNTIME ERRORS

### Issue 3.1: NoSuchElementException
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
org.openqa.selenium.NoSuchElementException: no such element: Unable to locate element
```

**Root Cause:**
- Element chưa được load khi tìm kiếm
- Locator sai hoặc element không tồn tại
- Element nằm trong iframe
- Timing issue (page chưa load xong)

**Solution:**

**Bước 1: Add explicit wait**
```java
// ❌ BAD
WebElement element = driver.findElement(By.id("username"));

// ✅ GOOD
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.presenceOfElementLocated(By.id("username")));
```

**Bước 2: Verify locator**
```java
// Open browser DevTools (F12)
// Console: Try locator
$('#username')  // CSS selector
$x('//input[@id="username"]')  // XPath

// Should return element, not null
```

**Bước 3: Check for iframe**
```java
// If element in iframe, switch to it first
driver.switchTo().frame("iframe-name");
WebElement element = driver.findElement(By.id("username"));
driver.switchTo().defaultContent();  // Switch back
```

**Bước 4: Wait for page load**
```java
public void waitForPageLoad() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
    wait.until(driver -> ((JavascriptExecutor) driver)
        .executeScript("return document.readyState").equals("complete"));
}
```

**Bước 5: Use multiple locator strategies**
```java
// Try different locators
By[] locators = {
    By.id("username"),
    By.name("username"),
    By.cssSelector("input[type='text']"),
    By.xpath("//input[@placeholder='Username']")
};

for (By locator : locators) {
    try {
        WebElement element = driver.findElement(locator);
        return element;
    } catch (NoSuchElementException e) {
        continue;
    }
}
```

**Verification:**
```java
@Test
public void testElementFound() {
    driver.get("https://example.com");
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    WebElement element = wait.until(ExpectedConditions.presenceOfElementLocated(By.id("username")));
    Assert.assertTrue(element.isDisplayed());
}
```

**Prevention:**
- Always use explicit waits
- Verify locators in DevTools first
- Create WaitHelper utility class
- Document dynamic elements in Page Objects

---

### Issue 3.2: ElementClickInterceptedException
**Severity:** 🟠 High

**Triệu chứng:**
```bash
org.openqa.selenium.ElementClickInterceptedException: 
  element click intercepted: Element <button>...</button> is not clickable at point (x, y). 
  Other element would receive the click: <div class="overlay">...</div>
```

**Root Cause:**
- Overlay/modal che phủ element
- Element bị che bởi sticky header/footer
- Animation chưa hoàn thành
- Element nằm ngoài viewport

**Solution:**

**Bước 1: Wait for element clickable**
```java
// ❌ BAD
driver.findElement(By.id("submitButton")).click();

// ✅ GOOD
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("submitButton")));
element.click();
```

**Bước 2: Scroll element into view**
```java
public void scrollToElement(WebElement element) {
    JavascriptExecutor js = (JavascriptExecutor) driver;
    js.executeScript("arguments[0].scrollIntoView({behavior: 'smooth', block: 'center'});", element);
    Thread.sleep(500);  // Wait for scroll animation
}

// Usage
scrollToElement(submitButton);
submitButton.click();
```

**Bước 3: Wait for overlays to disappear**
```java
public void waitForOverlayToDisappear() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.invisibilityOfElementLocated(By.className("loading-overlay")));
}
```

**Bước 4: Use JavaScript click as fallback**
```java
public void clickElement(WebElement element) {
    try {
        element.click();
    } catch (ElementClickInterceptedException e) {
        logger.warn("Normal click failed, using JavaScript click");
        JavascriptExecutor js = (JavascriptExecutor) driver;
        js.executeScript("arguments[0].click();", element);
    }
}
```

**Bước 5: Maximize window**
```java
@BeforeMethod
public void setup() {
    driver = new ChromeDriver();
    driver.manage().window().maximize();  // Ensure element in viewport
}
```

**Verification:**
```java
@Test
public void testClickWithoutInterception() {
    loginPage.scrollToSubmitButton();
    loginPage.waitForOverlays();
    loginPage.clickSubmit();  // Should click successfully
}
```

**Prevention:**
- Always wait for clickable condition
- Implement auto-scroll in Page Object click methods
- Check for overlays before interactions
- Use JavaScript click wrapper

---

### Issue 3.3: StaleElementReferenceException
**Severity:** 🟠 High

**Triệu chứng:**
```bash
org.openqa.selenium.StaleElementReferenceException: 
  stale element reference: element is not attached to the page document
```

**Root Cause:**
- Element bị DOM re-render sau khi locate
- AJAX request refresh page/element
- Navigation causes page reload
- Element reference outdated

**Solution:**

**Bước 1: Re-locate element**
```java
// ❌ BAD - Reusing old reference
WebElement element = driver.findElement(By.id("dynamic-content"));
// ... some actions ...
element.click();  // May throw StaleElementReferenceException

// ✅ GOOD - Re-locate before each interaction
public void clickDynamicElement() {
    driver.findElement(By.id("dynamic-content")).click();
}
```

**Bước 2: Use retry mechanism**
```java
public void clickWithRetry(By locator, int maxAttempts) {
    for (int i = 0; i < maxAttempts; i++) {
        try {
            driver.findElement(locator).click();
            return;
        } catch (StaleElementReferenceException e) {
            if (i == maxAttempts - 1) {
                throw e;
            }
            logger.warn("Stale element, retrying... Attempt: " + (i + 1));
        }
    }
}

// Usage
clickWithRetry(By.id("submitButton"), 3);
```

**Bước 3: Wait after AJAX**
```java
public void waitForAjaxComplete() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(20));
    wait.until(driver -> {
        JavascriptExecutor js = (JavascriptExecutor) driver;
        return (Boolean) js.executeScript("return jQuery.active == 0");
    });
}
```

**Bước 4: Use Expected Conditions**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.refreshed(
    ExpectedConditions.elementToBeClickable(By.id("dynamic-button"))
));
element.click();
```

**Bước 5: Store locator, not element**
```java
// ❌ BAD - Store WebElement
public class LoginPage {
    private WebElement emailField = driver.findElement(By.id("email"));
}

// ✅ GOOD - Store By locator
public class LoginPage {
    private By emailFieldLocator = By.id("email");

    public void fillEmail(String email) {
        driver.findElement(emailFieldLocator).sendKeys(email);  // Fresh element
    }
}
```

**Verification:**
```java
@Test
public void testNonStaleInteraction() {
    // Trigger AJAX
    searchPage.enterSearchTerm("test");
    searchPage.waitForResults();

    // Should not throw StaleElementReferenceException
    searchPage.clickFirstResult();
}
```

**Prevention:**
- Never reuse WebElement references
- Always re-locate before interaction
- Implement retry wrapper for dynamic pages
- Wait for AJAX/animations before interactions

---

### Issue 3.4: TimeoutException
**Severity:** 🟠 High

**Triệu chứng:**
```bash
org.openqa.selenium.TimeoutException: 
  Expected condition failed: waiting for presence of element located by: By.id: submitButton 
  (tried for 10 second(s) with 500 milliseconds interval)
```

**Root Cause:**
- Element takes longer than timeout to appear
- Locator sai → element không bao giờ xuất hiện
- Network slow
- Application slow to respond

**Solution:**

**Bước 1: Increase timeout for slow pages**
```java
// Adjust timeout based on expected load time
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));  // Increased from 10
WebElement element = wait.until(ExpectedConditions.presenceOfElementLocated(By.id("slowElement")));
```

**Bước 2: Verify locator is correct**
```java
// Debug: Print page source
System.out.println(driver.getPageSource());

// Or: Take screenshot
File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(screenshot, new File("debug-screenshot.png"));

// Check if element exists with different locator
```

**Bước 3: Wait for multiple conditions**
```java
public void waitForPageReady() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));

    // Wait for document ready state
    wait.until(driver -> ((JavascriptExecutor) driver)
        .executeScript("return document.readyState").equals("complete"));

    // Wait for jQuery if using jQuery
    wait.until(driver -> (Boolean) ((JavascriptExecutor) driver)
        .executeScript("return typeof jQuery != 'undefined' && jQuery.active == 0"));

    // Wait for specific element
    wait.until(ExpectedConditions.presenceOfElementLocated(By.id("content")));
}
```

**Bước 4: Use FluentWait for complex scenarios**
```java
Wait<WebDriver> fluentWait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofSeconds(2))
    .ignoring(NoSuchElementException.class)
    .ignoring(StaleElementReferenceException.class)
    .withMessage("Element did not appear within 30 seconds");

WebElement element = fluentWait.until(driver -> 
    driver.findElement(By.id("dynamic-element"))
);
```

**Bước 5: Add informative error messages**
```java
try {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.presenceOfElementLocated(By.id("element")));
} catch (TimeoutException e) {
    String errorMsg = String.format(
        "Element not found. Current URL: %s, Page title: %s, Timestamp: %s",
        driver.getCurrentUrl(),
        driver.getTitle(),
        System.currentTimeMillis()
    );
    logger.error(errorMsg);
    throw new TimeoutException(errorMsg, e);
}
```

**Verification:**
```java
@Test
public void testElementAppearsWithinTimeout() {
    driver.get("https://slow-loading-page.com");
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
    WebElement element = wait.until(ExpectedConditions.presenceOfElementLocated(By.id("content")));
    Assert.assertTrue(element.isDisplayed());
}
```

**Prevention:**
- Set reasonable timeouts (10-30s)
- Implement custom wait utilities
- Monitor application performance
- Log detailed error context

---

### Issue 3.5: InvalidSelectorException
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
org.openqa.selenium.InvalidSelectorException: invalid selector: Unable to locate an element
# Or
The string '//div[@class='invalid']' is not a valid XPath expression
```

**Root Cause:**
- XPath syntax error (unmatched quotes)
- Invalid CSS selector
- Special characters not escaped

**Solution:**

**Fix XPath syntax:**
```java
// ❌ BAD - Unmatched quotes
driver.findElement(By.xpath("//div[@class='item']"));  // Error if attribute has single quotes

// ✅ GOOD - Escape properly
driver.findElement(By.xpath("//div[@class="item"]"));
// Or use single quotes outside, double inside
driver.findElement(By.xpath("//div[@class='item']"));
```

**Fix CSS selector:**
```java
// ❌ BAD
driver.findElement(By.cssSelector("div[@class='item']"));  // @ not valid in CSS

// ✅ GOOD
driver.findElement(By.cssSelector("div.item"));  // Use . for class
driver.findElement(By.cssSelector("div[class='item']"));  // Or attribute selector
```

**Handle dynamic values:**
```java
// ✅ Use String.format
String dynamicId = "user_123";
By locator = By.xpath(String.format("//div[@id='%s']", dynamicId));

// ✅ Or concatenation
By locator = By.cssSelector("#" + dynamicId);
```

**Verification:**
```java
// Test locator in browser DevTools first
$x("//div[@class='item']")  // XPath
$("div.item")  // CSS
```

**Prevention:**
- Use ID/name/CSS over XPath
- Test locators in DevTools before coding
- Use locator helper methods

---

### Issue 3.6: WebDriverException: Chrome Crashed
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
org.openqa.selenium.WebDriverException: chrome not reachable
# Or
unknown error: Chrome failed to start: exited abnormally
```

**Root Cause:**
- Insufficient memory
- Chrome process zombie
- Too many browser instances
- Headless mode issues

**Solution:**

**Bước 1: Add Chrome options**
```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
options.addArguments("--disable-gpu");
options.addArguments("--remote-allow-origins=*");
driver = new ChromeDriver(options);
```

**Bước 2: Kill zombie processes**
```bash
# Windows
taskkill /F /IM chrome.exe /T
taskkill /F /IM chromedriver.exe /T

# Mac/Linux
pkill -9 chrome
pkill -9 chromedriver
```

**Bước 3: Proper cleanup**
```java
@AfterMethod
public void teardown() {
    if (driver != null) {
        driver.quit();  // Not just close()
        driver = null;
    }
}
```

**Bước 4: Limit parallel browsers**
```xml
<!-- testng.xml -->
<suite name="Suite" parallel="methods" thread-count="3">
  <!-- Don't run too many parallel tests -->
</suite>
```

**Prevention:**
- Always call driver.quit()
- Use ChromeOptions properly
- Monitor system resources
- Limit parallel execution

---

### Issue 3.7: UnexpectedAlertPresentException
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
org.openqa.selenium.UnexpectedAlertPresentException: unexpected alert open
```

**Root Cause:**
- JavaScript alert/confirm/prompt chưa được handle
- Alert xuất hiện bất ngờ

**Solution:**

```java
// Handle alert if present
public void handleAlertIfPresent() {
    try {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(2));
        Alert alert = wait.until(ExpectedConditions.alertIsPresent());
        String alertText = alert.getText();
        logger.info("Alert present: " + alertText);
        alert.accept();  // Or alert.dismiss()
    } catch (TimeoutException e) {
        // No alert present
    }
}

// Use before sensitive actions
handleAlertIfPresent();
driver.findElement(By.id("button")).click();
```

**Prevention:**
- Always check for alerts after actions
- Set UnexpectedAlertBehaviour in capabilities
- Implement alert handler utility

---

### Issue 3.8: InvalidSessionIdException
**Severity:** 🟠 High

**Triệu chứng:**
```bash
org.openqa.selenium.InvalidSessionIdException: invalid session id
```

**Root Cause:**
- WebDriver session closed prematurely
- Browser manually closed
- Driver.quit() called too early

**Solution:**

```java
// Ensure driver is valid before use
public void safeNavigate(String url) {
    try {
        driver.get(url);
    } catch (InvalidSessionIdException e) {
        logger.error("Session invalid, reinitializing driver");
        initializeDriver();
        driver.get(url);
    }
}
```

**Prevention:**
- Don't close browser manually during test
- Proper driver lifecycle management
- Use ThreadLocal for parallel execution

---

### Issue 3.9: ElementNotInteractableException
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
org.openqa.selenium.ElementNotInteractableException: element not interactable
```

**Root Cause:**
- Element hidden (display:none, visibility:hidden)
- Element disabled
- Element covered by another element

**Solution:**

```java
// Check if element interactable
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("button")));

// Or use JavaScript
JavascriptExecutor js = (JavascriptExecutor) driver;
js.executeScript("arguments[0].removeAttribute('disabled')", element);
js.executeScript("arguments[0].click()", element);
```

**Prevention:**
- Wait for elementToBeClickable
- Check element state before interaction
- Use JavaScript interaction as fallback

---

## 4️⃣ APPLICATION ISSUES (AUT)

### Issue 4.1: Port Already in Use
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
***************************
APPLICATION FAILED TO START
***************************

Description:

Web server failed to start. Port 8080 was already in use.

Action:

Identify and stop the process that's listening on port 8080 or configure this application to listen on another port.
```

**Root Cause:**
- Previous Spring Boot instance không shutdown clean
- Khác application đang dùng port 8080
- Zombie Java process

**Solution:**

**Bước 1: Find process using port**

*Windows:*
```bash
netstat -ano | findstr :8080
# Note the PID

taskkill /PID <PID> /F
```

*Mac/Linux:*
```bash
lsof -i :8080
# Note the PID

kill -9 <PID>
```

**Bước 2: Change application port**
```properties
# src/main/resources/application.properties
server.port=8081

# Or use random port for tests
server.port=0
```

**Bước 3: Configure in test**
```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
public class LoginTest {

    @LocalServerPort
    private int port;

    @BeforeMethod
    public void setup() {
        baseUrl = "http://localhost:" + port;
    }
}
```

**Bước 4: Cleanup in @AfterClass**
```java
@AfterClass
public void cleanup() {
    // Ensure application stops
    SpringApplication.exit(applicationContext, () -> 0);
}
```

**Verification:**
```bash
# Start application
mvn spring-boot:run

# Should start without error
# Check: http://localhost:8080
```

**Prevention:**
- Use random port in tests
- Implement proper shutdown hooks
- Check port before starting
- Use Docker containers for isolation

---

### Issue 4.2: H2 Database Connection Failed
**Severity:** 🟠 High

**Triệu chứng:**
```bash
Unable to open JDBC Connection for DDL execution
# Or
H2 Console not accessible at http://localhost:8080/h2-console
# Or
Database "mem:testdb" not found
```

**Root Cause:**
- H2 dependency missing
- Database URL misconfigured
- H2 console not enabled
- Database auto-create disabled

**Solution:**

**Bước 1: Add H2 dependency**
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

**Bước 2: Configure H2 in application.properties**
```properties
# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# H2 Console
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# JPA Settings
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
```

**Bước 3: Enable H2 Console**
```java
// No code needed if using properties above
// Access at: http://localhost:8080/h2-console

// JDBC URL: jdbc:h2:mem:testdb
// User: sa
// Password: (leave empty)
```

**Bước 4: Create schema initialization**
```sql
-- src/main/resources/schema.sql
CREATE TABLE IF NOT EXISTS users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

```properties
# Enable schema initialization
spring.sql.init.mode=always
```

**Verification:**
```bash
# Start application
mvn spring-boot:run

# Access H2 Console
# Browser: http://localhost:8080/h2-console

# Should connect successfully
```

**Prevention:**
- Always configure H2 in application.properties
- Use separate properties for test profile
- Document H2 console access in README
- Create schema.sql for complex tables

---

### Issue 4.3: Thymeleaf Template Not Found
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
TemplateInputException: Error resolving template "index", template might not exist
# Or
Error resolving template [login], template might not exist or might not be accessible
```

**Root Cause:**
- Template file đặt sai vị trí
- Sai tên file
- Template prefix/suffix misconfigured
- File extension sai

**Solution:**

**Bước 1: Verify file location**
```
Correct structure:
src/
  main/
    resources/
      templates/          ← Must be here
        index.html        ← .html extension
        login.html
        register.html
```

**Bước 2: Check controller mapping**
```java
@Controller
public class HomeController {

    @GetMapping("/")
    public String index() {
        return "index";  // Returns "index" → looks for templates/index.html
    }

    @GetMapping("/login")
    public String login() {
        return "login";  // Returns "login" → looks for templates/login.html
    }
}
```

**Bước 3: Verify Thymeleaf configuration**
```properties
# src/main/resources/application.properties
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.cache=false  # Disable for development
```

**Bước 4: Check file naming**
```
✅ login.html
❌ Login.html (case-sensitive on Linux)
❌ login.htm (wrong extension)
❌ login.jsp (wrong template engine)
```

**Verification:**
```bash
mvn spring-boot:run

# Access endpoint
curl http://localhost:8080/login
# Should return HTML content, not 500 error
```

**Prevention:**
- Follow naming conventions strictly
- Use lowercase for template names
- Keep templates in src/main/resources/templates/
- Disable Thymeleaf cache during development

---

### Issue 4.4: Bean Creation Failed
**Severity:** 🟠 High

**Triệu chứng:**
```bash
BeanCreationException: Error creating bean with name 'userService'
# Or
UnsatisfiedDependencyException: Error creating bean ... unsatisfied dependency
# Or
NoSuchBeanDefinitionException: No qualifying bean of type 'UserRepository'
```

**Root Cause:**
- Missing @Component/@Service/@Repository annotation
- Component scanning không cover package
- Circular dependency
- Bean configuration error

**Solution:**

**Bước 1: Add proper annotations**
```java
// ❌ Missing annotation
public class UserService {
    // Spring won't detect this
}

// ✅ Correct
@Service
public class UserService {
    // Spring will auto-detect and create bean
}
```

**Bước 2: Ensure component scanning**
```java
@SpringBootApplication
@ComponentScan(basePackages = {
    "com.example.aut.controller",
    "com.example.aut.service",
    "com.example.aut.repository"
})
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

**Bước 3: Fix circular dependency**
```java
// ❌ BAD - Circular dependency
@Service
public class UserService {
    @Autowired
    private OrderService orderService;  // Depends on OrderService
}

@Service
public class OrderService {
    @Autowired
    private UserService userService;  // Depends on UserService → Circular!
}

// ✅ GOOD - Use @Lazy or refactor
@Service
public class UserService {
    @Autowired
    @Lazy  // Break circular dependency
    private OrderService orderService;
}
```

**Bước 4: Verify dependency injection**
```java
@Service
public class UserService {

    private final UserRepository userRepository;

    // Constructor injection (preferred)
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Verification:**
```bash
mvn spring-boot:run
# Should start without BeanCreationException
```

**Prevention:**
- Always use proper annotations
- Use constructor injection
- Avoid circular dependencies
- Enable DEBUG logging for bean creation

---

### Issue 4.5: Static Resources Not Loading
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Browser console
GET http://localhost:8080/css/style.css 404 (Not Found)
GET http://localhost:8080/js/script.js 404 (Not Found)
```

**Root Cause:**
- Files đặt sai vị trí
- Wrong path in HTML
- Security configuration blocking static resources

**Solution:**

**Bước 1: Verify file structure**
```
src/
  main/
    resources/
      static/              ← Must be here
        css/
          style.css
        js/
          script.js
        images/
          logo.png
```

**Bước 2: Correct path in Thymeleaf**
```html
<!-- ❌ BAD -->
<link rel="stylesheet" href="/static/css/style.css">

<!-- ✅ GOOD -->
<link rel="stylesheet" th:href="@{/css/style.css}">
<script th:src="@{/js/script.js}"></script>
<img th:src="@{/images/logo.png}">
```

**Bước 3: Configure static locations (if needed)**
```properties
# application.properties
spring.web.resources.static-locations=classpath:/static/
```

**Bước 4: Allow in Security Config**
```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/css/**", "/js/**", "/images/**").permitAll()
                .anyRequest().authenticated()
            );
        return http.build();
    }
}
```

**Verification:**
```bash
# Start app
mvn spring-boot:run

# Check static resources
curl http://localhost:8080/css/style.css
# Should return CSS content
```

**Prevention:**
- Keep static files in src/main/resources/static/
- Use Thymeleaf @{} syntax for paths
- Test static resources after deployment

---

### Issue 4.6: Application Context Failed to Start
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
ApplicationContextException: Unable to start web server
# Or
Failed to load ApplicationContext
```

**Root Cause:**
- Configuration error in @Bean methods
- Property file syntax error
- Missing required dependencies

**Solution:**

**Check application.properties syntax:**
```properties
# ❌ BAD - Syntax error
server.port 8080

# ✅ GOOD
server.port=8080
```

**Check @Bean configuration:**
```java
@Configuration
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        // Ensure returns valid object
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}
```

**Enable debug logging:**
```properties
logging.level.org.springframework=DEBUG
```

**Verification:**
```bash
mvn spring-boot:run
# Should start successfully
```

---

## 5️⃣ TEST EXECUTION ISSUES

### Issue 5.1: Flaky Tests (Pass/Fail Randomly)
**Severity:** 🟠 High

**Triệu chứng:**
- Tests pass locally, fail in CI
- Same test passes then fails without code changes
- Intermittent failures

**Root Cause:**
- Race conditions / timing issues
- Shared test data
- Async operations not properly awaited
- Network latency variations

**Solution:**

**Bước 1: Add explicit waits**
```java
// ❌ BAD - Implicit wait only
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
driver.findElement(By.id("element")).click();

// ✅ GOOD - Explicit wait for specific condition
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(By.id("element"))).click();
```

**Bước 2: Use unique test data**
```java
// ❌ BAD - Hardcoded data
String testEmail = "test@example.com";

// ✅ GOOD - Unique data per execution
String testEmail = "test_" + System.currentTimeMillis() + "@example.com";
```

**Bước 3: Implement retry analyzer**
```java
public class RetryAnalyzer implements IRetryAnalyzer {
    private int retryCount = 0;
    private static final int maxRetryCount = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetryCount) {
            retryCount++;
            logger.warn("Retrying test: " + result.getName() + " (Attempt " + (retryCount + 1) + ")");
            return true;
        }
        return false;
    }
}

// Use in test
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlakyFeature() {
    // Test code
}
```

**Bước 4: Isolate test data**
```java
@BeforeMethod
public void setupTestData() {
    // Create fresh test data for each test
    testUser = userService.createUser("user_" + UUID.randomUUID());
}

@AfterMethod
public void cleanupTestData() {
    // Cleanup
    if (testUser != null) {
        userService.deleteUser(testUser.getId());
    }
}
```

**Bước 5: Wait for async operations**
```java
public void waitForAjaxComplete() {
    new WebDriverWait(driver, Duration.ofSeconds(20))
        .until(d -> (Boolean) ((JavascriptExecutor) d)
            .executeScript("return jQuery.active == 0"));
}

public void waitForAngularLoad() {
    String angularReadyScript = "return window.getAllAngularTestabilities()"
        + ".findIndex(x=>!x.isStable()) === -1";
    new WebDriverWait(driver, Duration.ofSeconds(20))
        .until(d -> (Boolean) ((JavascriptExecutor) d)
            .executeScript(angularReadyScript));
}
```

**Verification:**
```bash
# Run test multiple times
for i in {1..10}; do mvn test -Dtest=LoginTest; done
# Should pass consistently
```

**Prevention:**
- Never use Thread.sleep()
- Always use explicit waits
- Unique test data per execution
- Proper test isolation
- Wait for async operations

---

### Issue 5.2: Tests Pass Locally But Fail in CI/CD
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
# Local
Tests run: 10, Failures: 0, Errors: 0, Skipped: 0

# CI/CD
Tests run: 10, Failures: 5, Errors: 2, Skipped: 0
```

**Root Cause:**
- Environment differences (OS, timezone, locale)
- CI runs headless, local runs headed
- Different screen resolutions
- Slower CI environment
- Missing dependencies in CI

**Solution:**

**Bước 1: Use headless mode**
```java
// Configure for CI environment
ChromeOptions options = new ChromeOptions();

if (System.getenv("CI") != null) {  // Check if running in CI
    options.addArguments("--headless");
    options.addArguments("--no-sandbox");
    options.addArguments("--disable-dev-shm-usage");
    options.addArguments("--disable-gpu");
}

driver = new ChromeDriver(options);
```

**Bước 2: Set consistent window size**
```java
// CI environments may have different default sizes
driver.manage().window().setSize(new Dimension(1920, 1080));
// Or maximize (but be aware of CI limitations)
driver.manage().window().maximize();
```

**Bước 3: Increase timeouts for CI**
```java
int timeout = System.getenv("CI") != null ? 30 : 10;  // Longer timeout in CI
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(timeout));
```

**Bước 4: Configure CI pipeline properly**

*GitLab CI (.gitlab-ci.yml):*
```yaml
test:
  stage: test
  image: maven:3.8-jdk-21
  services:
    - selenium/standalone-chrome:latest
  variables:
    MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"
  script:
    - apt-get update && apt-get install -y wget gnupg
    - wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
    - sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
    - apt-get update && apt-get install -y google-chrome-stable
    - mvn clean test
  artifacts:
    when: always
    reports:
      junit: target/surefire-reports/TEST-*.xml
    paths:
      - target/screenshots/
      - target/surefire-reports/
```

*GitHub Actions (.github/workflows/test.yml):*
```yaml
name: Test
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          java-version: '21'
          distribution: 'temurin'

      - name: Install Chrome
        uses: browser-actions/setup-chrome@latest

      - name: Run tests
        run: mvn clean test
        env:
          CI: true

      - name: Upload screenshots
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: screenshots
          path: target/screenshots/
```

**Bước 5: Add environment checks**
```java
public class EnvironmentHelper {

    public static boolean isCI() {
        return System.getenv("CI") != null
            || System.getenv("JENKINS_HOME") != null
            || System.getenv("GITLAB_CI") != null;
    }

    public static String getEnvironment() {
        if (System.getenv("CI") != null) return "CI";
        if (System.getenv("JENKINS_HOME") != null) return "Jenkins";
        if (System.getenv("GITLAB_CI") != null) return "GitLab CI";
        return "Local";
    }
}

// Usage
logger.info("Running in environment: " + EnvironmentHelper.getEnvironment());
```

**Verification:**
```bash
# Simulate CI environment locally
export CI=true
mvn clean test

# Should pass with same results as local
```

**Prevention:**
- Always test with headless mode locally
- Use environment-aware configuration
- Consistent dependencies across environments
- Proper CI/CD pipeline setup
- Monitor CI performance metrics

---

### Issue 5.3: Tests Run But All Ignored/Skipped
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
Tests run: 0, Failures: 0, Errors: 0, Skipped: 10
# Or
No tests found
```

**Root Cause:**
- Test methods missing @Test annotation
- Tests disabled with `enabled=false`
- Wrong test groups selected
- Test class not following naming convention

**Solution:**

**Bước 1: Verify @Test annotation**
```java
// ❌ BAD - No annotation
public void testLogin() {
    // Not executed
}

// ✅ GOOD
@Test
public void testLogin() {
    // Will be executed
}
```

**Bước 2: Check enabled status**
```java
// ❌ Disabled test
@Test(enabled = false)
public void testFeature() {
    // Skipped
}

// ✅ Enabled
@Test
public void testFeature() {
    // Executed
}
```

**Bước 3: Verify groups configuration**
```java
// Test with groups
@Test(groups = {"smoke"})
public void testQuickFeature() {
    // Only runs when 'smoke' group selected
}

// testng.xml
<suite name="Suite">
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <packages>
            <package name="tests.*"/>
        </packages>
    </test>
</suite>
```

**Bước 4: Check naming convention**
```bash
# Maven Surefire default patterns:
✅ **/*Test.java
✅ **/Test*.java
✅ **/*Tests.java
✅ **/*TestCase.java

❌ LoginTestClass.java  # Doesn't match
❌ TestsLogin.java      # Doesn't match
```

**Bước 5: Verify Surefire configuration**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.2</version>
    <configuration>
        <includes>
            <include>**/*Test.java</include>
        </includes>
        <!-- Don't exclude accidentally -->
        <!--<excludes>
            <exclude>**/LoginTest.java</exclude>
        </excludes>-->
    </configuration>
</plugin>
```

**Verification:**
```bash
mvn clean test
# Should show: Tests run: X (where X > 0)
```

**Prevention:**
- Follow naming conventions: *Test.java
- Always add @Test annotation
- Review testng.xml groups configuration
- Check Surefire includes/excludes

---

(continued in next message due to length...)


### Issue 5.4: Parallel Execution Conflicts
**Severity:** 🟠 High

**Triệu chứng:**
```bash
# Tests interfere with each other when run in parallel
# Shared data corruption
# Race conditions
```

**Root Cause:**
- Shared WebDriver instance
- Shared test data
- Non-thread-safe code

**Solution:**

**Use ThreadLocal for WebDriver:**
```java
public class BaseTest {
    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    @BeforeMethod
    public void setup() {
        driver.set(new ChromeDriver());
        getDriver().manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    @AfterMethod
    public void teardown() {
        if (getDriver() != null) {
            getDriver().quit();
            driver.remove();
        }
    }
}
```

**Configure TestNG for parallel:**
```xml
<suite name="Parallel Suite" parallel="methods" thread-count="3">
    <test name="Tests">
        <packages>
            <package name="tests.*"/>
        </packages>
    </test>
</suite>
```

**Prevention:**
- Use ThreadLocal for WebDriver
- Unique test data per thread
- Avoid shared state

---

### Issue 5.5: Test Dependencies Causing Cascading Failures
**Severity:** 🟠 High

**Triệu chứng:**
```bash
# One test fails → All dependent tests fail
@Test(dependsOnMethods = {"testA"})
public void testB() {
    // Skipped if testA fails
}
```

**Root Cause:**
- Tests not independent
- dependsOnMethods creates fragile test suite

**Solution:**

```java
// ❌ BAD - Dependencies
@Test(priority = 1)
public void testCreateUser() {
    // Creates user
}

@Test(priority = 2, dependsOnMethods = "testCreateUser")
public void testLoginUser() {
    // Depends on testCreateUser
}

// ✅ GOOD - Independent
@Test
public void testCreateUser() {
    // Setup own data
    // Test
    // Cleanup
}

@Test
public void testLoginUser() {
    // Setup own data
    User user = testDataHelper.createUser();
    // Test
    // Cleanup
    testDataHelper.deleteUser(user.getId());
}
```

**Prevention:**
- Never use dependsOnMethods
- Each test creates own test data
- Implement proper cleanup

---

### Issue 5.6: Out of Memory During Test Suite
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
java.lang.OutOfMemoryError: Java heap space
# After running many tests
```

**Root Cause:**
- WebDriver not properly closed
- Memory leaks in application
- Too many parallel threads

**Solution:**

```java
// Always cleanup
@AfterMethod(alwaysRun = true)
public void teardown() {
    if (driver != null) {
        try {
            driver.quit();
        } catch (Exception e) {
            logger.error("Error quitting driver", e);
        }
        driver = null;
    }
}
```

**Increase memory:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <argLine>-Xmx2048m -XX:+HeapDumpOnOutOfMemoryError</argLine>
        <forkCount>1</forkCount>
        <reuseForks>true</reuseForks>
    </configuration>
</plugin>
```

**Prevention:**
- Always call driver.quit()
- Limit parallel thread count
- Monitor memory usage

---

### Issue 5.7: Tests Hang/Timeout
**Severity:** 🟠 High

**Triệu chứng:**
```bash
# Test never completes
# Maven build hangs
```

**Root Cause:**
- Infinite wait
- Deadlock
- Application hung

**Solution:**

**Set test timeout:**
```java
@Test(timeOut = 60000)  // 60 seconds max
public void testSlowOperation() {
    // Test code
}
```

**Configure Surefire timeout:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <forkedProcessTimeoutInSeconds>300</forkedProcessTimeoutInSeconds>
    </configuration>
</plugin>
```

**Prevention:**
- Always set timeouts
- Monitor test execution time
- Kill zombie processes

---

### Issue 5.8: Test Data Cleanup Failures
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Test data accumulates
# Tests fail due to duplicate data
```

**Root Cause:**
- Cleanup not in finally block
- Exception prevents cleanup
- Database transactions not rolled back

**Solution:**

```java
@Test
public void testUserRegistration() {
    String userId = null;
    try {
        // Test
        userId = registerUser("test@test.com");
        Assert.assertNotNull(userId);
    } finally {
        // Always cleanup
        if (userId != null) {
            deleteUser(userId);
        }
    }
}

// Or use @AfterMethod
@AfterMethod(alwaysRun = true)
public void cleanup() {
    // Cleanup code
    testDataHelper.cleanupAllTestData();
}
```

**Prevention:**
- Always use finally blocks
- Implement robust cleanup logic
- Use database transactions with rollback

---

## 6️⃣ REPORTING & LOGGING ISSUES

### Issue 6.1: ExtentReports Not Generated
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Report file not created
# Or empty report
```

**Root Cause:**
- ExtentReports not initialized
- flush() not called
- Wrong output path

**Solution:**

**Proper ExtentReports setup:**
```java
public class ExtentManager {
    private static ExtentReports extent;
    private static String reportPath = "reports/extent-report.html";

    public static ExtentReports getInstance() {
        if (extent == null) {
            ExtentSparkReporter spark = new ExtentSparkReporter(reportPath);
            spark.config().setDocumentTitle("Test Report");
            spark.config().setReportName("Automation Results");

            extent = new ExtentReports();
            extent.attachReporter(spark);
            extent.setSystemInfo("OS", System.getProperty("os.name"));
            extent.setSystemInfo("Browser", "Chrome");
        }
        return extent;
    }

    public static void flush() {
        if (extent != null) {
            extent.flush();  // ← Must call to generate report
        }
    }
}

// In TestListener
@Override
public void onFinish(ITestContext context) {
    ExtentManager.flush();  // Generate report
}
```

**Verification:**
```bash
mvn clean test
ls reports/
# Should see extent-report.html
```

**Prevention:**
- Always call extent.flush()
- Verify report path exists
- Check file permissions

---

### Issue 6.2: Screenshots Not Captured on Failure
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Test fails but no screenshot
# Screenshot folder empty
```

**Root Cause:**
- Screenshot code not in @AfterMethod
- Driver already quit before screenshot
- Wrong file path

**Solution:**

```java
public class TestListener implements ITestListener {

    @Override
    public void onTestFailure(ITestResult result) {
        WebDriver driver = BaseTest.getDriver();
        if (driver != null) {
            captureScreenshot(driver, result.getName());
        }
    }

    private void captureScreenshot(WebDriver driver, String testName) {
        try {
            File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
            String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            String fileName = testName + "_" + timestamp + ".png";
            File destination = new File("target/screenshots/" + fileName);

            // Create directory if not exists
            destination.getParentFile().mkdirs();

            Files.copy(screenshot.toPath(), destination.toPath(), 
                StandardCopyOption.REPLACE_EXISTING);

            logger.info("Screenshot saved: " + destination.getAbsolutePath());
        } catch (Exception e) {
            logger.error("Failed to capture screenshot", e);
        }
    }
}
```

**Prevention:**
- Capture before driver.quit()
- Create screenshot directory
- Handle exceptions properly

---

### Issue 6.3: Logs Not Appearing
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# No log output
# Or logs going to wrong location
```

**Root Cause:**
- Logging not configured
- Wrong log level
- Log file permissions

**Solution:**

**Configure Logback (logback.xml):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- Console output -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File output -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>logs/automation.log</file>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

**Use in code:**
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LoginTest {
    private static final Logger logger = LoggerFactory.getLogger(LoginTest.class);

    @Test
    public void testLogin() {
        logger.info("Starting login test");
        logger.debug("Email: test@test.com");
        logger.error("Login failed", exception);
    }
}
```

**Prevention:**
- Always configure logging
- Use appropriate log levels
- Create log directories

---

### Issue 6.4: Report Shows Incorrect Test Count
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# TestNG XML shows: Tests run: 10
# But report shows: Tests run: 15
```

**Root Cause:**
- Multiple test suites running
- Tests counted multiple times
- Retry analyzer inflating count

**Solution:**

```java
// Exclude retried tests from report
public class ExtentTestListener implements ITestListener {

    @Override
    public void onTestFailure(ITestResult result) {
        if (result.wasRetried()) {
            return;  // Don't log retried failures
        }
        // Log actual failure
    }
}
```

**Prevention:**
- Handle retry logic properly
- Use single test suite
- Verify test counts

---

### Issue 6.5: Report Path Issues on Different OS
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Works on Windows: reports\extent-report.html
# Fails on Linux: reports\extent-report.html
```

**Root Cause:**
- Hardcoded path separators
- Platform-specific paths

**Solution:**

```java
// ❌ BAD - Hardcoded separators
String reportPath = "reports\extent-report.html";

// ✅ GOOD - Use File.separator or Paths
String reportPath = "reports" + File.separator + "extent-report.html";

// Or better - Use Paths
Path reportPath = Paths.get("reports", "extent-report.html");
String pathString = reportPath.toString();
```

**Prevention:**
- Use File.separator or Paths API
- Test on multiple OS
- Use relative paths

---

## 7️⃣ PERFORMANCE ISSUES

### Issue 7.1: Tests Running Too Slow
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Test suite takes > 30 minutes
# Individual tests > 2 minutes each
```

**Root Cause:**
- Too many unnecessary waits
- Slow page loads
- Inefficient locators
- Serial execution

**Solution:**

**Optimize waits:**
```java
// ❌ BAD - Fixed wait
Thread.sleep(5000);

// ✅ GOOD - Wait only as long as needed
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("element")));
```

**Use efficient locators:**
```java
// Fast → Slow
// id > name > css > xpath

// ✅ Fast
By.id("email")
By.name("email")
By.cssSelector("#email")

// ❌ Slow
By.xpath("//div/div/div[3]/input[@type='email']")
```

**Enable parallel execution:**
```xml
<suite name="Parallel Suite" parallel="methods" thread-count="4">
</suite>
```

**Profile slow tests:**
```java
@Test
public void testPerformance() {
    long startTime = System.currentTimeMillis();

    // Test code

    long endTime = System.currentTimeMillis();
    long duration = endTime - startTime;
    logger.info("Test duration: " + duration + "ms");

    if (duration > 5000) {
        logger.warn("Test exceeded 5 seconds!");
    }
}
```

**Prevention:**
- Set test timeouts
- Monitor test duration
- Optimize locators
- Use parallel execution

---

### Issue 7.2: Maven Build Too Slow
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# mvn clean install takes > 10 minutes
```

**Root Cause:**
- Re-downloading dependencies
- Running all tests
- Not using parallel builds

**Solution:**

**Skip tests during build:**
```bash
mvn clean install -DskipTests
# Or
mvn clean install -Dmaven.test.skip=true
```

**Use parallel builds:**
```bash
mvn clean install -T 4  # Use 4 threads
mvn clean install -T 1C  # Use 1 thread per CPU core
```

**Enable offline mode:**
```bash
mvn clean install -o  # Offline - don't check for updates
```

**Configure Maven for speed:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <fork>true</fork>
        <compilerArgs>
            <arg>-J-XX:+TieredCompilation</arg>
            <arg>-J-XX:TieredStopAtLevel=1</arg>
        </compilerArgs>
    </configuration>
</plugin>
```

**Prevention:**
- Use Maven daemon
- Cache dependencies
- Skip unnecessary plugins

---

### Issue 7.3: High Memory Usage
**Severity:** 🟠 High

**Triệu chứng:**
```bash
# System memory maxed out
# Frequent garbage collection
```

**Root Cause:**
- Memory leaks
- Too many parallel browsers
- Large test data

**Solution:**

**Increase JVM memory:**
```bash
export MAVEN_OPTS="-Xmx4096m -Xms512m"
```

**Configure Surefire memory:**
```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <argLine>-Xmx2048m -XX:MaxPermSize=512m</argLine>
        <forkCount>1</forkCount>
        <reuseForks>true</reuseForks>
    </configuration>
</plugin>
```

**Limit parallel execution:**
```xml
<!-- Reduce thread count -->
<suite name="Suite" parallel="methods" thread-count="2">
</suite>
```

**Monitor memory:**
```java
Runtime runtime = Runtime.getRuntime();
long usedMemory = (runtime.totalMemory() - runtime.freeMemory()) / 1024 / 1024;
logger.info("Used memory: " + usedMemory + " MB");
```

**Prevention:**
- Proper cleanup
- Limit parallelism
- Monitor memory usage

---

### Issue 7.4: Slow Browser Startup
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Browser takes 10+ seconds to start
```

**Root Cause:**
- Chrome extensions loading
- Large browser profile
- Disk I/O slow

**Solution:**

**Optimize Chrome options:**
```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--disable-extensions");
options.addArguments("--disable-gpu");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
options.addArguments("--disable-browser-side-navigation");
options.addArguments("--disable-infobars");
options.setPageLoadStrategy(PageLoadStrategy.EAGER);  // Don't wait for full load

driver = new ChromeDriver(options);
```

**Prevention:**
- Use minimal Chrome options
- Use EAGER page load strategy
- Consider using browser pool

---

## 8️⃣ BROWSER-SPECIFIC ISSUES

### Issue 8.1: Chrome Crashes or Not Reachable
**Severity:** 🔴 Critical

**Triệu chứng:**
```bash
org.openqa.selenium.WebDriverException: chrome not reachable
# Or
unknown error: Chrome failed to start: crashed
```

**Root Cause:**
- Chrome process killed
- Memory exhausted
- Chrome version mismatch

**Solution:**

**Add stability options:**
```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");
options.addArguments("--disable-gpu");
options.addArguments("--remote-allow-origins=*");
options.addArguments("--disable-blink-features=AutomationControlled");

driver = new ChromeDriver(options);
```

**Use WebDriverManager:**
```java
WebDriverManager.chromedriver().setup();
driver = new ChromeDriver();
```

**Prevention:**
- Use stable Chrome options
- Proper cleanup
- Match Chrome and driver versions

---

### Issue 8.2: Firefox Specific Issues
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Tests work in Chrome, fail in Firefox
```

**Root Cause:**
- Different browser behaviors
- Firefox-specific bugs

**Solution:**

```java
FirefoxOptions options = new FirefoxOptions();
options.addArguments("--headless");
options.setPageLoadStrategy(PageLoadStrategy.NORMAL);

driver = new FirefoxDriver(options);
```

**Prevention:**
- Test in multiple browsers
- Use browser-agnostic code
- Handle browser differences

---

### Issue 8.3: Headless Mode Issues
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Tests pass in headed mode, fail in headless
```

**Root Cause:**
- Different rendering in headless
- JavaScript not fully loaded
- Window size differences

**Solution:**

```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless=new");  // New headless mode
options.addArguments("--window-size=1920,1080");  // Set size
options.addArguments("--disable-gpu");

driver = new ChromeDriver(options);
```

**Prevention:**
- Always set window size in headless
- Test both modes
- Use new headless syntax

---

### Issue 8.4: Edge Browser Issues
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# EdgeDriver not found
# Or Edge browser not launching
```

**Root Cause:**
- EdgeDriver not installed
- Wrong Edge version

**Solution:**

```java
WebDriverManager.edgedriver().setup();
EdgeOptions options = new EdgeOptions();
driver = new EdgeDriver(options);
```

**Prevention:**
- Use WebDriverManager
- Test on actual Edge browser
- Keep Edge updated

---

### Issue 8.5: Browser Language/Locale Issues
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Browser opens in wrong language
# Date formats different
```

**Root Cause:**
- System locale different
- Browser preferences

**Solution:**

```java
ChromeOptions options = new ChromeOptions();
Map<String, Object> prefs = new HashMap<>();
prefs.put("intl.accept_languages", "en-US");
options.setExperimentalOption("prefs", prefs);

driver = new ChromeDriver(options);
```

**Prevention:**
- Set locale explicitly
- Test with different locales
- Handle locale-specific elements

---

## 9️⃣ DATA MANAGEMENT ISSUES

### Issue 9.1: Test Data Not Loading from Files
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
FileNotFoundException: testdata.json (No such file or directory)
# Or
NullPointerException when accessing test data
```

**Root Cause:**
- File path incorrect
- File not in correct location
- Wrong classpath

**Solution:**

**Correct file location:**
```
src/
  test/
    resources/          ← Place data files here
      testdata.json
      users.csv
      config.properties
```

**Load from classpath:**
```java
// ✅ GOOD - Load from classpath
InputStream inputStream = getClass()
    .getClassLoader()
    .getResourceAsStream("testdata.json");

if (inputStream == null) {
    throw new RuntimeException("testdata.json not found in classpath");
}

// Parse JSON
ObjectMapper mapper = new ObjectMapper();
TestData data = mapper.readValue(inputStream, TestData.class);
```

**Alternative - Use absolute path:**
```java
// Load from project root
File file = new File("src/test/resources/testdata.json");
TestData data = mapper.readValue(file, TestData.class);
```

**Verification:**
```bash
# Check file exists
ls src/test/resources/testdata.json

# Run test
mvn test -Dtest=DataLoadTest
```

**Prevention:**
- Always use src/test/resources for test data
- Use classpath resource loading
- Handle FileNotFoundException properly

---

### Issue 9.2: Database Connection Failures in Tests
**Severity:** 🟠 High

**Triệu chứng:**
```bash
Unable to acquire JDBC Connection
# Or
Communications link failure
```

**Root Cause:**
- Database not started
- Wrong connection URL
- Network issues

**Solution:**

**Use in-memory H2 for tests:**
```properties
# src/test/resources/application-test.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.hibernate.ddl-auto=create-drop
```

**Use test profile:**
```java
@SpringBootTest
@ActiveProfiles("test")  // Use application-test.properties
public class DatabaseTest {
    // Tests
}
```

**Verify connection:**
```java
@Autowired
private DataSource dataSource;

@Test
public void testDatabaseConnection() throws SQLException {
    try (Connection connection = dataSource.getConnection()) {
        Assert.assertTrue(connection.isValid(1));
    }
}
```

**Prevention:**
- Use H2 in-memory for tests
- Separate test database configuration
- Test database setup in CI/CD

---

### Issue 9.3: Test Data Cleanup Incomplete
**Severity:** 🟡 Medium

**Triệu chứng:**
```bash
# Database fills up with test data
# Tests fail due to duplicate data
```

**Root Cause:**
- Cleanup not executed
- Transactions not rolled back
- Exception prevents cleanup

**Solution:**

**Use @Transactional with rollback:**
```java
@SpringBootTest
@Transactional  // Auto-rollback after each test
public class UserServiceTest {

    @Test
    public void testCreateUser() {
        User user = userService.create("test@test.com");
        Assert.assertNotNull(user);
        // Auto-rolled back after test
    }
}
```

**Manual cleanup in finally:**
```java
@Test
public void testUserFlow() {
    String userId = null;
    try {
        userId = createUser();
        // Test operations
    } finally {
        if (userId != null) {
            deleteUser(userId);
        }
    }
}
```

**@AfterMethod cleanup:**
```java
private List<String> createdUserIds = new ArrayList<>();

@Test
public void testUser() {
    String userId = createUser();
    createdUserIds.add(userId);
    // Test
}

@AfterMethod(alwaysRun = true)
public void cleanup() {
    for (String userId : createdUserIds) {
        deleteUser(userId);
    }
    createdUserIds.clear();
}
```

**Prevention:**
- Use @Transactional for automatic rollback
- Always use finally blocks
- Track created data for cleanup

---

## 🛠️ DEBUGGING TECHNIQUES

### Technique 1: Print Page Source
```java
public void debugPageSource() {
    String pageSource = driver.getPageSource();
    System.out.println("=== PAGE SOURCE ===");
    System.out.println(pageSource);

    // Or save to file
    try (FileWriter writer = new FileWriter("debug-page-source.html")) {
        writer.write(pageSource);
    }
}
```

### Technique 2: Take Debug Screenshots
```java
public void takeDebugScreenshot(String name) {
    File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
    File destination = new File("debug-screenshots/" + name + ".png");
    FileUtils.copyFile(screenshot, destination);
    logger.info("Debug screenshot: " + destination.getAbsolutePath());
}

// Usage
takeDebugScreenshot("before-click");
element.click();
takeDebugScreenshot("after-click");
```

### Technique 3: Log Element Properties
```java
public void debugElement(WebElement element) {
    logger.info("Element tag: " + element.getTagName());
    logger.info("Element text: " + element.getText());
    logger.info("Element displayed: " + element.isDisplayed());
    logger.info("Element enabled: " + element.isEnabled());
    logger.info("Element location: " + element.getLocation());
    logger.info("Element size: " + element.getSize());

    // Log all attributes
    JavascriptExecutor js = (JavascriptExecutor) driver;
    String script = "var items = {}; " +
                    "for (index = 0; index < arguments[0].attributes.length; ++index) { " +
                    "items[arguments[0].attributes[index].name] = arguments[0].attributes[index].value }; " +
                    "return items;";
    Map<String, String> attributes = (Map<String, String>) js.executeScript(script, element);
    logger.info("Element attributes: " + attributes);
}
```

### Technique 4: Use Browser DevTools Protocol
```java
public void enableDevToolsLogging() {
    LoggingPreferences logs = new LoggingPreferences();
    logs.enable(LogType.BROWSER, Level.ALL);
    logs.enable(LogType.PERFORMANCE, Level.ALL);

    ChromeOptions options = new ChromeOptions();
    options.setCapability(CapabilityType.LOGGING_PREFS, logs);

    driver = new ChromeDriver(options);
}

public void printBrowserLogs() {
    LogEntries logEntries = driver.manage().logs().get(LogType.BROWSER);
    for (LogEntry entry : logEntries) {
        logger.info(entry.getLevel() + " " + entry.getMessage());
    }
}
```

### Technique 5: Breakpoint Debugging in IDE
```java
@Test
public void testWithBreakpoint() {
    driver.get("https://example.com");

    // Set breakpoint here in IDE
    WebElement element = driver.findElement(By.id("username"));

    // Inspect 'element' variable
    // Check driver state
    // Verify page source

    element.sendKeys("test");
}
```

### Technique 6: Add Debug Waits
```java
public void debugWait(int seconds, String reason) {
    logger.info("Waiting " + seconds + " seconds: " + reason);
    try {
        Thread.sleep(seconds * 1000);
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
}

// Usage - for visual debugging
loginPage.fillEmail("test@test.com");
debugWait(2, "Visual check - email filled");
loginPage.fillPassword("password");
debugWait(2, "Visual check - password filled");
```

### Technique 7: Highlight Elements
```java
public void highlightElement(WebElement element) {
    JavascriptExecutor js = (JavascriptExecutor) driver;
    String originalStyle = element.getAttribute("style");

    // Highlight
    js.executeScript("arguments[0].setAttribute('style', 'border: 3px solid red;')", element);

    try {
        Thread.sleep(500);  // Show highlight
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }

    // Restore
    js.executeScript("arguments[0].setAttribute('style', '" + originalStyle + "')", element);
}
```

### Technique 8: Network Traffic Analysis
```java
public void enableNetworkLogging() {
    ChromeOptions options = new ChromeOptions();
    options.setCapability("goog:loggingPrefs", 
        Map.of("performance", "ALL"));

    driver = new ChromeDriver(options);
}

public void analyzeNetworkTraffic() {
    LogEntries logs = driver.manage().logs().get("performance");
    for (LogEntry entry : logs) {
        JSONObject json = new JSONObject(entry.getMessage());
        JSONObject message = json.getJSONObject("message");
        String method = message.getString("method");

        if (method.contains("Network")) {
            logger.info("Network event: " + message);
        }
    }
}
```

---

## 🚨 EMERGENCY TROUBLESHOOTING WORKFLOW

When stuck with an unknown error, follow this systematic approach:

### Step 1: Identify Error Category (5 min)
```
✓ Is this a SETUP error? (Can't start)
   → Go to Section 1: Setup & Environment

✓ Is this a BUILD error? (Compilation/Dependencies)
   → Go to Section 2: Dependency & Build

✓ Is this a RUNTIME error? (During test execution)
   → Go to Section 3: Runtime Errors

✓ Is this a CI/CD error? (Works locally, fails in CI)
   → Go to Section 5.2: CI/CD Issues

✓ Is this a PERFORMANCE error? (Too slow)
   → Go to Section 7: Performance
```

### Step 2: Gather Information (10 min)
```bash
# Collect evidence
1. Full error stack trace
2. Test logs (application + test)
3. Screenshot (if UI-related)
4. Page source (if element-related)
5. Browser console logs

# Commands to gather info
mvn clean test > full-test-output.log 2>&1
cat logs/automation.log
cat target/surefire-reports/*.txt
```

### Step 3: Search This Guide (5 min)
```
1. Ctrl+F → Search error message
2. Check Table of Contents
3. Use Quick Search Index
4. Review Decision Tree
```

### Step 4: Try Standard Fixes (15 min)
```java
// Standard fix checklist
□ Clean and rebuild: mvn clean install -U
□ Restart IDE
□ Restart browser/system
□ Update dependencies
□ Check recent code changes
□ Increase timeouts
□ Add explicit waits
□ Verify locators
□ Check logs
```

### Step 5: Debug Systematically (30 min)
```java
// Enable debug mode
1. Add extensive logging
2. Take screenshots at each step
3. Print page source
4. Use breakpoint debugging
5. Run test in isolation
6. Simplify test to minimal reproducible case
```

### Step 6: Isolate the Problem (20 min)
```
Test progressively:
□ Does it work locally? → Environment issue
□ Does it work in other browser? → Browser issue
□ Does it work for other tests? → Test-specific issue
□ Does it work with simple locator? → Locator issue
□ Does it work with longer wait? → Timing issue
```

### Step 7: Document and Escalate (10 min)
If still stuck after 90 minutes:

```markdown
**Issue Report Template:**

**Environment:**
- OS: Windows 10 / Mac / Linux
- Java: java -version output
- Maven: mvn -version output
- Browser: Chrome 120.0.xxxx
- Framework version: 1.0.0

**Error:**
```
[Full stack trace]
```

**Steps to Reproduce:**
1. Clone repo
2. Run: mvn clean test -Dtest=LoginTest
3. Error occurs at line 45

**Expected vs Actual:**
- Expected: Login successful
- Actual: NoSuchElementException

**Already Tried:**
- Cleaned and rebuilt
- Updated dependencies
- Added explicit waits
- Verified locator in DevTools

**Attachments:**
- Screenshot: error-screenshot.png
- Logs: test-output.log
- Page source: page-source.html
```

### Step 8: Prevention for Future
```java
// After resolving, document:
1. Add issue to this guide if new
2. Create test case to prevent regression
3. Update README if setup-related
4. Share solution with team
5. Add to FAQ if common
```

---

## 📚 GETTING HELP

### Internal Resources

**Framework Documentation:**
- File 00: Master Index - Overview and navigation
- File 01: Architecture - Framework structure
- File 11: Best Practices - Coding standards
- File 12: This file - Troubleshooting

**Team Contacts:**
- Framework Lead: [Name/Email]
- Test Automation Team: [Slack Channel]
- DevOps Support: [Email/Ticket System]

### External Resources

**Official Documentation:**
- Selenium: https://www.selenium.dev/documentation/
- TestNG: https://testng.org/doc/documentation-main.html
- Spring Boot: https://spring.io/projects/spring-boot
- Maven: https://maven.apache.org/guides/

**Community Support:**
- Stack Overflow: Tag [selenium], [testng], [selenium-webdriver]
- Selenium Slack: https://www.selenium.dev/support/
- GitHub Issues: Framework repository

**Learning Resources:**
- Selenium WebDriver Tutorial: https://www.guru99.com/selenium-tutorial.html
- TestNG Tutorial: https://testng.org/doc/tutorials.html
- Best Practices: File 11 in this framework

### When to Escalate

**Escalate to Team Lead if:**
- Issue blocks testing for > 4 hours
- CI/CD pipeline completely broken
- Security-related issues
- Infrastructure problems

**Escalate to DevOps if:**
- CI/CD environment issues
- Network/firewall problems
- Server access issues
- Docker/container problems

**Escalate to Development Team if:**
- Application bugs blocking tests
- Missing test IDs/attributes
- Performance issues in AUT
- API changes breaking tests

### Escalation Template

```markdown
**Escalation: [Brief Issue Description]**

**Priority:** Critical / High / Medium / Low

**Impact:**
- Tests affected: 15/50 (30%)
- Timeline blocked: 2 days
- Release at risk: Yes/No

**Issue Summary:**
[Brief description]

**Investigation Done:**
- Time spent: 4 hours
- Attempted fixes: [list]
- Root cause hypothesis: [explanation]

**Request:**
[What you need: infrastructure access, dev support, etc.]

**Attachments:**
- Error logs
- Screenshots
- Test reports
```

---

## ✅ KNOWN LIMITATIONS & WORKAROUNDS

### Limitation 1: Chrome Auto-Update
**Limitation:** Chrome auto-updates can break ChromeDriver compatibility

**Workaround:**
```java
// Use WebDriverManager for automatic driver management
WebDriverManager.chromedriver().setup();

// Or disable Chrome auto-update (enterprise)
// Group Policy: Prevent Chrome auto-updates
```

### Limitation 2: Headless Mode Differences
**Limitation:** Some elements render differently in headless mode

**Workaround:**
```java
// Use new headless mode (Chromium 109+)
options.addArguments("--headless=new");

// Or run specific tests in headed mode
if (testName.equals("visualTest")) {
    // Don't add --headless
}
```

### Limitation 3: File Download in Headless
**Limitation:** File downloads don't work well in headless Chrome

**Workaround:**
```java
ChromeOptions options = new ChromeOptions();
Map<String, Object> prefs = new HashMap<>();
prefs.put("download.default_directory", downloadPath);
prefs.put("download.prompt_for_download", false);
options.setExperimentalOption("prefs", prefs);

// Enable downloads in headless
options.addArguments("--headless=new");
options.addArguments("--disable-gpu");

// Use Chrome DevTools Protocol
Map<String, Object> commandParams = new HashMap<>();
commandParams.put("behavior", "allow");
commandParams.put("downloadPath", downloadPath);
((ChromeDriver) driver).executeCdpCommand("Page.setDownloadBehavior", commandParams);
```

### Limitation 4: iframe Interaction Issues
**Limitation:** Some dynamic iframes are hard to detect

**Workaround:**
```java
// Wait for iframe to be available
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.frameToBeAvailableAndSwitchToIt(By.id("iframe")));

// Do operations in iframe
driver.findElement(By.id("element")).click();

// Always switch back
driver.switchTo().defaultContent();
```

### Limitation 5: Windows Authentication Popups
**Limitation:** Selenium can't handle OS-level authentication dialogs

**Workaround:**
```java
// Pass credentials in URL
String url = "https://username:password@example.com/page";
driver.get(url);

// Or use AutoIT for Windows
// Or configure ChromeOptions to avoid popup
options.addArguments("--auth-server-whitelist=*");
options.addArguments("--auth-negotiate-delegate-whitelist=*");
```

### Limitation 6: CAPTCHA in Tests
**Limitation:** CAPTCHA prevents automation

**Workaround:**
```
1. Disable CAPTCHA in test environment
2. Use test API key that bypasses CAPTCHA
3. Mock CAPTCHA response in test environment
4. Never test production with CAPTCHA enabled
```

### Limitation 7: Shadow DOM Elements
**Limitation:** Standard locators don't work with Shadow DOM

**Workaround:**
```java
public WebElement getShadowElement(String shadowHost, String selector) {
    JavascriptExecutor js = (JavascriptExecutor) driver;
    WebElement shadowRoot = (WebElement) js.executeScript(
        "return document.querySelector(arguments[0]).shadowRoot.querySelector(arguments[1])",
        shadowHost, selector
    );
    return shadowRoot;
}
```

---

## 📝 MAINTENANCE & UPDATES

**Last Updated:** October 2025  
**Version:** 1.0  
**Applies to:** Framework Files 01-11

**Update History:**
- v1.0 (Oct 2025): Initial release covering 55+ issues

**Contribution:**
If you encounter new issues not covered in this guide:
1. Document the issue following the format in this guide
2. Add solution with verification steps
3. Create pull request or notify framework team
4. Include in next version update

**Feedback:**
- Report missing issues: [Framework Team Email]
- Suggest improvements: [GitHub Issues]
- Request clarifications: [Slack Channel]

---



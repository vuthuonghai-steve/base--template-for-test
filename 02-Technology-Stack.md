# 📚 TECHNOLOGY STACK - CÔNG NGHỆ & PHIÊN BẢN

## MỤC ĐÍCH FILE NÀY

File này cung cấp **specification chi tiết** về công nghệ và phiên bản được sử dụng trong Test Automation Framework. Đây là reference để setup môi trường development.

---

## TECH STACK OVERVIEW

```
┌─────────────────────────────────────────────────────────────┐
│                    TECHNOLOGY STACK                         │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: CORE PLATFORM                                     │
│  ├─ Java 21 (LTS)                                           │
│  ├─ Maven 3.8+                                              │
│  └─ IntelliJ IDEA                                           │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: AUT (Application Under Test)                     │
│  ├─ Spring Boot 3.2.x                                       │
│  ├─ Thymeleaf 3.1.x                                         │
│  ├─ H2 Database 2.2.x                                       │
│  ├─ Bootstrap 5.3.x                                         │
│  └─ jQuery 3.7.x                                            │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: TAF CORE (Test Framework)                        │
│  ├─ Selenium WebDriver 4.16.x                               │
│  └─ TestNG 7.8.x                                            │
├─────────────────────────────────────────────────────────────┤
│  Layer 4: LEVEL 2 FEATURES (Optional)                      │
│  ├─ ExtentReports 5.1.x                                     │
│  ├─ SLF4J 2.0.x                                             │
│  ├─ Logback 1.4.x                                           │
│  ├─ Jackson (JSON) 2.15.x                                   │
│  ├─ Apache Commons CSV 1.10.x                               │
│  └─ Apache POI (Excel) 5.2.x                                │
└─────────────────────────────────────────────────────────────┘
```

---

## LAYER 1: CORE PLATFORM

### Java Development Kit

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 21 (LTS) |
| **Release** | September 2023 |
| **Support Until** | September 2028 |
| **Download** | https://adoptium.net/ |

**Verification:**
```bash
java -version
# Expected: openjdk version "21.0.x" 2024-xx-xx LTS
```

**Why Java 21?**
- ✅ Long-term support đến 2028
- ✅ Virtual threads (useful for parallel tests)
- ✅ Modern features: Pattern matching, Records, Text blocks

---

### Apache Maven

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 3.8.0+ (Recommended: 3.9.6) |
| **Download** | https://maven.apache.org/download.cgi |

**Verification:**
```bash
mvn -version
# Expected: Apache Maven 3.9.x, Java version: 21.0.x
```

**Required Plugins:**
- `maven-compiler-plugin` 3.12.1
- `maven-surefire-plugin` 3.2.5
- `maven-resources-plugin` 3.3.1

---

### IDE - IntelliJ IDEA

| Attribute | Specification |
|:----------|:--------------|
| **Edition** | Community (Free) hoặc Ultimate |
| **Version** | 2024.1+ |
| **Download** | https://www.jetbrains.com/idea/download/ |

**Required Plugins:**
- Maven (Built-in)
- TestNG (Built-in or from marketplace)

---

## LAYER 2: APPLICATION UNDER TEST

### Spring Boot Framework

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 3.2.x (Latest: 3.2.5) |
| **Minimum Java** | 17 (Works with 21) |
| **Documentation** | https://docs.spring.io/spring-boot/ |

**Core Dependencies:**
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.5</version>
</parent>

<!-- Web MVC -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- Thymeleaf Template Engine -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

<!-- JPA + Database -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

### Thymeleaf Template Engine

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 3.1.x (Managed by Spring Boot) |
| **Documentation** | https://www.thymeleaf.org/ |

**Configuration:**
```properties
# application.properties
spring.thymeleaf.cache=false
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

---

### H2 Database

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 2.2.x (Latest: 2.2.224) |
| **Type** | In-memory, Embedded |
| **Console** | http://localhost:8080/h2-console |

**Configuration:**
```properties
# application.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto=update
```

**Why H2?**
- ✅ In-memory: Fast, reset on restart
- ✅ Zero configuration
- ✅ Lightweight (~2MB)

---

### Front-end Libraries (CDN)

#### Bootstrap
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" 
      rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

#### jQuery
```html
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
```

---

## LAYER 3: TEST AUTOMATION FRAMEWORK CORE

### Selenium WebDriver

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 4.16.x (Latest: 4.16.1) |
| **Release** | December 2023 |
| **Documentation** | https://www.selenium.dev/documentation/ |

**Maven Dependency:**
```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.16.1</version>
</dependency>
```

**Selenium 4 Features:**
- ✅ Auto driver management (no need WebDriverManager)
- ✅ Relative Locators
- ✅ Better window management
- ✅ Chrome DevTools Protocol support

**Browser Drivers:**
| Browser | Auto-managed by Selenium 4 |
|:--------|:-------------------------:|
| Chrome  | ✅ Yes |
| Firefox | ✅ Yes |
| Edge    | ✅ Yes |

---

### TestNG

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 7.8.x (Latest: 7.8.0) |
| **Release** | October 2023 |
| **Documentation** | https://testng.org/doc/ |

**Maven Dependency:**
```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.8.0</version>
    <scope>test</scope>
</dependency>
```

**Key Features:**
| Feature | Annotation/Config |
|:--------|:-----------------|
| Test Methods | `@Test` |
| Data Providers | `@DataProvider` |
| Setup/Teardown | `@BeforeMethod`, `@AfterMethod` |
| Listeners | `ITestListener`, `IRetryAnalyzer` |
| Parallel Execution | `parallel="methods"` in XML |
| Test Groups | `@Test(groups = {"smoke"})` |

---

## LAYER 4: LEVEL 2 FEATURES (OPTIONAL)

### ExtentReports

| Attribute | Specification |
|:----------|:--------------|
| **Version** | 5.1.x (Latest: 5.1.1) |
| **Type** | HTML Reporting Library |

**Maven Dependency:**
```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.1.1</version>
</dependency>
```

---

### Logging - SLF4J + Logback

**SLF4J (API):**
```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.9</version>
</dependency>
```

**Logback (Implementation):**
```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.4.14</version>
</dependency>
```

---

### Data Parsing Libraries

**Jackson (JSON):**
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.3</version>
</dependency>
```

**Apache Commons CSV:**
```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.10.0</version>
</dependency>
```

**Apache POI (Excel):**
```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.2.5</version>
</dependency>
```

---

## VERSION COMPATIBILITY MATRIX

| Java Version | Spring Boot | Selenium | TestNG | Status |
|:------------:|:-----------:|:--------:|:------:|:------:|
| 11 (LTS) | 3.2.x | 4.16.x | 7.8.x | ✅ Supported |
| 17 (LTS) | 3.2.x | 4.16.x | 7.8.x | ✅ Recommended |
| 21 (LTS) | 3.2.x | 4.16.x | 7.8.x | ✅ Production-Ready |

---

## MINIMAL POM.XML TEMPLATE

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>{{PROJECT_NAME}}</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <!-- Spring Boot Parent -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
    </parent>

    <properties>
        <java.version>21</java.version>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <selenium.version>4.16.1</selenium.version>
        <testng.version>7.8.0</testng.version>
    </properties>

    <dependencies>
        <!-- Spring Boot Starters -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- H2 Database -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Selenium & TestNG -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>${selenium.version}</version>
        </dependency>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- Add Level 2 Features here based on Feature Selection Matrix -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.5</version>
                <configuration>
                    <suiteXmlFiles>
                        <suiteXmlFile>src/test/resources/testng/testng.xml</suiteXmlFile>
                    </suiteXmlFiles>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## DEPENDENCY SIZE BREAKDOWN

| Dependency Group | Size | Purpose |
|:-----------------|:----:|:--------|
| Spring Boot + H2 | ~50 MB | AUT runtime |
| Selenium WebDriver | ~20 MB | Browser automation |
| TestNG | ~2 MB | Test framework |
| Level 2 Features | ~20 MB | Optional features |
| **Total** | **~92 MB** | Complete stack |

---

## COMMON ISSUES & SOLUTIONS

### Issue: "Class not found: ChromeDriver"
**Solution:** Selenium 4 auto-manages, ensure internet connection

### Issue: "TestNG tests not discovered"
**Solution:** Check maven-surefire-plugin configuration in pom.xml

### Issue: "H2 console 404 not found"
**Solution:** 
```properties
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

---

## NEXT STEPS

✅ **Đã setup xong môi trường?** → Tiếp tục với [03-Problem-Analysis-Framework.md](../LAYER-2-ANALYSIS/03-Problem-Analysis-Framework.md)

✅ **Cần reference chi tiết hơn?** → Xem file `tech-version-v2.md` đầy đủ

✅ **Sẵn sàng code?** → Follow Process Guide từ File 06-09

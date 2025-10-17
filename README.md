# 🎯 HỆ THỐNG TEMPLATE TEST AUTOMATION FRAMEWORK (TAF)

### 1. Giới thiệu Tổng quan

Dự án này là một **Hệ thống Template Modular** được thiết kế để xây dựng Test Automation Framework (TAF) cho các **ứng dụng đã tồn tại (staging/production)**.

Hệ thống hoạt động theo **Real-World Mode** (Chế độ Thế giới Thực), tập trung hoàn toàn vào việc xây dựng code ở lớp `src/test/`.

**Mục đích cốt lõi:**
*   Hỗ trợ triển khai test automation cho các ứng dụng đã được triển khai (deployed).
*   **KHÔNG TẠO ỨNG DỤNG** - CHỈ XÂY DỰNG TAF (Milestones M3, M4, M5).
*   Yêu cầu bắt buộc là phải có **URL staging/production** trước khi bắt đầu viết test.

**Phiên bản hiện tại:** `3.0.0` (Real-World Mode - TAF Only).

### 2. Công nghệ (Technology Stack)

Framework được xây dựng dựa trên các công nghệ sau:

| Thành phần | Công nghệ | Phiên bản | Mục đích |
| :--- | :--- | :--- | :--- |
| **Ngôn ngữ** | Java Development Kit | 21 (LTS) | Nền tảng core |
| **Build Tool** | Apache Maven | 3.8.0+ | Quản lý dependencies |
| **Automation** | Selenium WebDriver | 4.16.x | Tự động hóa trình duyệt |
| **Testing** | TestNG | 7.8.x | Framework chạy test, data providers, listeners |
| **Config Core** | Spring Boot | 3.2.x | Quản lý configuration (Lớp TAF) |
| **Reporting** | ExtentReports | 5.1.x | Báo cáo HTML chuyên nghiệp (Optional Level 2 Feature) |
| **Logging** | SLF4J + Logback | N/A | Structured logging |

### 3. Kiến trúc Framework (TAF ONLY)

Kiến trúc TAF tuân thủ mô hình **Page Object Model (POM)** nghiêm ngặt và được tổ chức trong thư mục `src/test/`.

#### 3.1 Cấu trúc Thư mục Quan trọng

Dự án **KHÔNG** chứa thư mục `src/main/` vì ứng dụng chạy từ xa (REMOTE).

| Thư mục | Vai trò | Chi tiết |
| :--- | :--- | :--- |
| **src/test/config** | Quản lý Cấu hình | Chứa `TestConfig.java` và `config.properties` để kết nối tới **REAL URLs** (https://...). |
| **src/test/listeners** | TestNG Event Hooks | Chứa `TestListener.java` (Screenshot, Report) và `RetryAnalyzer.java`. |
| **src/test/pages** | Page Object Model (POM) | Chứa `BasePage.java` và các lớp Page Object cụ thể (`{{PAGE_NAME}}Page.java`). |
| **src/test/tests** | Test Scenarios | Chứa `BaseTest.java` và các lớp Test (`{{TEST_CLASS}}Test.java`). |
| **src/test/utils** | Utilities | Chứa các lớp hỗ trợ như `WaitHelper.java`, `DataHelper.java`, `ScreenshotHelper.java`. |
| **reports/** | Output | Chứa `ExtentReport.html` và `testng-results.xml`. |
| **screenshots/** | Output | Chứa ảnh chụp màn hình các tests bị Fail. |

#### 3.2 Các Tính năng Mở rộng (Level 2 Features)

Tùy thuộc vào độ phức tạp của bài toán, các tính năng sau được triển khai trong Milestone 4 (M4):

| Tính năng | Mục đích | Template Áp dụng (Ví dụ) |
| :--- | :--- | :--- |
| **Data-Driven Testing** | Chạy test với nhiều bộ dữ liệu (JSON/CSV). | Template B (Complex Form). |
| **ExtentReports** | Tạo báo cáo HTML chuyên nghiệp. | Templates B, C, D, E. |
| **Explicit Waits** | Dynamic waits thay cho `Thread.sleep()`. | BẮT BUỘC cho mọi Template (Phase 1). |
| **Retry Mechanism** | Tự động chạy lại các test bị flaky. | Template B, D. |

### 4. Quy trình làm việc Chuẩn (4 Bước)

Mọi dự án Test Automation mới phải tuân thủ quy trình 4 bước sau, bắt đầu bằng việc xác minh URL ứng dụng:

| Bước | Tên File (Bắt đầu) | Hoạt động Chính | Checkpoint Output |
| :--- | :--- | :--- | :--- |
| **Bước 1** | `06-Process-Step-1-Analysis.md` | **Problem Analysis**: Thu thập yêu cầu, phân loại form (Simple/Complex/CRUD). | **Problem Analysis Document** (Phải được user CONFIRM). |
| **Bước 2** | `07-Process-Step-2-Design.md` | **Solution Design**: Chọn Template (File 04), chọn Features (File 05), lập Roadmap. | **Implementation Plan Document** (Phải được user APPROVE). |
| **Bước 3** | `08-Process-Step-3-Overview.md` | **Implementation (M3, M4)**: Viết code TAF Core (M3) và Level 2 Features (M4) theo Milestones. | **Working TAF Code** (Tests chạy pass &gt;90%). |
| **Bước 4** | `09-Process-Step-4-Execution.md` | **Finalization**: Chạy Full Test Suite, tạo Test Reports và Documentation (README, Test Cases Excel). | **Final Deliverables** (Code, Reports, README.md, TestCases.xlsx). |

### 5. Thư viện Templates (Template Library)

Hệ thống cung cấp 5 template đã được tối ưu hóa, được chọn sau khi hoàn thành phân tích bài toán (File 03):

| Template | File Chi tiết | Mô tả Use Case | Độ phức tạp |
| :--- | :--- | :--- | :--- |
| **A** | `10A-Template-Simple-Form.md` | Form đơn giản (&lt;10 fields, validation cơ bản). | ⭐ |
| **B** | `10B-Template-Complex-Form.md` | Form phức tạp (Business logic, cross-field validation). | ⭐⭐⭐ |
| **C** | `10C-Template-Multi-Step.md` | Form nhiều bước (Wizard, 2+ steps, quản lý state). | ⭐⭐ |
| **D** | `10D-Template-CRUD.md` | Quản lý thực thể (Create, Read, Update, Delete). | ⭐⭐⭐ |
| **E** | `10E-Template-Search-Filter.md` | Chức năng tìm kiếm, lọc và phân trang kết quả. | ⭐⭐ |

### 6. Hướng dẫn tham khảo nhanh

Sử dụng bảng sau để điều hướng trong kho tài liệu này:

| Nếu bạn muốn... | Tham khảo File | Mục đích chính |
| :--- | :--- | :--- |
| **Bắt đầu một dự án mới** | `03-Problem-Analysis-Framework.md` | Phân tích yêu cầu và xác định bài toán. |
| **Chọn Template phù hợp** | `04-Decision-Tree.md` | Dựa vào phân tích để quyết định sử dụng Template nào. |
| **Thiết kế tính năng mở rộng** | `05-Feature-Selection-Matrix.md` | Quyết định 8 Level 2 Features nào cần triển khai. |
| **Tìm kiến trúc code chuẩn** | `01-Framework-Architecture.md` | Tham khảo cấu trúc thư mục, Page Objects, và naming conventions. |
| **Tìm Naming Conventions** | `11-Best-Practices.md` | Tiêu chuẩn đặt tên Page Objects, Test Methods, và WebElements. |
| **Xử lý lỗi runtime** | `12-Troubleshooting-Guide.md` | Hướng dẫn giải quyết `NoSuchElementException`, driver mismatch, và các lỗi build/runtime khác. |
| **Biết bước tiếp theo** | `06-Process-Step-1-Analysis.md` (nếu mới bắt đầu) | Bắt đầu quy trình 4 bước. |

### 7. Hướng dẫn Khởi chạy (Running Tests)

Do đây là TAF ONLY, bạn cần đảm bảo ứng dụng đang chạy trên một máy chủ từ xa (remote server).

#### 7.1 Cài đặt môi trường

1.  **Cài đặt Java 21 (LTS)**.
2.  **Cài đặt Apache Maven** (3.8.0+).
3.  **Clone repository:**
    ```bash
    git clone [repository URL]
    ```
4.  **Cài đặt dependencies:**
    ```bash
    mvn clean install
    ```
5.  **Cấu hình URL:** Chỉnh sửa `src/test/resources/config/config.properties` để trỏ đến **URL thực tế** của ứng dụng (ví dụ: `BASE_URL=https://staging.example.com/`).

#### 7.2 Chạy Test

Để chạy toàn bộ test suite sử dụng TestNG:

```bash
mvn test -Dsurefire.suiteXmlFiles=testng.xml
```

#### 7.3 Xem Báo cáo

*   **Báo cáo HTML (ExtentReports):** Mở `reports/ExtentReport.html` trong trình duyệt.
*   **Logs:** Xem file `logs/automation.log`.

### 8. Ghi chú Quan trọng (Real-World Mode)

*   **Ứng dụng Tồn tại:** Chúng tôi giả định ứng dụng (AUT) đã được triển khai.
*   **Không dùng Localhost:** Tuyệt đối không sử dụng URL `localhost` trong cấu hình.
*   **Dữ liệu Test:** Nên sử dụng dữ liệu test **duy nhất (unique)** (ví dụ: timestamp, UUID) và có chiến lược dọn dẹp dữ liệu (cleanup).
*   **Authentication:** Nếu ứng dụng yêu cầu đăng nhập, cần triển khai `LoginPage.java` trước tiên.

### 9. Liên hệ và Hỗ trợ

*   Nếu phát hiện vấn đề hoặc cần đề xuất cải tiến (về template, quy trình, hoặc troubleshooting), vui lòng tham khảo phần `FEEDBACK & IMPROVEMENT` trong **File 00-MASTER-INDEX.md** hoặc liên hệ.
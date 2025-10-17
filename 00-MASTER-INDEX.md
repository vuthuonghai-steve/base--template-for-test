# 🎯 MASTER INDEX - HỆ THỐNG TEMPLATE TEST AUTOMATION

## MỤC ĐÍCH CỦA HỆ THỐNG

Đây là hệ thống template modular dành cho **Test Automation Framework (TAF)** cho ứng dụng thực tế, được thiết kế để:

✅ Hỗ trợ AI Chatbot thực hiện test automation cho **ứng dụng ĐÃ TỒN TẠI** (staging/production)
✅ **KHÔNG TẠO ỨNG DỤNG** - CHỈ XÂY DỰNG TAF (src/test/ layer)
✅ Áp dụng cho nhiều loại bài toán khác nhau (form testing, CRUD, search/filter)
✅ Cung cấp quy trình làm việc 4 bước rõ ràng với checkpoints
✅ Module hóa để dễ maintain và mở rộng

⚠️ **QUAN TRỌNG**: Hệ thống này hướng dẫn viết test cho ứng dụng REMOTE/STAGING, KHÔNG phải localhost. Ứng dụng phải được triển khai TRƯỚC KHI bắt đầu viết test.  

---

## CẤU TRÚC HỆ THỐNG 5 TẦNG

### TẦNG 1: CORE FRAMEWORK DEFINITION 📚
*Định nghĩa kiến trúc và công nghệ cơ bản*

| File | Mục đích | Khi nào đọc |
|:-----|:---------|:------------|
| [01-Framework-Architecture.md](LAYER-1-CORE/01-Framework-Architecture.md) | Kiến trúc tổng quan của framework, cấu trúc thư mục generic | Khi bắt đầu dự án mới |
| [02-Technology-Stack.md](LAYER-1-CORE/02-Technology-Stack.md) | Tech stack chi tiết: Java 21, Selenium 4, TestNG 7, Spring Boot 3.2 | Khi setup môi trường |

---

### TẦNG 2: PROBLEM ANALYSIS FRAMEWORK 🔍
*Phân tích và phân loại bài toán*

| File | Mục đích | Khi nào đọc |
|:-----|:---------|:------------|
| [03-Problem-Analysis-Framework.md](LAYER-2-ANALYSIS/03-Problem-Analysis-Framework.md) | Checklist phân tích bài toán: loại form, fields, validation rules | **BẮT ĐẦU MỌI DỰ ÁN** - Đọc đầu tiên |
| [04-Decision-Tree.md](LAYER-2-ANALYSIS/04-Decision-Tree.md) | Cây quyết định để chọn template phù hợp | Sau khi phân tích xong bài toán |
| [05-Feature-Selection-Matrix.md](LAYER-2-ANALYSIS/05-Feature-Selection-Matrix.md) | Ma trận chọn Level 2 features dựa trên độ phức tạp | Khi thiết kế solution |

---
 
### TẦNG 3: PROCESS GUIDE 🔄
*Quy trình làm việc 4 bước chi tiết*

| File | Mục đích | Khi nào đọc |
|:-----|:---------|:------------|
| [06-Process-Step-1-Analysis.md](LAYER-3-PROCESS/06-Process-Step-1-Analysis.md) | **Bước 1**: Thu thập yêu cầu và phân tích bài toán | Bắt đầu dự án |
| [07-Process-Step-2-Design.md](LAYER-3-PROCESS/07-Process-Step-2-Design.md) | **Bước 2**: Thiết kế solution và implementation plan | Sau khi người dùng confirm Bước 1 |
| [08-Process-Step-3-Overview.md](LAYER-3-PROCESS/08-Process-Step-3-Overview.md) | **Bước 3**: Triển khai code với checkpoints (đã split thành 08-Overview, 08A-M3, 08B-M4, 08C-M5) | Sau khi người dùng approve plan |
| [09-Process-Step-4-Execution.md](LAYER-3-PROCESS/09-Process-Step-4-Execution.md) | **Bước 4**: Chạy test và tạo documentation | Khi code hoàn thành |

---

### TẦNG 4: TEMPLATE LIBRARY 📋
*Templates con cho từng loại bài toán*

| File | Mục đích | Khi nào đọc |
|:-----|:---------|:------------|
| [10-Template-Library-Index.md](LAYER-4-TEMPLATES/10-Template-Library-Index.md) | 5 templates: Simple Form, Complex Form, Multi-Step, CRUD, Search/Filter (đã split thành 10-Index, 10A-E) | Khi đã biết loại bài toán từ Decision Tree |

---

### TẦNG 5: TECHNICAL REFERENCE 📖
*Kiến thức kỹ thuật và xử lý lỗi*

| File | Mục đích | Khi nào đọc |
|:-----|:---------|:------------|
| [11-Best-Practices.md](LAYER-5-REFERENCE/11-Best-Practices.md) | Best practices: naming, POM, error handling, wait strategy | Khi viết code (reference) |
| [12-Troubleshooting-Guide.md](LAYER-5-REFERENCE/12-Troubleshooting-Guide.md) | Xử lý lỗi thường gặp: ChromeDriver, TestNG, H2 console | Khi gặp lỗi |

---

## QUY TRÌNH SỬ DỤNG CHUẨN (CHO AI CHATBOT)

### 🎬 WORKFLOW TỔNG QUAN - TAF ONLY (KHÔNG TẠO APP)

```
⚠️ PREREQUISITES (USER RESPONSIBILITY):
   └─ Ứng dụng ĐÃ TRIỂN KHAI tại URL remote (staging/production)
   └─ URL có thể truy cập: https://staging.example.com/...
   └─ Form/tính năng đã hoạt động và có thể test thủ công
   └─ Có credentials (nếu cần authentication)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1️⃣ ĐỌC FILE 03-Problem-Analysis-Framework.md
   └─ Phân tích BÀI TOÁN TEST (KHÔNG phải bài toán viết app)
   └─ Phân loại loại test: UI testing, API testing, E2E
   └─ Xác định scope: Simple form / Complex form / CRUD / Search

2️⃣ ĐỌC FILE 04-Decision-Tree.md
   └─ Chọn TEMPLATE TAF phù hợp (A, B, C, D, hoặc E)
   └─ Template chỉ chứa src/test/ code, KHÔNG có src/main/

3️⃣ ĐỌC FILE 05-Feature-Selection-Matrix.md
   └─ Chọn Level 2 features (ExtentReports, Data-Driven, Retry)
   └─ Tránh over-engineering cho bài toán đơn giản

4️⃣ THEO PROCESS 4 BƯỚC (Files 06-09) - TAF FOCUS
   STEP 1 (Analysis): Thu thập THÔNG TIN ỨNG DỤNG ĐÃ CÓ
      └─ URL của form/page cần test
      └─ Field locators và validation rules
      └─ Expected behavior (success/error messages)
      └─ ⚠️ KHÔNG phân tích cách tạo app

   STEP 2 (Design): Thiết kế TEST CASES và TAF STRUCTURE
      └─ Page Object classes (BasePage, LoginPage, etc.)
      └─ Test classes với TestNG annotations
      └─ config.properties với REAL URL (not localhost)
      └─ ⚠️ KHÔNG thiết kế Entity.java, Controller.java

   STEP 3 (Implementation): Viết CODE TAF (src/test/ ONLY)
      └─ Milestones: M3 (TAF Core), M4 (Level 2 Features), M5 (Docs)
      └─ ⚠️ BỎ QUA M1-M2 (Application creation milestones)

   STEP 4 (Execution): Chạy tests và tạo reports
      └─ mvn clean test
      └─ ExtentReports cho stakeholders

5️⃣ ĐỌC TEMPLATE LIBRARY (File 10) khi cần
   └─ Mỗi template có phần PREREQUISITES yêu cầu app đã tồn tại
   └─ Code examples chỉ có Page Objects và Test classes

6️⃣ THAM KHẢO REFERENCE (Files 11-12) trong quá trình code
   └─ Best practices: Explicit Waits, POM, staging URL handling
   └─ Real-world considerations: Unique test data, cleanup, VPN
```

---

## HƯỚNG DẪN CHO AI CHATBOT - REAL-WORLD MODE

### Quy tắc vàng khi sử dụng hệ thống này:

#### ✅ BẮT BUỘC PHẢI LÀM:

1. **YÊU CẦU USER CUNG CẤP URL ỨNG DỤNG** - Không bắt đầu test nếu chưa có URL staging/production
2. **CHỈ XÂY DỰNG TAF (src/test/)** - Không tạo Entity.java, Controller.java, Service.java
3. **Luôn đọc file 03** trước khi bắt đầu bất kỳ dự án nào
4. **Tuân thủ Process 4 bước** - không được nhảy cóc
5. **Chờ confirmation từ user** trước khi chuyển bước
6. **Sử dụng checkpoints** ở mỗi milestone trong Bước 3
7. **Sử dụng REAL URLs** trong config.properties (https://staging..., KHÔNG localhost)
8. **Tham khảo Prerequisites section** trong mỗi template (File 10)

#### ❌ CẤM TUYỆT ĐỐI - REAL-WORLD MODE:

1. **KHÔNG TẠO ỨNG DỤNG** - Không viết code cho src/main/ layer (Entity, Controller, Service)
2. **KHÔNG GIẢI THÍCH M1-M2** - Bỏ qua hoàn toàn milestones tạo application
3. **KHÔNG DÙNG LOCALHOST URLs** - Luôn dùng remote staging URLs
4. **Không tự ý sinh code** trước khi user approve plan (Bước 2)
5. **Không assume** - Hỏi rõ URL, field locators, validation rules nếu thiếu
6. **Không dùng hard-coded test data** - Dùng unique identifiers (timestamp, UUID)
7. **Không skip Prerequisites verification** - Đảm bảo app tồn tại trước khi viết test
8. **Không implement tất cả 8 features** nếu bài toán đơn giản

#### 🎯 ĐIỂM KHÁC BIỆT REAL-WORLD MODE:

| Aspect | Practice Mode (Old) | Real-World Mode (Current) |
|--------|---------------------|---------------------------|
| **Scope** | M1-M5 (App + TAF) | M3-M5 (TAF ONLY) |
| **Duration** | 2-4 weeks (app+test) | 1-2 weeks (test only) |
| **URLs** | http://localhost:8080 | https://staging.company.com |
| **Prerequisites** | None (tạo app từ đầu) | App must exist remotely |
| **Test Data** | Hardcoded values ok | Unique data, cleanup required |
| **Code Output** | Entity, Controller, TAF | TAF ONLY (Page Objects, Tests) |

---

## QUICK REFERENCE: CHỌN FILE NÀO?

### Tình huống 1: "Tôi có một bài toán mới"
→ Đọc **File 03** (Problem Analysis Framework)

### Tình huống 2: "Tôi đã phân tích xong bài toán"
→ Đọc **File 04** (Decision Tree) để chọn template

### Tình huống 3: "Tôi cần biết phải implement features gì"
→ Đọc **File 05** (Feature Selection Matrix)

### Tình huống 4: "Tôi sẵn sàng bắt đầu làm"
→ Đọc **Files 06-09** theo thứ tự (Process 4 bước)

### Tình huống 5: "Bài toán là Simple Form, làm gì tiếp?"
→ Đọc **File 10** phần Template A

### Tình huống 6: "Cách đặt tên class Page Object?"
→ Đọc **File 11** (Best Practices)

### Tình huống 7: "ChromeDriver không tìm thấy"
→ Đọc **File 12** (Troubleshooting)

---

## VERSION CONTROL

- **Version**: 3.0.0 (Real-World Mode - TAF Only)
- **Date**: 2025-01-17
- **Changes** (v3.0.0):
  - **BREAKING CHANGE**: Chuyển từ Practice Mode → Real-World Mode
  - Loại bỏ toàn bộ M1-M2 (Application creation milestones)
  - Thêm Prerequisites sections cho tất cả templates
  - Thêm Real-World Considerations (staging URLs, unique test data, cleanup)
  - Cập nhật timelines: 2-4 tuần → 1-2 tuần (chỉ TAF)
  - Cập nhật CLAUDE.md và tất cả process files (06-09)
  - Cập nhật tất cả 5 templates trong Template Library (File 10)

- **Version**: 2.0.0 (Refactored - Modular Architecture)
- **Date**: 2025-01-10
- **Changes** (v2.0.0):
  - Tách file template-base.md thành 12 files modular
  - Thêm Problem Analysis Framework
  - Thêm Decision Tree và Feature Selection Matrix
  - Chuẩn hóa Process 4 bước với checkpoints
  - Thêm Template Library với 5 templates con

---

## FEEDBACK & IMPROVEMENT

Nếu phát hiện:
- Template thiếu coverage cho một loại bài toán nào đó
- Quy trình chưa rõ ràng ở bước nào
- Cần thêm best practice hoặc troubleshooting item

→ Document lại và đề xuất cải tiến cho version tiếp theo

---

**🚀 SẴN SÀNG BẮT ĐẦU? → Đọc File 03-Problem-Analysis-Framework.md**

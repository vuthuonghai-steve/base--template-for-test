# 🔍 PROBLEM ANALYSIS FRAMEWORK - PHÂN TÍCH BÀI TOÁN

## MỤC ĐÍCH FILE NÀY

File này là **điểm khởi đầu bắt buộc** cho mọi dự án test automation mới. Nó cung cấp framework có cấu trúc để phân tích và hiểu rõ bài toán trước khi viết bất kỳ dòng code nào.

---

## ⚠️ QUY TẮC SỬ DỤNG CHO AI CHATBOT

### BẮT BUỘC:

1. **Đọc file này TRƯỚC TIÊN** khi nhận bài toán mới từ user
2. **Điền đầy đủ checklist** dưới đây bằng cách hỏi user
3. **Không được assume** - hỏi rõ nếu thiếu thông tin
4. **Tạo Problem Analysis Document** hoàn chỉnh
5. **Chờ user confirm** document trước khi sang bước tiếp theo

### CẤM TUYỆT ĐỐI:

❌ Không bắt đầu code trước khi phân tích xong  
❌ Không dùng hard-coded examples (Student, Registration)  
❌ Không skip checklist nếu user cung cấp thông tin không đủ  
❌ Không tự ý giả định requirements  

---

## PROBLEM ANALYSIS CHECKLIST

### PHẦN 1: PROBLEM STATEMENT

**📝 Câu hỏi cho user:**
```
Bạn cần test loại form/trang nào?
- Mô tả ngắn gọn về form/trang này
- Mục đích chính của form này là gì?
- Link đến đề bài (nếu có)
```

**Output cần có:**
```markdown
### Problem Statement
- **Problem Type**: [Form Testing / CRUD Operations / Search & Filter / Other]
- **Form/Page Name**: {{FORM_NAME}}
- **Primary Purpose**: [Mô tả ngắn gọn]
- **Reference**: [Link hoặc description]
```

---

### PHẦN 2: FORM/PAGE CLASSIFICATION

**📝 Câu hỏi cho user:**
```
1. Form/trang này có bao nhiêu steps/pages?
   - Single-page form (tất cả fields trên 1 trang)
   - Multi-step form (wizard: bước 1 → bước 2 → bước 3)
   - Multiple related pages (CRUD: list → create → edit → delete)

2. Tổng số fields/inputs trên form?
   - Ít hơn 5 fields
   - 5-10 fields
   - 10-20 fields
   - Hơn 20 fields

3. Form submission behavior?
   - Submit và refresh page
   - Submit và redirect sang page khác
   - Submit qua AJAX (không reload page)
   - Trigger workflow hoặc background job
```

**Decision Matrix:**

| Đặc điểm | Classification | Template gợi ý |
|:---------|:--------------|:---------------|
| Single-page, <5 fields, simple validation | **Simple Form** | Template A |
| Single-page, 5-10 fields, moderate validation | **Medium Form** | Template A (với more features) |
| Single-page, >10 fields, complex validation | **Complex Form** | Template B |
| Multi-step, any number of fields | **Multi-Step Form** | Template C |
| List + Create/Edit/Delete pages | **CRUD Operations** | Template D |
| Search box + Results table + Filters | **Search & Filter** | Template E |

**Output cần có:**
```markdown
### Form Classification
- **Type**: [Simple/Medium/Complex/Multi-Step/CRUD/Search]
- **Number of Steps**: [1/2/3/...]
- **Total Fields**: [X fields]
- **Submission Behavior**: [Refresh/Redirect/AJAX/...]
- **Recommended Template**: [Template A/B/C/D/E]
```

---

### PHẦN 3: FIELDS INVENTORY

**📝 Câu hỏi cho user:**
```
Liệt kê tất cả fields trong form với thông tin:
1. Field name (label hiển thị)
2. Field type (text, number, email, date, dropdown, checkbox, radio, file upload)
3. Required hay optional
4. Validation rules (nếu có)

Ví dụ:
- "Mã sinh viên" | Text | Required | Format: SVxxxxxx (SV + 6 chữ số)
- "Email" | Email | Required | Format: email hợp lệ
- "Ngày sinh" | Date | Required | Phải trên 18 tuổi
```

**Output cần có (dạng bảng):**

| # | Field Name | Field Type | Required? | Validation Rules |
|:--|:-----------|:-----------|:---------:|:-----------------|
| 1 | {{Field 1}} | {{Type}} | ✅/❌ | {{Rules}} |
| 2 | {{Field 2}} | {{Type}} | ✅/❌ | {{Rules}} |
| ... | ... | ... | ... | ... |

**Phân loại validation:**
- **Required Field**: Field không được để trống
- **Format Validation**: Email, phone, date, pattern matching
- **Range Validation**: Min/max length, min/max value, date range
- **Business Logic**: Tuổi >18, mã SV unique, số dư đủ
- **Cross-Field**: Confirm password = password, end date > start date

---

### PHẦN 4: USER FLOWS

**📝 Câu hỏi cho user:**
```
1. HAPPY PATH (Successful Scenario):
   - User làm gì?
   - System phản hồi như thế nào?
   - Expected outcome là gì?

2. ERROR PATHS (Failure Scenarios):
   - User submit với missing required fields → ?
   - User submit với invalid format → ?
   - User submit với business logic violation → ?
   - Mỗi error case, system hiển thị error message ở đâu và nội dung gì?
```

**Output cần có:**

#### Happy Path:
```
1. User mở form tại [URL]
2. User điền đầy đủ [X] fields với valid data
3. User click Submit button
4. System validates data
5. System saves data to database
6. System hiển thị success message: "[Message text]"
7. System redirect đến [URL] hoặc refresh form
```

#### Error Paths:
```
ERROR PATH 1: Missing Required Field
- Trigger: User bỏ trống field [Field name]
- Expected: Error message "[Message text]" hiển thị tại [location]
- Form state: [Giữ nguyên data/Clear data]

ERROR PATH 2: Invalid Format
- Trigger: User nhập email không hợp lệ
- Expected: Error message "[Message text]" hiển thị tại [location]

ERROR PATH 3: Business Logic Violation
- Trigger: [Mô tả specific violation]
- Expected: Error message "[Message text]" hiển thị tại [location]
```

---

### PHẦN 5: DATA FLOW ANALYSIS

**📝 Câu hỏi cho user:**
```
Sau khi user submit form thành công:
1. Data được lưu ở đâu?
   - Database (H2, MySQL, PostgreSQL)
   - External API
   - File system
   - Session/Cookie

2. Có backend validation không?
   - Frontend only
   - Frontend + Backend double validation
   - Backend only

3. Có cần authentication/authorization không?
   - Public form (ai cũng truy cập được)
   - Require login trước
   - Role-based access control
```

**Output cần có:**
```markdown
### Data Flow
- **Storage**: [Database/API/File/Session]
- **Validation Layer**: [Frontend only/Frontend+Backend/Backend only]
- **Authentication**: [None/Login required/Role-based]
- **API Endpoints** (nếu có):
  - POST /{{endpoint}} - Submit form
  - GET /{{endpoint}} - Retrieve data
```

---

### PHẦN 6: DEPENDENCIES & CONSTRAINTS

**📝 Câu hỏi cho user:**
```
1. Form có phụ thuộc vào external services không?
   - Third-party API (payment gateway, email service)
   - Other internal services
   - Dynamic data sources (dropdown load từ database)

2. Có file upload không?
   - Loại file: image, PDF, Excel, etc.
   - Size limit
   - Format restrictions

3. Browser requirements?
   - Chrome only
   - Chrome + Firefox + Edge
   - Mobile responsive cần test không?

4. Performance expectations?
   - Form phải load trong bao nhiêu giây?
   - Submit phải complete trong bao nhiêu giây?
```

**Output cần có:**
```markdown
### Dependencies & Constraints
- **External Services**: [List services]
- **File Upload**: [Yes/No] - [Details nếu có]
- **Browser Support**: [Chrome/Firefox/Edge/Safari]
- **Mobile Responsive**: [Yes/No]
- **Performance SLA**:
  - Page load: < [X] seconds
  - Form submit: < [Y] seconds
```

---

### PHẦN 7: SUCCESS CRITERIA

**📝 Câu hỏi cho user:**
```
Khi nào dự án này được coi là thành công?
1. Test coverage mong muốn?
   - Happy path only
   - Happy path + negative cases
   - Full coverage (happy + negative + edge cases)

2. Automation level?
   - Chỉ smoke tests (critical flows)
   - Regression suite (major scenarios)
   - Full automation (tất cả scenarios)

3. Reporting requirements?
   - Console log đơn giản
   - HTML report (ExtentReports)
   - Integration với CI/CD

4. Timeline?
   - Cần hoàn thành trong bao lâu?
```

**Output cần có:**
```markdown
### Success Criteria
- **Test Coverage**: [Happy only/Happy+Negative/Full coverage]
- **Automation Level**: [Smoke/Regression/Full]
- **Reporting**: [Console/HTML/CI-CD integrated]
- **Timeline**: [X weeks/Y days]
- **Definition of Done**:
  - [ ] AUT running successfully
  - [ ] All test cases documented
  - [ ] Automation code complete
  - [ ] Tests passing with >XX% success rate
  - [ ] Documentation complete
```

---

## PROBLEM ANALYSIS DOCUMENT TEMPLATE

Sau khi hoàn thành checklist, tạo document theo template này:

```markdown
# PROBLEM ANALYSIS DOCUMENT
**Project**: {{PROJECT_NAME}}
**Date**: {{DATE}}
**Analyst**: {{NAME/AI}}

---

## 1. PROBLEM STATEMENT
- **Problem Type**: 
- **Form/Page Name**: 
- **Primary Purpose**: 
- **Reference**: 

---

## 2. FORM CLASSIFICATION
- **Type**: 
- **Number of Steps**: 
- **Total Fields**: 
- **Submission Behavior**: 
- **Recommended Template**: 

---

## 3. FIELDS INVENTORY

| # | Field Name | Field Type | Required? | Validation Rules |
|:--|:-----------|:-----------|:---------:|:-----------------|
|   |            |            |           |                  |

---

## 4. USER FLOWS

### Happy Path:
[Steps 1-7]

### Error Paths:
[ERROR PATH 1, 2, 3...]

---

## 5. DATA FLOW
- **Storage**: 
- **Validation Layer**: 
- **Authentication**: 
- **API Endpoints**: 

---

## 6. DEPENDENCIES & CONSTRAINTS
- **External Services**: 
- **File Upload**: 
- **Browser Support**: 
- **Mobile Responsive**: 
- **Performance SLA**: 

---

## 7. SUCCESS CRITERIA
- **Test Coverage**: 
- **Automation Level**: 
- **Reporting**: 
- **Timeline**: 
- **Definition of Done**: 

---

## 8. NOTES & ASSUMPTIONS
[Any additional notes, assumptions, or open questions]

---

**Status**: ⏳ Pending User Confirmation
```

---

## AI CHATBOT WORKFLOW

### Khi nhận bài toán mới:

**Step 1**: Chào user và giải thích process
```
Chào bạn! Tôi sẽ giúp bạn xây dựng test automation framework.
Trước tiên, cho phép tôi phân tích bài toán của bạn qua một số câu hỏi.
Việc này giúp đảm bảo tôi hiểu đúng yêu cầu và design solution phù hợp.
```

**Step 2**: Đặt câu hỏi theo checklist
- Không đặt tất cả câu hỏi cùng lúc
- Đặt từng group (PHẦN 1, 2, 3, ...)
- Chờ user trả lời xong một phần mới hỏi phần tiếp

**Step 3**: Tạo Problem Analysis Document
- Điền đầy đủ thông tin từ câu trả lời
- Format rõ ràng, dễ đọc
- Highlight các điểm quan trọng

**Step 4**: Xin confirmation
```
Tôi đã tạo Problem Analysis Document ở trên.
Vui lòng xem lại và cho tôi biết:
1. Có thông tin nào thiếu sót không?
2. Có thông tin nào cần điều chỉnh không?
3. Bạn có câu hỏi gì về phân tích này không?

Chỉ khi bạn xác nhận document này chính xác, tôi mới tiến sang bước
thiết kế solution (Bước 2).
```

**Step 5**: Chỉ tiến sang File 04 (Decision Tree) sau khi có confirmation

---

## VALIDATION CHECKLIST (TRƯỚC KHI CHUYỂN BƯỚC)

Trước khi move sang File 04-Decision-Tree.md, verify:

- [ ] Problem statement đã rõ ràng
- [ ] Form classification đã xác định được
- [ ] Ít nhất 80% fields đã được liệt kê với validation rules
- [ ] Happy path được mô tả chi tiết
- [ ] Ít nhất 2-3 error paths được identify
- [ ] Data flow đã hiểu rõ
- [ ] Dependencies và constraints đã note
- [ ] Success criteria đã define
- [ ] **User đã confirm document**

---

## EXAMPLES (FOR REFERENCE ONLY)

### Example 1: Simple Contact Form

**Classification**: Simple Form  
**Fields**: 4 (Name, Email, Subject, Message)  
**Template**: Template A  

### Example 2: Student Registration

**Classification**: Complex Form  
**Fields**: 12 (Student ID, Full Name, DOB, Email, Phone, Address, etc.)  
**Template**: Template B  

### Example 3: E-commerce Checkout

**Classification**: Multi-Step Form  
**Steps**: 3 (Shipping Info → Payment → Review)  
**Template**: Template C  

### Example 4: User Management

**Classification**: CRUD Operations  
**Pages**: List, Create, Edit, Delete  
**Template**: Template D  

---

**⏭️ SAU KHI HOÀN THÀNH: [04-Decision-Tree.md](04-Decision-Tree.md)**

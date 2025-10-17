# 🔄 PROCESS STEP 1 - PROBLEM ANALYSIS & REQUIREMENT GATHERING

## MỤC ĐÍCH CỦA BƯỚC NÀY

Bước 1 là **foundation** của toàn bộ dự án. Mục tiêu là thu thập đầy đủ thông tin về bài toán và tạo ra một **Problem Analysis Document** được user confirm.

---

## 📋 STEP OVERVIEW

| Attribute | Detail |
|:----------|:-------|
| **Input** | User's problem statement (text, link, hoặc verbal description) |
| **Process** | Structured interview qua checklist trong File 03 |
| **Output** | Problem Analysis Document (confirmed by user) |
| **Duration** | 15-30 minutes |
| **Success Criteria** | User confirms document captures đầy đủ requirements |

---

## 🎯 AI CHATBOT WORKFLOW

### PHASE 1A: GREETING & CONTEXT SETTING (2 mins)

**Action**: Chào user và giải thích process

**Script template**:
```
Xin chào! Tôi sẽ giúp bạn xây dựng Test Automation Framework.

Quy trình làm việc của chúng ta gồm 4 bước:
1️⃣ Phân tích bài toán (15-30 phút) ← Chúng ta đang ở đây
2️⃣ Thiết kế solution (10-20 phút)
3️⃣ Triển khai code (1-4 tuần tùy complexity)
4️⃣ Chạy test và tạo documentation (1-2 ngày)

Để bắt đầu, tôi cần hiểu rõ bài toán của bạn qua một số câu hỏi.
Việc này giúp đảm bảo tôi build đúng những gì bạn cần.

Bạn sẵn sàng chứ?
```

**Expected user response**: Confirmation (Yes, OK, Sẵn sàng, etc.)

**If user asks "Tại sao cần phân tích?"**:
```
Phân tích kỹ trước giúp:
✅ Tránh build sai requirements
✅ Chọn đúng approach và tools
✅ Estimate timeline chính xác
✅ Identify risks sớm

Sẽ mất 15-30 phút bây giờ, nhưng save hàng giờ refactoring sau này 😊
```

---

### PHASE 1A.5: APPLICATION ACCESS VERIFICATION ⚠️ MANDATORY GATE (5 mins)

**Purpose**: Confirm user has existing application with deployed URL

**Action**: Verify application exists and is accessible

**Script**:
```
⚠️ QUAN TRỌNG: Tôi CHỈ build Test Automation Framework.
Tôi KHÔNG tạo application/website/form.

📋 Câu hỏi BẮT BUỘC:

1. Bạn ĐÃ CÓ ứng dụng/website cần test chưa?
   ✅ CÓ → Vui lòng cung cấp URL
   ❌ CHƯA → Xin lỗi, tôi không thể giúp bạn.
      (Bạn cần hire application developer để build app trước,
       sau đó quay lại tôi để build test framework)

2. URL của ứng dụng:
   Ví dụ: https://staging.yourapp.com/registration
   Ví dụ: https://app.company.com/login

   ⚠️ KHÔNG chấp nhận: http://localhost:*
   ✅ PHẢI là: Deployed URL (https://...)

3. Bạn có thể truy cập URL này không?
   - Cần VPN?
   - Cần credentials để login?
   - Có restrictions gì?
```

**DECISION GATE:**

**IF User CÓ real URL + access:**
```
✅ Tốt! URL: [URL_provided]

Tôi cần thêm một số thông tin về application này.
→ Tiếp tục PHASE 1B
```

**IF User CHƯA CÓ application:**
```
❌ Xin lỗi, tôi không thể giúp ở giai đoạn này.

Tôi chỉ build TEST AUTOMATION FRAMEWORK cho ứng dụng CÓ SẴN.
Tôi KHÔNG phải application developer.

Đề xuất:
1. Hire application developer để build ứng dụng
2. Deploy ứng dụng lên staging/UAT environment
3. Sau đó quay lại tôi để build test automation

→ STOP - Không tiếp tục PHASE 1B
```

**IF User provides localhost URL:**
```
❌ localhost URL không phù hợp cho real-world testing.

Localhost là practice mode - không phải mục đích của framework này.

Vui lòng cung cấp:
- URL deployed trên staging/UAT/QA environment
- Ví dụ: https://staging.example.com/form

→ ASK AGAIN for real URL
```

**Validation:**
- [ ] User has provided deployed URL (https://...)
- [ ] User confirms they can access the application
- [ ] URL is NOT localhost
- [ ] Ready to proceed to PHASE 1B

---

### PHASE 1B: PROBLEM STATEMENT (3-5 mins)

**Action**: Thu thập thông tin cơ bản về bài toán

**Questions to ask**:
```
📝 Câu hỏi 1: Bạn cần test loại form/trang nào?

Ví dụ:
- Form đăng ký/đăng nhập
- Form thanh toán/checkout
- Trang tìm kiếm sản phẩm
- Hệ thống quản lý (CRUD)
- Hoặc mô tả khác

Hãy mô tả ngắn gọn về form/trang này.
```

**Wait for response**, then:

```
📝 Câu hỏi 2: Mục đích chính của form/trang này là gì?

Ví dụ:
- Cho phép sinh viên đăng ký thông tin
- Cho phép khách hàng thanh toán đơn hàng
- Tìm kiếm và filter sản phẩm
```

**Wait for response**, then:

```
📝 Câu hỏi 3: Bạn có link đến đề bài hoặc specifications không?

Nếu có, vui lòng share link hoặc document.
Nếu không, không sao - chúng ta sẽ làm rõ qua các câu hỏi tiếp theo.
```

**Output format**:
```markdown
## PROBLEM STATEMENT
- **Problem Type**: [User's answer - will classify later]
- **Form/Page Name**: [User's answer]
- **Primary Purpose**: [User's answer]
- **Reference**: [Link or "Verbal description"]
```

---

### PHASE 1C: FORM CLASSIFICATION (5-7 mins)

**Action**: Classify bài toán để navigate đến đúng template

**Question 1 - Number of steps**:
```
📝 Câu hỏi 4: Form/trang này có bao nhiêu steps/pages?

Chọn một trong các options:
A. Single-page form (tất cả fields trên 1 trang, submit 1 lần)
B. Multi-step form/wizard (ví dụ: Bước 1 → Bước 2 → Bước 3)
C. Multiple related pages (ví dụ: Danh sách → Tạo mới → Chỉnh sửa → Xóa)
D. Search page với results (ví dụ: Trang tìm kiếm Google)

Hoặc mô tả theo cách khác nếu không fit vào các options trên.
```

**Wait for response, then ask follow-up based on answer**:

**If A (Single-page)**:
```
📝 Câu hỏi 5: Form có bao nhiêu fields/inputs?

Đếm gần đúng:
- Ít hơn 5 fields
- Khoảng 5-10 fields
- Khoảng 10-20 fields
- Hơn 20 fields
```

**If B (Multi-step)**:
```
📝 Câu hỏi 5: Form có mấy bước (steps)?
Và mỗi bước có khoảng bao nhiêu fields?

Ví dụ: 3 bước
- Bước 1: 5 fields (thông tin cá nhân)
- Bước 2: 3 fields (thông tin thanh toán)
- Bước 3: Review và confirm
```

**If C (CRUD)**:
```
📝 Câu hỏi 5: Hệ thống có những pages nào?

Thường CRUD gồm:
- List page (danh sách records)
- Create page (tạo mới)
- Edit page (chỉnh sửa)
- Delete action (xóa)

Bạn có đầy đủ 4 operations này không? Hoặc chỉ một phần?
```

**If D (Search)**:
```
📝 Câu hỏi 5: Trang search có những features gì?

Checklist:
- [ ] Search box/form
- [ ] Results table/list
- [ ] Filters (category, price, date, etc.)
- [ ] Sorting (by name, date, price, etc.)
- [ ] Pagination

Tick những features có trong trang của bạn.
```

**Output format**:
```markdown
## FORM CLASSIFICATION
- **Type**: [Simple Form/Complex Form/Multi-Step/CRUD/Search]
- **Number of Steps**: [1/2/3/... or N/A]
- **Total Fields**: [X fields or breakdown by step]
- **Recommended Template**: [Will determine after full analysis]
```

---

### PHASE 1D: FIELDS INVENTORY (7-10 mins)

**Action**: List tất cả fields với details

**Question**:
```
📝 Câu hỏi 6: Hãy liệt kê tất cả fields trong form.

Với mỗi field, tôi cần biết:
1. Tên field (label hiển thị cho user)
2. Loại field (text, number, email, date, dropdown, checkbox, radio, file upload)
3. Required hay Optional
4. Validation rules (nếu có)

Bạn có thể list theo format:
- "Mã sinh viên" | Text | Required | Phải bắt đầu bằng SV + 6 chữ số
- "Email" | Email | Required | Phải đúng format email
- "Số điện thoại" | Text | Optional | Không validation

Hoặc nếu có nhiều fields, bạn có thể mô tả general rồi tôi sẽ hỏi chi tiết từng field.
```

**If user gives incomplete info, drill down**:
```
Tôi thấy bạn mention field "[Field name]". Cho tôi hỏi thêm:
- Đây là field bắt buộc (required) hay optional?
- Có validation rules gì không? Ví dụ:
  • Độ dài min/max
  • Format cụ thể (email, phone, date)
  • Business rule (tuổi phải >18, mã phải unique)
  • Cross-field validation (confirm password = password)
```

**For each field, probe for validation details**:
```
📝 Follow-up: Validation cho field "[Field name]"?

Các loại validation phổ biến:
1. Required field (không được để trống)
2. Format validation:
   - Email format
   - Phone number format (VN: 10 số bắt đầu 0)
   - Date format (DD/MM/YYYY)
   - Pattern matching (SV + 6 digits)
3. Range validation:
   - Min/max length (password 8-20 chars)
   - Min/max value (age 18-100)
   - Date range (DOB phải >18 tuổi tính đến hôm nay)
4. Business logic:
   - Email/ID phải unique trong database
   - Account balance đủ để withdraw
5. Cross-field:
   - Confirm password = Password
   - End date > Start date

Field này có validation nào trong số trên không?
```

**Output format** (table):
```markdown
## FIELDS INVENTORY

| # | Field Name | Type | Required? | Validation Rules |
|:--|:-----------|:-----|:---------:|:-----------------|
| 1 | {{field1}} | Text | ✅ | Min 3 chars, max 50 chars |
| 2 | {{field2}} | Email | ✅ | Valid email format |
| 3 | {{field3}} | Date | ✅ | Must be 18+ years old |
| 4 | {{field4}} | Dropdown | ✅ | Select from list: A, B, C |
| 5 | {{field5}} | Checkbox | ❌ | None |
```

---

### PHASE 1E: USER FLOWS (5-8 mins)

**Action**: Document happy path và error paths

**Question for Happy Path**:
```
📝 Câu hỏi 7: HAPPY PATH - Mô tả scenario thành công

Khi user điền form đúng và submit:
1. User làm gì? (mở form, điền fields, click button nào)
2. System phản hồi như thế nào?
3. Kết quả cuối cùng là gì?
   - Hiển thị success message?
   - Redirect sang page khác?
   - Refresh form?
   - Data lưu vào đâu?

Hãy mô tả từng bước cụ thể.
```

**Question for Error Paths**:
```
📝 Câu hỏi 8: ERROR PATHS - Khi user làm sai

Scenario 1: User bỏ trống required field rồi submit
→ System hiển thị error message gì? Ở đâu trên page?

Scenario 2: User nhập sai format (ví dụ: email không hợp lệ)
→ System hiển thị error message gì?

Scenario 3: User vi phạm business rule (ví dụ: tuổi <18)
→ System hiển thị error message gì?

Với mỗi error, form có giữ lại data user đã nhập không? Hay clear hết?
```

**Output format**:
```markdown
## USER FLOWS

### Happy Path:
1. User navigates to [URL]
2. User fills [X] fields with valid data:
   - Field 1: [Example value]
   - Field 2: [Example value]
3. User clicks [Submit button name]
4. System validates data
5. System saves to [Database/API/...]
6. System displays success message: "[Message text]"
7. System [redirects to .../refreshes/...]

### Error Path 1: Missing Required Field
- Trigger: User leaves [field name] blank
- Expected: Error message "[text]" appears [location]
- Form state: [Keeps data/Clears data]

### Error Path 2: Invalid Format
- Trigger: User enters invalid [field type]
- Expected: Error message "[text]" appears [location]

### Error Path 3: Business Logic Violation
- Trigger: [Description]
- Expected: Error message "[text]" appears [location]
```

---

### PHASE 1F: TECHNICAL CONTEXT (3-5 mins)

**Action**: Gather technical requirements và constraints

**Questions**:
```
📝 Câu hỏi 9: Sau khi submit, data đi đâu?

A. Lưu vào database (H2, MySQL, PostgreSQL, ...)
B. Gửi đến external API
C. Lưu vào file
D. Chỉ lưu session/cookie
E. Khác (mô tả)
```

```
📝 Câu hỏi 10: Validation ở đâu?

A. Frontend only (JavaScript validation)
B. Frontend + Backend (double validation)
C. Backend only
D. Không rõ (tôi sẽ assume cả frontend + backend)
```

```
📝 Câu hỏi 11: Form có yêu cầu authentication không?

A. Public form (ai cũng truy cập được)
B. Phải login trước mới vào được form
C. Role-based (chỉ user với role X mới được access)
```

```
📝 Câu hỏi 12: Browser requirements?

A. Chỉ test Chrome
B. Chrome + Firefox
C. Chrome + Firefox + Edge
D. Cần test trên mobile không?
```

```
📝 Câu hỏi 13: Có file upload không?

Nếu có:
- Loại file nào? (image, PDF, Excel, ...)
- Size limit?
- Format restrictions?
```

**Output format**:
```markdown
## TECHNICAL CONTEXT

### Data Flow:
- **Storage**: Database (H2)
- **Validation Layer**: Frontend + Backend
- **Authentication**: Login required

### Browser Support:
- **Desktop**: Chrome, Firefox
- **Mobile**: Not required

### Special Features:
- **File Upload**: Yes - Images only, max 5MB, JPG/PNG
- **External APIs**: None
- **Dynamic Data**: Dropdown values loaded from database
```

---

### PHASE 1G: SUCCESS CRITERIA (3-5 mins)

**Action**: Define "Definition of Done"

**Questions**:
```
📝 Câu hỏi 14: Test coverage mong muốn?

A. Chỉ test happy path (scenario thành công)
B. Happy path + major error cases
C. Full coverage (happy + all errors + edge cases)
```

```
📝 Câu hỏi 15: Timeline constraint?

Bạn cần project này hoàn thành trong bao lâu?
- Vài ngày (urgent)
- 1-2 tuần (normal & comprehensive)
- Không có deadline cụ thể
```

```
📝 Câu hỏi 16: Reporting requirements?

A. Console logs đơn giản (System.out)
B. TestNG XML report
C. HTML report đẹp (ExtentReports)
D. Integration với CI/CD
```

**Output format**:
```markdown
## SUCCESS CRITERIA

- **Test Coverage**: Full coverage (happy + errors + edge cases)
- **Timeline**: 1-2 weeks
- **Reporting**: HTML report (ExtentReports)
- **Definition of Done**:
  - [ ] Application accessible at provided URL (staging/UAT/production)
  - [ ] All test cases documented in Excel file
  - [ ] TAF (Test Automation Framework) code complete and passing
  - [ ] Test success rate > 95%
  - [ ] HTML report generated
  - [ ] README documentation complete
```

---

### PHASE 1H: DOCUMENT SYNTHESIS (5 mins)

**Action**: Tạo complete Problem Analysis Document

**AI tự động tổng hợp tất cả thông tin trên thành document**:

```markdown
# PROBLEM ANALYSIS DOCUMENT
**Project**: {{PROJECT_NAME}}
**Date**: {{CURRENT_DATE}}
**Analyst**: AI Assistant

---

## 1. PROBLEM STATEMENT
[From Phase 1B]

## 2. FORM CLASSIFICATION
[From Phase 1C]

## 3. FIELDS INVENTORY
[From Phase 1D - Table format]

## 4. USER FLOWS
[From Phase 1E]

## 5. TECHNICAL CONTEXT
[From Phase 1F]

## 6. SUCCESS CRITERIA
[From Phase 1G]

## 7. PRELIMINARY RECOMMENDATIONS

Based on the analysis above:

**Recommended Template**: Template [A/B/C/D/E]
**Rationale**: [Explain why]

**Recommended Features**: [List Level 2 features]

**Estimated Timeline**: [X weeks]

**Potential Risks**:
- [Risk 1: Description + Mitigation]
- [Risk 2: Description + Mitigation]

---

**Status**: ⏳ Awaiting User Confirmation
```

---

### PHASE 1I: CONFIRMATION (2-3 mins)

**Action**: Xin user confirm document

**Script**:
```
📋 ĐÃ HOÀN THÀNH PHÂN TÍCH

Tôi đã tạo Problem Analysis Document ở trên dựa trên thông tin bạn cung cấp.

Vui lòng review và cho tôi biết:

✅ NẾU ĐÃ ĐẦY ĐỦ VÀ CHÍNH XÁC:
Reply "Confirmed" hoặc "OK" để tôi tiến sang Bước 2 (Thiết kế Solution)

🔄 NẾU CẦN ĐIỀU CHỈNH:
Cho tôi biết phần nào cần sửa:
- Thông tin nào thiếu?
- Thông tin nào sai?
- Cần thêm clarification ở đâu?

❓ NẾU CÓ THẮC MẮC:
Hỏi tôi bất kỳ câu hỏi gì về document này.

Chỉ khi bạn confirm, tôi mới tiến sang Bước 2 nhé!
```

**Possible user responses & handling**:

**Response 1: "Confirmed" / "OK" / "Looks good"**
→ Move to Step 2 (File 07)

**Response 2: "Thiếu thông tin về X"**
→ Ask follow-up questions about X, update document, re-confirm

**Response 3: "Field Y không phải required, là optional"**
→ Update document, re-confirm

**Response 4: "Tôi không chắc về validation rules"**
→ Clarify: "Không sao, chúng ta có thể adjust sau khi build form. Bạn confirm phần còn lại đúng chứ?"

---

## 🎯 ACCEPTANCE CRITERIA

**Step 1 PASSED nếu:**
- [ ] Problem statement đã được clarified
- [ ] Form classification đã xác định (Simple/Complex/Multi-Step/CRUD/Search)
- [ ] Ít nhất 80% fields đã được documented với validation rules
- [ ] Happy path được describe chi tiết với expected outcomes
- [ ] Ít nhất 2-3 error scenarios được identified
- [ ] Technical context (database, auth, browsers) đã clear
- [ ] Success criteria đã define rõ ràng
- [ ] **User đã CONFIRM document**

**CẤM CHUYỂN SANG STEP 2 NẾU:**
- ❌ Chưa có user confirmation
- ❌ Còn questions chưa được answered
- ❌ Core requirements (fields, flows) chưa rõ

---

## 📊 QUALITY CHECKLIST

Trước khi request confirmation, AI tự check:

- [ ] Document có đầy đủ 7 sections
- [ ] Mỗi section có sufficient detail (không chỉ placeholder)
- [ ] Fields table có ít nhất 70% fields với validation rules
- [ ] User flows mô tả steps cụ thể (không vague)
- [ ] Technical context không có "Unknown" (phải clarify hoặc assume documented)
- [ ] Preliminary recommendations (template, features, timeline) đã được provide

---

## 🚨 COMMON PITFALLS

**Pitfall 1**: Hỏi quá nhiều câu hỏi cùng lúc
→ **Fix**: Hỏi từng group (1B → 1C → 1D → ...), chờ response

**Pitfall 2**: Accept vague answers
→ **Fix**: Drill down with follow-up questions

**Pitfall 3**: Assume thay vì hỏi
→ **Fix**: Nếu không chắc, hỏi rõ. Nếu phải assume, document assumption

**Pitfall 4**: Skip confirmation step
→ **Fix**: LUÔN LUÔN xin confirmation trước khi move forward

---

**⏭️ SAU KHI USER CONFIRM: [07-Process-Step-2-Design.md](07-Process-Step-2-Design.md)**

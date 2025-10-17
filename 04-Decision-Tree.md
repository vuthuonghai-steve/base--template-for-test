# 🔍 DECISION TREE - CÂY QUYẾT ĐỊNH CHỌN TEMPLATE

## MỤC ĐÍCH FILE NÀY

Sau khi hoàn thành **Problem Analysis** (File 03), sử dụng file này để xác định **template nào phù hợp nhất** cho bài toán của bạn.

---

## 🎯 CÁCH SỬ DỤNG

**Input**: Problem Analysis Document đã được user confirm  
**Output**: Một trong 5 templates (A, B, C, D, E) với lý do cụ thể  
**Time**: 2-3 phút để đi qua decision tree  

---

## DECISION TREE DIAGRAM

```
START: Đã có Problem Analysis Document?
│
├─► NO ──► Quay lại File 03-Problem-Analysis-Framework.md
│
└─► YES
    │
    ▼
Q1: Loại bài toán là gì?
    │
    ├─► FORM TESTING ────────────────────┐
    │                                    │
    ├─► CRUD OPERATIONS ─────► Template D
    │
    ├─► SEARCH & FILTER ─────► Template E
    │
    └─► OTHER ───────────────► Custom (consult File 01)

    ┌────────────────────────────────────┘
    │ (FORM TESTING path)
    ▼
Q2: Form có bao nhiêu steps?
    │
    ├─► SINGLE-STEP FORM ────────────────┐
    │                                    │
    └─► MULTI-STEP FORM ─────► Template C

    ┌────────────────────────────────────┘
    │ (SINGLE-STEP FORM path)
    ▼
Q3: Form có bao nhiêu fields?
    │
    ├─► < 5 FIELDS ──────────────────────┐
    │                                    │
    ├─► 5-10 FIELDS ─────────────────────┤
    │                                    │
    └─► > 10 FIELDS ─────────────────────┤

    ┌────────────────────────────────────┘
    │ (Tiếp tục phân tích)
    ▼
Q4: Validation rules phức tạp không?
    │
    ├─► SIMPLE (chỉ required + format) ──┐
    │                                    │
    └─► COMPLEX (business logic, cross-field) ┐
                                         │
    ┌────────────────────────────────────┘
    │
    ▼
FINAL DECISION:
│
├─► < 5 fields + Simple validation ────► Template A (Simple)
│
├─► 5-10 fields + Simple validation ───► Template A (với more features)
│
├─► 5-10 fields + Complex validation ──► Template B (Complex)
│
└─► > 10 fields (any validation) ──────► Template B (Complex)
```

---

## DETAILED DECISION RULES

### Rule 1: CRUD Operations

**Trigger conditions:**
- Bài toán có các pages: List, Create, Edit, Delete
- Không phải là single form submission
- Focus vào database operations

**Decision**: → **Template D - CRUD Operations**

**Rationale**: Template D được optimize cho:
- Database integrity testing
- State transitions (create → edit → delete)
- List view with pagination/sorting
- Multiple page objects cho mỗi operation

---

### Rule 2: Search & Filter

**Trigger conditions:**
- Có search box hoặc search form
- Có result table/list
- Có filters, sorting, pagination

**Decision**: → **Template E - Search & Filter**

**Rationale**: Template E được optimize cho:
- Dynamic results testing
- Filter combinations
- Result verification
- Performance testing với large datasets

---

### Rule 3: Multi-Step Form (Wizard)

**Trigger conditions:**
- Form có 2+ steps/pages
- User phải complete step 1 trước khi sang step 2
- Có navigation buttons (Next, Previous, Submit)

**Decision**: → **Template C - Multi-Step Form**

**Rationale**: Template C được optimize cho:
- State management giữa các steps
- Navigation handling
- Progress tracking
- Data persistence across steps

---

### Rule 4: Simple Form

**Trigger conditions:**
- Single-page form
- < 5 fields HOẶC 5-10 fields với simple validation
- Validation rules: Chỉ required field + format validation (email, phone)
- Không có business logic phức tạp

**Decision**: → **Template A - Simple Form**

**Features included:**
- ✅ Basic Page Object Model
- ✅ TestNG framework
- ✅ Screenshot on failure
- ✅ Logging framework
- ✅ Configuration management
- ❌ ExtentReports (optional)
- ❌ Data-driven testing (optional)
- ❌ Retry mechanism (optional)

**Rationale**: 
- Lightweight setup
- Fast to implement (1-2 days)
- Sufficient cho simple use cases

---

### Rule 5: Complex Form

**Trigger conditions:**
- Single-page form với:
  - > 10 fields HOẶC
  - 5-10 fields với complex validation HOẶC
  - Business logic validation HOẶC
  - Cross-field validation

**Examples of complex validation:**
- Age must be 18+ (calculated from DOB)
- Student ID must follow pattern SV + 6 digits
- Email must be unique in database
- Start date < End date
- Password strength requirements
- Dependent dropdowns (Province → District → Ward)

**Decision**: → **Template B - Complex Form**

**Features included:**
- ✅ Full Page Object Model
- ✅ TestNG framework
- ✅ All 8 Level 2 features:
  - Data-driven testing
  - Screenshot on failure
  - Logging framework
  - Retry mechanism
  - ExtentReports
  - Configuration management
  - Explicit waits
  - Parallel execution

**Rationale**:
- Comprehensive testing needed
- More test cases to cover all scenarios
- Professional reporting required
- Longer timeline (2-4 weeks)

---

## TEMPLATE COMPARISON TABLE

| Criteria | Template A | Template B | Template C | Template D | Template E |
|:---------|:----------:|:----------:|:----------:|:----------:|:----------:|
| **Use Case** | Simple Form | Complex Form | Multi-Step | CRUD | Search |
| **Fields** | <10 | >10 or complex | Any | N/A | N/A |
| **Steps** | 1 | 1 | 2+ | Multiple pages | 1 |
| **Validation** | Simple | Complex | Any | DB integrity | Query results |
| **Timeline** | 1-2 days | 2-4 weeks | 3-5 days | 2-3 weeks | 1-2 weeks |
| **Level 2 Features** | 5/8 | 8/8 | 6/8 | 7/8 | 6/8 |
| **Complexity** | ⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |

---

## DECISION EXAMPLES

### Example 1: Contact Form

**Analysis:**
- Type: Form Testing
- Steps: 1 (single-page)
- Fields: 4 (Name, Email, Subject, Message)
- Validation: Required + Email format only

**Decision Path:**
```
Form Testing → Single-Step → <5 Fields → Simple Validation
→ Template A (Simple Form)
```

---

### Example 2: Student Registration

**Analysis:**
- Type: Form Testing
- Steps: 1 (single-page)
- Fields: 12 (Student ID, Name, DOB, Email, Phone, Address, etc.)
- Validation: Complex (ID pattern, Age >18, Email unique, Phone format)

**Decision Path:**
```
Form Testing → Single-Step → >10 Fields
→ Template B (Complex Form)
```

---

### Example 3: E-commerce Checkout

**Analysis:**
- Type: Form Testing
- Steps: 3 (Shipping → Payment → Review)
- Fields: Step 1 (5), Step 2 (4), Step 3 (summary)
- Validation: Moderate (required, format, payment validation)

**Decision Path:**
```
Form Testing → Multi-Step
→ Template C (Multi-Step Form)
```

---

### Example 4: User Management System

**Analysis:**
- Type: CRUD Operations
- Pages: User List, Create User, Edit User, Delete User
- Focus: Database operations, user state transitions

**Decision Path:**
```
CRUD Operations
→ Template D (CRUD Operations)
```

---

### Example 5: Product Catalog with Search

**Analysis:**
- Type: Search & Filter
- Features: Search box, category filters, price range, sorting, pagination
- Focus: Dynamic results, filter combinations

**Decision Path:**
```
Search & Filter
→ Template E (Search & Filter)
```

---

## AI CHATBOT WORKFLOW

### Sau khi có Problem Analysis Document:

**Step 1**: Announce decision process
```
Dựa trên Problem Analysis Document, tôi sẽ xác định template phù hợp
nhất cho bài toán của bạn.
```

**Step 2**: Walk through decision tree
```
Q1: Loại bài toán = Form Testing ✓
Q2: Single-step form ✓
Q3: Số fields = [X] → [< 5 / 5-10 / >10]
Q4: Validation complexity = [Simple/Complex]
```

**Step 3**: Announce decision with rationale
```
📊 KẾT QUẢ PHÂN TÍCH:

Dựa trên các đặc điểm:
- [Characteristic 1]
- [Characteristic 2]
- [Characteristic 3]

Tôi recommend sử dụng **Template [X]: [Name]**

LÝ DO:
- [Reason 1: Why this template fits]
- [Reason 2: What it covers]
- [Reason 3: Timeline consideration]

FEATURES included:
- [List features from Feature Selection Matrix]

TIMELINE estimate:
- [X days/weeks]
```

**Step 4**: Ask for confirmation
```
Bạn có đồng ý với decision này không?
- Nếu YES → Tiếp tục sang File 05 (Feature Selection Matrix)
- Nếu NO → Thảo luận lại decision criteria
```

---

## OVERRIDE RULES

### Khi nào có thể override decision?

**Scenario 1**: User có constraint về timeline
```
Template B được recommend nhưng user chỉ có 3 ngày
→ Có thể downgrade sang Template A + bớt features
→ Trade-off: Coverage giảm nhưng đáp ứng timeline
```

**Scenario 2**: User muốn learning project
```
Bài toán simple nhưng user muốn học full features
→ Có thể upgrade từ Template A sang Template B
→ Trade-off: Overkill nhưng học được nhiều hơn
```

**Scenario 3**: User có specific requirements
```
Template recommend không include CI/CD
→ Có thể customize thêm features
→ Reference File 10-Template-Library cho customization
```

**IMPORTANT**: Luôn document override reason trong Problem Analysis Document

---

## VALIDATION CHECKLIST

Trước khi move sang File 05, verify:

- [ ] Decision path đã được walk through đầy đủ
- [ ] Template được chọn có rationale rõ ràng
- [ ] Timeline estimate đã được thảo luận
- [ ] User confirm decision hoặc đã discuss override
- [ ] Features sẽ được implement đã được preview

---

## FALLBACK STRATEGY

**Nếu bài toán không fit vào 5 templates:**

1. Identify closest template
2. List specific differences
3. Propose customization plan
4. Consult File 01-Framework-Architecture.md cho generic guidance
5. Document custom approach trong implementation plan

---

**⏭️ SAU KHI CHỌN XONG TEMPLATE: [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md)**

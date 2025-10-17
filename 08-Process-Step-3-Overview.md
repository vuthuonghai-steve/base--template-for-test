# 🔄 PROCESS STEP 3 - ITERATIVE IMPLEMENTATION WITH CHECKPOINTS

## MỤC ĐÍCH CỦA BƯỚC NÀY

Bước 3 là **execution phase** - nơi code được viết. Quan trọng nhất: implement theo **milestones** với **checkpoints** thường xuyên để user có thể review và adjust.

---

## 📋 STEP OVERVIEW

| Attribute | Detail |
|:----------|:-------|
| **Input** | Implementation Plan (approved từ Step 2) + User provides URL + Access |
| **Process** | Build TAF từng milestone, checkpoint sau mỗi milestone |
| **Output** | Working Test Automation Framework (src/test/ code ONLY) |
| **Duration** | 1-2 weeks (TAF only - NO application build) |
| **Success Criteria** | All milestones complete, all checkpoints passed, tests running against REAL app |

---

## 🎯 CORE PRINCIPLES

### Principle 1: NO BIG BANG DELIVERY

**❌ WRONG**:
```
Build everything → Show user at the end → User: "Not what I wanted"
→ Massive refactoring needed
```

**✅ CORRECT**:
```
Verify Prerequisites → User confirms URL + Access
→ Build Milestone 3 (TAF Core) → Checkpoint → User approves
→ Build Milestone 4 (Level 2 Features) → Checkpoint → User approves
→ Build Milestone 5 (Documentation) → ...
```

---

### Principle 2: WORKING SOFTWARE AT EACH CHECKPOINT

Mỗi checkpoint phải có **working, demonstrable deliverable**:
- Prerequisites: User provides URL, credentials, and confirms app is accessible
- Milestone 3: TAF Core - Tests có thể chạy và pass against REAL application
- Milestone 4: Enhanced features working (ExtentReports, Data-Driven, etc.)
- Milestone 5: Documentation complete với test execution results

**Không được**: "Code đã viết 80% nhưng chưa chạy được"

---

### Principle 3: USER FEEDBACK DRIVES ITERATION

Nếu user không hài lòng ở checkpoint:
- **Iterate immediately** at that milestone
- **Fix trước khi move forward**
- Không để tích lũy changes cuối project

---

## 🗺️ MILESTONE EXECUTION FRAMEWORK

### General Milestone Structure

```
MILESTONE [N]: [Name]
│
├─► PHASE 1: Planning (5-10% of milestone time)
│   - Review requirements for this milestone
│   - Identify files to create/modify
│   - Note dependencies
│
├─► PHASE 2: Implementation (70-80% of milestone time)
│   - Write code incrementally
│   - Test as you go (unit/manual testing)
│   - Document non-obvious decisions
│
├─► PHASE 3: Self-QA (10-15% of milestone time)
│   - Run checklist (see below)
│   - Fix obvious issues
│   - Prepare demo for user
│
└─► CHECKPOINT: User Review
    - Demo deliverables
    - Get feedback
    - Iterate if needed
    - Get approval to proceed
```

---

## 🔍 M3/M4 BOUNDARY CLARIFICATION

### TẠI SAO CẦN PHÂN BIỆT RÕ RÀNG?

**Vấn đề**: File 08 phiên bản cũ không định nghĩa rõ ràng đâu là "core framework" và đâu là "enhancement features".

**Giải pháp**: Đồng bộ M3/M4 với Phase Priority trong File 05 và Architecture trong File 01.

---

### MILESTONE 3 vs MILESTONE 4 - PHÂN BIỆT RÕ RÀNG

| Tiêu chí | **M3: TAF Core - Foundation** | **M4: Level 2 Features - Enhancement** |
|:---------|:------------------------------|:---------------------------------------|
| **Mục đích** | Xây dựng nền tảng framework ổn định, có thể debug | Mở rộng khả năng và tối ưu hiệu suất |
| **Priority** | CRITICAL - BẮT BUỘC cho mọi Template | OPTIONAL/MEDIUM - Tùy theo Template và yêu cầu |
| **Phase** | Phase 1: FOUNDATION (File 05) | Phase 2 & 3: ENHANCEMENT/OPTIMIZATION (File 05) |
| **Thời gian** | Fixed: 3 hours (Phase 1 features) + 0.5-2 days (Core Classes) | Variable: Depends on features selected |
| **Packages** | config/, listeners/, utils/, pages/ (Core), tests/ (Core) | utils/ (advanced), testdata/, testng/ (parallel) |

---

### M3 - BẮT BUỘC TÍCH HỢP PHASE 1 FEATURES

**M3 PHẢI bao gồm tất cả Phase 1: FOUNDATION features:**

| # | Feature | Files to Create | Rationale |
|:--|:--------|:----------------|:----------|
| 1 | **Configuration Management** | `config/TestConfig.java`<br>`resources/config/config.properties` | Tránh hard-code URLs, timeouts |
| 2 | **Logging Framework** | `resources/logging/logback.xml`<br>SLF4J usage in code | Professional logging thay System.out |
| 3 | **Explicit Waits** | `utils/WaitHelper.java` | Dynamic waits thay Thread.sleep() |
| 4 | **Screenshot on Failure** | `utils/ScreenshotHelper.java`<br>`listeners/TestListener.java` | Auto capture khi test fail |

**Plus Core Classes** (đã có trong M3 cũ):
- `pages/BasePage.java`, `tests/BaseTest.java`
- `pages/{{PAGE_NAME}}Page.java`, `tests/{{TEST_CLASS}}Test.java`

**Tổng M3 Deliverables**: 10-12 files

---

### M4 - IMPLEMENT PHASE 2 & 3 FEATURES (SELECTIVE)

**M4 CHỈ implement các features đã được chọn trong Feature Selection Matrix:**

| # | Feature | Status | Templates yêu cầu |
|:--|:--------|:------:|:------------------|
| 1 | **Data-Driven Testing** | Phase 2 | B (Required), C, D, E |
| 5 | **ExtentReports** | Phase 2 | B, C, D, E (Required hoặc Optional) |
| 4 | **Retry Mechanism** | Phase 3 | B, D (Required), C, E (Optional) |
| 8 | **Parallel Execution** | Phase 3 | B (Required), E (Optional) |

**Key Point**:
- Template A: M4 có thể **SKIP hoàn toàn** (4/8 features đã có trong M3)
- Template B: M4 phải implement **tất cả 4 features** (8/8 Required)
- Template C, D, E: M4 implement **selective** dựa trên ma trận

---

### VÍ DỤ CHO TEMPLATE A (SIMPLE FORM)

**M3 cho Template A:**
```
✅ Phase 1 Features:
   - Config Management (TestConfig.java, config.properties)
   - Logging (logback.xml)
   - Explicit Waits (WaitHelper.java)
   - Screenshot on Failure (ScreenshotHelper.java, TestListener.java)

✅ Core Classes:
   - BasePage.java, BaseTest.java
   - RegistrationPage.java
   - RegistrationTest.java (2-3 tests)

Total: ~4 hours (3h Phase 1 + 1h Core)
```

**M4 cho Template A:**
```
❌ Data-Driven: SKIP (Template A - Optional)
❌ ExtentReports: SKIP (Console reports đủ)
❌ Retry: SKIP (Simple tests ít flaky)
❌ Parallel: SKIP (Not needed)

→ M4 có thể SKIP hoàn toàn cho Template A!
→ Go directly to M5 (Documentation)
```

---

### VÍ DỤ CHO TEMPLATE B (COMPLEX FORM)

**M3 cho Template B:** (Giống Template A - Phase 1 features + Core)

**M4 cho Template B:**
```
✅ Data-Driven: MUST IMPLEMENT (Required)
   - DataHelper.java
   - testdata/*.json

✅ ExtentReports: MUST IMPLEMENT (Required)
   - ExtentManager.java
   - Update TestListener.java

✅ Retry: MUST IMPLEMENT (Required)
   - RetryAnalyzer.java

✅ Parallel: MUST IMPLEMENT (Required)
   - testng-parallel.xml

Total: ~8 hours (Phase 2 & 3)
```

---

### VALIDATION CHECKLIST

Trước khi move từ M3 sang M4, verify:

**M3 Completion Checklist:**
- [ ] Tất cả 4 Phase 1 features đã implemented
- [ ] Config.properties load thành công
- [ ] Logs xuất hiện trong logs/automation.log
- [ ] WaitHelper methods hoạt động
- [ ] Screenshot captured khi test fail
- [ ] Core classes (Base + Page + Test) hoạt động
- [ ] Ít nhất 2-3 tests chạy pass

**M4 Decision Point:**
- [ ] Đã review Feature Selection Matrix cho Template
- [ ] Xác định features nào Required/Optional/Skip
- [ ] User confirm list features cần implement
- [ ] Timeline estimate cho M4 đã được approve

---

## 📝 MILESTONE-BY-MILESTONE GUIDE

⚠️ **CRITICAL NOTE:**
- **Milestones 1 & 2 have been REMOVED** - We do NOT build applications anymore
- Application must **ALREADY EXIST** at a deployed URL
- We start directly at **Milestone 3: TAF Core - Foundation**

For detailed implementation guides:
- **Milestone 3**: See [08A-Milestone-3-TAF-Core.md](08A-Milestone-3-TAF-Core.md)
- **Milestone 4**: See [08B-Milestone-4-Level2-Features.md](08B-Milestone-4-Level2-Features.md)
- **Milestone 5**: See [08C-Milestone-5-Documentation.md](08C-Milestone-5-Documentation.md)

---

**⏭️ NEXT STEPS**: Start with Prerequisites verification in [08A-Milestone-3-TAF-Core.md](08A-Milestone-3-TAF-Core.md)

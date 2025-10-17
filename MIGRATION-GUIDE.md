# 📖 MIGRATION GUIDE: Practice Mode → Real-World Mode

**Version:** 3.0.0
**Migration Date:** 2025-10-17
**Breaking Change:** ⚠️ **YES** - This is a major version upgrade with backward-incompatible changes

---

## 🎯 TL;DR (Too Long; Didn't Read)

**What Changed:**
- ❌ **REMOVED**: Milestones 1 & 2 (Application creation - Frontend & Backend)
- ✅ **ADDED**: Mandatory Prerequisites check (Application must exist)
- ✅ **ADDED**: Real-World Considerations for all templates
- ✅ **OPTIMIZED**: File splitting (33,000 → 19,900 tokens, -40%)

**Impact:**
- **Before (v2.x)**: System helped you build app THEN test it (Practice Mode)
- **After (v3.0)**: System ONLY helps you test EXISTING apps (Real-World Mode)

**Migration Path:**
- If you have old projects from v2.x → They won't work with v3.0 (different scope)
- If you're starting fresh → Perfect! Follow new workflow from File 03

---

## 📊 WHAT CHANGED

### 1. Scope Change (BREAKING)

| Aspect | v2.x (Practice Mode) | v3.0 (Real-World Mode) |
|--------|----------------------|------------------------|
| **Milestones** | M1 (Frontend) + M2 (Backend) + M3-M5 (TAF) | M3-M5 (TAF ONLY) |
| **Duration** | 2-4 weeks (app + test) | 1-2 weeks (test only) |
| **Prerequisites** | None (build from scratch) | Application MUST exist remotely |
| **URLs** | localhost:8080 | https://staging.example.com |
| **Code Output** | src/main/ + src/test/ | src/test/ ONLY |
| **Use Case** | Learning/Practice | Real-world projects |

### 2. Files Removed

**Deleted Content:**
- ❌ Milestone 1: Setup & AUT Frontend (~220 lines from file 08)
- ❌ Milestone 2: AUT Backend & Database (~210 lines from file 08)
- ❌ All references to "Practice Mode" (except historical context)
- ❌ All localhost configuration examples (except warnings)
- ❌ H2 database setup instructions

**Deleted Files:**
- ❌ `08-Process-Step-3-Implementation.md` (split into 4 files)
- ❌ `10-Template-Librar.md` (split into 6 files)

### 3. Files Added

**New Structure Files:**
- ✅ `08-Process-Step-3-Overview.md` - High-level M3-M5 overview
- ✅ `08A-Milestone-3-TAF-Core.md` - M3 detailed implementation
- ✅ `08B-Milestone-4-Level2-Features.md` - M4 features guide
- ✅ `08C-Milestone-5-Documentation.md` - M5 documentation guide
- ✅ `10-Template-Library-Index.md` - Template overview & selection
- ✅ `10A-Template-Simple-Form.md` - Template A (full)
- ✅ `10B-Template-Complex-Form.md` - Template B (full)
- ✅ `10C-Template-Multi-Step.md` - Template C (condensed)
- ✅ `10D-Template-CRUD.md` - Template D (condensed)
- ✅ `10E-Template-Search-Filter.md` - Template E (condensed)

**New Sections Added:**
- ✅ Prerequisites Check (mandatory) in all process files
- ✅ Real-World Considerations in all 5 templates
- ✅ FATAL ERRORS section in CLAUDE.md
- ✅ Mandatory Prerequisites Checklist in CLAUDE.md
- ✅ Real-World vs Practice Mode comparison tables

### 4. Files Updated

| File | Lines Changed | Key Updates |
|------|---------------|-------------|
| **CLAUDE.md** | ~100 lines | Core mission rewritten, FATAL ERRORS added, Prerequisites mandatory |
| **00-MASTER-INDEX.md** | ~50 lines | Purpose updated, workflow revised, Version 3.0.0 documented |
| **01-Framework-Architecture.md** | ~150 lines | src/main/ deleted, Real-World architecture only, Integration points updated |
| **06-Process-Step-1-Analysis.md** | ~80 lines | Phase 1A.5 gate added, Practice Mode removed, URL-first approach |
| **07-Process-Step-2-Design.md** | ~60 lines | M1-M2 removed, Architecture updated, Timeline adjusted |
| **10-Template-Librar.md → split** | ALL | Split into 6 files with cross-references |
| **08-Process-Step-3-Implementation.md → split** | ALL | Split into 4 files, M1-M2 deleted |

---

## 🚨 BREAKING CHANGES

### 1. Milestones Renumbering

**Before (v2.x):**
```
M1: Setup & AUT Frontend
M2: AUT Backend & Database
M3: TAF Core
M4: Level 2 Features
M5: Documentation
M6: Integration Testing (for Complex Form)
```

**After (v3.0):**
```
[DELETED] M1
[DELETED] M2
M3: TAF Core ← START HERE
M4: Level 2 Features
M5: Documentation
[DELETED] M6 (merged into M4)
```

**Impact:**
- ⚠️ Old references to "Milestone 1" or "Milestone 2" are **INVALID**
- ⚠️ All new projects start at **Milestone 3**
- ⚠️ Templates now show "M3-M5" instead of "M1-M6"

### 2. Prerequisites Gate

**Before (v2.x):**
- User could say: "Tôi muốn tạo form đăng ký" → System would build app
- No URL required
- No existing application needed

**After (v3.0):**
- User **MUST** provide: Real URL + Form details + Test credentials
- System **DECLINES** if no URL: "Tôi chỉ có thể test ứng dụng ĐÃ TỒN TẠI..."
- Mandatory checklist in CLAUDE.md lines 76-104

**Impact:**
- ⚠️ Cannot use system for learning/practice (building toy apps)
- ⚠️ Must have access to staging/UAT/production environment
- ⚠️ Must have test credentials and VPN (if required)

### 3. Configuration URLs

**Before (v2.x):**
```properties
# config.properties
app.url=http://localhost:8080
database.url=jdbc:h2:mem:testdb
```

**After (v3.0):**
```properties
# config.properties
app.url=https://staging.example.com
# NO database.url (database is on remote server)
```

**Impact:**
- ⚠️ All examples use real URLs (https://staging...)
- ⚠️ localhost examples **ONLY** appear in "❌ DON'T DO" warnings
- ⚠️ H2 database references removed (except Troubleshooting Guide for legacy support)

### 4. Code Output Scope

**Before (v2.x):**
- System generated:
  - `src/main/java/controller/` - @RestController classes
  - `src/main/java/model/` - @Entity classes
  - `src/main/java/repository/` - JPA repositories
  - `src/test/java/` - TAF code

**After (v3.0):**
- System generates:
  - `src/test/java/` **ONLY** - TAF code

**Impact:**
- ⚠️ **FATAL ERROR** if system generates src/main/ code
- ⚠️ System will **REFUSE** to create controllers, entities, services
- ⚠️ Focus is 100% on test automation framework

---

## 📁 FILE STRUCTURE CHANGES

### Before (v2.x):

```
TEST/
├── 00-MASTER-INDEX.md
├── 01-Framework-Architecture.md
├── ...
├── 08-Process-Step-3-Implementation.md (1,461 lines - HUGE!)
├── 09-Process-Step-4-Execution.md
├── 10-Template-Librar.md (2,837 lines - HUGE!)
├── 11-Best-Practices.md
└── 12-Troubleshooting-Guide.md
```

### After (v3.0):

```
TEST/
├── 00-MASTER-INDEX.md (updated)
├── 01-Framework-Architecture.md (updated)
├── ...
├── 08-Process-Step-3-Overview.md (NEW - 1,200 tokens)
├── 08A-Milestone-3-TAF-Core.md (NEW - 3,800 tokens)
├── 08B-Milestone-4-Level2-Features.md (NEW - 3,200 tokens)
├── 08C-Milestone-5-Documentation.md (NEW - 1,400 tokens)
├── 09-Process-Step-4-Execution.md
├── 10-Template-Library-Index.md (NEW - 900 tokens)
├── 10A-Template-Simple-Form.md (NEW - 2,800 tokens)
├── 10B-Template-Complex-Form.md (NEW - 2,700 tokens)
├── 10C-Template-Multi-Step.md (NEW - 1,200 tokens)
├── 10D-Template-CRUD.md (NEW - 1,400 tokens)
├── 10E-Template-Search-Filter.md (NEW - 1,300 tokens)
├── 11-Best-Practices.md (updated)
├── 12-Troubleshooting-Guide.md
└── MIGRATION-GUIDE.md (THIS FILE)
```

**Context Reduction:**
- Before: ~33,000 tokens (large files)
- After: ~19,900 tokens (split files)
- **Savings: 40%**

---

## 🔧 HOW TO MIGRATE

### Scenario 1: You Have Existing v2.x Projects

**Bad News:**
- v2.x projects (with M1-M2 milestones) are **NOT compatible** with v3.0
- They were "Practice Mode" projects (built toy apps for learning)
- v3.0 is "Real-World Mode" (tests existing production apps)

**Recommendation:**
- Keep v2.x projects as-is for learning purposes
- Use v3.0 for NEW real-world testing projects
- Do **NOT** try to convert v2.x → v3.0 (different goals)

### Scenario 2: You're Starting Fresh

**Great News:**
- v3.0 is **BETTER** for real-world testing
- Follow this workflow:

```
1. Verify Prerequisites:
   [ ] Application URL: https://staging.example.com/...
   [ ] Test credentials: username + password
   [ ] VPN access (if required)
   [ ] Form/page details: fields, validation rules

2. Start with File 03:
   → 03-Problem-Analysis-Framework.md
   → Classify your problem (Simple/Complex/CRUD/etc.)

3. Follow 4-Step Process:
   → Step 1 (File 06): Analysis - Gather app details
   → Step 2 (File 07): Design - Plan your TAF
   → Step 3 (Files 08*): Implementation - Build TAF (M3-M5 only)
   → Step 4 (File 09): Execution - Run tests

4. Reference Templates:
   → File 10-Template-Library-Index.md for overview
   → Files 10A-10E for specific template details
```

### Scenario 3: You're an AI Chatbot Integration

**Update Required:**
- Replace CLAUDE.md with new version (v3.0)
- Update file references:
  - `08-Process-Step-3-Implementation.md` → `08-Process-Step-3-Overview.md`
  - `10-Template-Librar.md` → `10-Template-Library-Index.md`
- Enable **Prerequisites Gate** (mandatory check before starting)
- Configure **Decline Response** for no-URL scenarios

---

## 💡 NEW FEATURES

### 1. Mandatory Prerequisites Check (CLAUDE.md lines 76-104)

**What:**
- System **CANNOT proceed** without receiving from user:
  - ✅ Real URL (https://staging.example.com/...)
  - ✅ Test credentials
  - ✅ Form/page details
  - ✅ Technical context

**Why:**
- Prevents wasted time building tests for non-existent apps
- Ensures quality requirements gathering
- Real-world projects ALWAYS have staging URLs

**How to Use:**
- User provides URL → System proceeds
- User doesn't provide URL → System politely declines

### 2. Real-World Considerations (All 5 Templates)

**What:**
- Each template (A-E) now has section: "Real-World Considerations"
- Examples:
  - Template A: Application Access, Environment Config, Test Data Strategy
  - Template B: Business Logic Documentation, Unique Constraint Testing, Dependent Field Dynamics
  - Template C: Session Timeout, Direct URL Access, Browser Back Button
  - Template D: Shared Database Environment, Pagination Testing, Delete Confirmation Dialogs
  - Template E: Dynamic Results Loading (AJAX), Filter State Persistence, Search Result Caching

**Why:**
- Real staging environments have challenges localhost doesn't
- Network latency, shared data, authentication flows
- These weren't covered in Practice Mode

**How to Use:**
- Read "Real-World Considerations" in your chosen template
- Plan mitigations BEFORE writing code
- Reference during troubleshooting

### 3. File Splitting for Context Optimization

**What:**
- File 08 split: 1 file (18,000 tokens) → 4 files (avg 2,400 tokens)
- File 10 split: 1 file (15,000 tokens) → 6 files (avg 1,717 tokens)
- Total reduction: 40%

**Why:**
- AI chatbots have context limits (~100k-200k tokens)
- Large files caused slow loading and truncation
- Split files load faster, more focused

**How to Use:**
- Start with Overview/Index files (08-Overview, 10-Index)
- Drill down to specific sections (08A, 10B, etc.) as needed
- Cross-references guide you to related content

### 4. FATAL ERRORS Detection (CLAUDE.md lines 48-71)

**What:**
- 4 fatal errors that trigger **IMMEDIATE STOP**:
  1. Create src/main/ code
  2. Use localhost URLs
  3. Assume application behavior
  4. Skip authentication

**Why:**
- These were common mistakes in v2.x
- They violate Real-World Mode principles
- Early detection prevents wasted effort

**How to Use:**
- AI chatbot checks before generating code
- If violated → Shows warning and stops
- User can clarify requirements

---

## 📚 REMOVED FEATURES

### 1. Milestone 1: Setup & AUT Frontend

**What Was Removed:**
- HTML form creation instructions
- CSS styling guidelines
- Thymeleaf template examples
- Spring Boot @Controller setup for frontend

**Why Removed:**
- Out of scope for Real-World Mode
- User already has frontend deployed
- TAF shouldn't build UI

**Alternative:**
- Inspect existing frontend with F12 (Inspector)
- Copy HTML snippets for locator extraction
- Document existing form structure

### 2. Milestone 2: AUT Backend & Database

**What Was Removed:**
- @Entity class creation
- @Repository interface setup
- @Service business logic
- H2 database configuration
- application.properties for Spring Boot app

**Why Removed:**
- Out of scope for Real-World Mode
- User already has backend deployed
- TAF shouldn't build APIs

**Alternative:**
- Use API documentation (Swagger/OpenAPI)
- Test against existing API endpoints
- Focus on TAF → RestAssured for API testing

### 3. Practice Mode Workflow

**What Was Removed:**
- "Build then test" approach
- Local development setup instructions
- H2 console troubleshooting (mostly - kept in File 12 for legacy)
- localhost configuration examples (as recommended practices)

**Why Removed:**
- Fundamentally different goal (learning vs real-world)
- Confusing to mix both modes
- Real-world focus requires different mindset

**Alternative:**
- For learning: Use v2.x (archived)
- For real-world: Use v3.0 (current)
- Clear separation of concerns

---

## 🐛 BUGS FIXED IN v3.0

### Bug #1: Milestone 1 Reference in Design File

**Issue:**
- File: `07-Process-Step-2-Design.md` line 561
- Text: "Reply 'Approved' để tôi bắt đầu Milestone 1"
- **BUG**: Milestone 1 doesn't exist in v3.0!

**Fix:**
- Changed to: "Reply 'Approved' để tôi bắt đầu Milestone 3 (TAF Core)"
- Validated during P3 automated checks

**Impact:**
- Would have confused users/chatbots
- Fixed before v3.0 release

---

## 📝 DOCUMENTATION UPDATES

### Updated Files:

1. **CLAUDE.md**
   - Core mission rewritten (lines 14-28)
   - FATAL ERRORS added (lines 48-71)
   - Prerequisites mandatory (lines 76-104)

2. **00-MASTER-INDEX.md**
   - Purpose statement updated (lines 3-13)
   - Workflow revised (lines 71-123)
   - Version 3.0.0 documented (lines 191-211)

3. **01-Framework-Architecture.md**
   - src/main/ layer deleted
   - Real-World integration points added
   - Comparison table added

4. **06-Process-Step-1-Analysis.md**
   - Phase 1A.5 gate added (mandatory URL)
   - Practice Mode questions removed
   - Timeline adjusted (3-4 weeks → 1-2 weeks)

5. **07-Process-Step-2-Design.md**
   - M1-M2 removed from roadmaps
   - Architecture shows src/test/ ONLY
   - Timeline estimates adjusted

6. **All 5 Templates (10A-10E)**
   - Prerequisites sections added
   - Real-World Considerations added
   - M1-M2 milestones removed
   - Timeline adjusted

### Cross-References Updated:

- `00-MASTER-INDEX.md` → Points to new 08* and 10* files
- `07-Process-Step-2-Design.md` → References 08-Overview
- `11-Best-Practices.md` → References 08-Overview
- `context-prompt.md` → References 08-Overview

---

## 🎓 LEARNING PATH

### For New Users:

```
Week 1: Understand Real-World Mode
├─ Day 1: Read CLAUDE.md + 00-MASTER-INDEX.md
├─ Day 2: Read 03-Problem-Analysis-Framework.md
├─ Day 3: Read 04-Decision-Tree.md + 05-Feature-Selection-Matrix.md
├─ Day 4: Read Process Guide (06-09)
└─ Day 5: Read Templates (10-Index, 10A-10E as needed)

Week 2: Practice with Real Project
├─ Day 1-2: Gather prerequisites (URL, credentials, form details)
├─ Day 3: Complete Step 1 (Analysis)
├─ Day 4: Complete Step 2 (Design)
└─ Day 5: Start Step 3 (Implementation - M3)

Week 3: Build TAF
├─ Day 1-2: M3 (TAF Core)
├─ Day 3-4: M4 (Level 2 Features)
└─ Day 5: M5 (Documentation)

Week 4: Execute & Iterate
├─ Day 1-2: Run tests, fix issues
├─ Day 3-4: Generate reports, document
└─ Day 5: Handover to team
```

### For v2.x Users Migrating:

```
Step 1: Understand Scope Change
→ Read "WHAT CHANGED" section above
→ Understand: v3.0 is NOT for building apps

Step 2: Identify Your Use Case
→ Learning/Practice? → Stay on v2.x
→ Real-world testing? → Migrate to v3.0

Step 3: Gather Real-World Prerequisites
→ Get staging URL from DevOps/Team Lead
→ Get test credentials
→ Document existing form/page structure

Step 4: Follow New Workflow
→ Start with File 03 (Problem Analysis)
→ Skip looking for M1-M2 (they don't exist)
→ Begin directly at M3 (TAF Core)
```

---

## ⚠️ BACKWARD COMPATIBILITY

**v3.0 is NOT backward compatible with v2.x**

| Aspect | Compatible? | Notes |
|--------|-------------|-------|
| **Projects** | ❌ NO | v2.x projects (with M1-M2) won't work |
| **Workflows** | ❌ NO | Different starting points (URL vs scratch) |
| **File references** | ❌ NO | File 08 and 10 split into multiple files |
| **Milestones** | ❌ NO | M1-M2 deleted, numbering changed |
| **Documentation** | ⚠️ PARTIAL | Core concepts same, structure different |

**Recommendation:**
- Treat v3.0 as a **NEW system** for real-world testing
- Keep v2.x archived for learning/practice purposes
- Do NOT attempt to "upgrade" v2.x projects to v3.0

---

## 📞 SUPPORT & TROUBLESHOOTING

### Common Migration Issues:

**Issue 1: "Where is Milestone 1?"**
- **Answer**: Deleted in v3.0. Start at Milestone 3 (TAF Core).
- **File**: See `08A-Milestone-3-TAF-Core.md`

**Issue 2: "System refuses to build my app"**
- **Answer**: v3.0 ONLY tests EXISTING apps. Not for building apps.
- **Solution**: Deploy app first, then provide URL for testing.

**Issue 3: "File 10-Template-Library.md not found"**
- **Answer**: Split into 6 files in v3.0.
- **Solution**: Use `10-Template-Library-Index.md` as starting point.

**Issue 4: "localhost examples gone?"**
- **Answer**: Yes, v3.0 uses real staging URLs only.
- **Solution**: Use `https://staging.example.com` pattern.

**Issue 5: "Can't find M6 in Complex Form?"**
- **Answer**: M6 (Integration Testing) merged into M4 in v3.0.
- **Solution**: See `08B-Milestone-4-Level2-Features.md`.

---

## 📊 MIGRATION STATISTICS

### Content Changes:

| Metric | Before (v2.x) | After (v3.0) | Change |
|--------|---------------|--------------|--------|
| **Files** | 12 files | 20 files | +8 files |
| **Large Files** | 2 (>1000 lines) | 0 | -2 files |
| **Total Lines** | ~14,000 lines | ~13,400 lines | -600 lines |
| **Context Tokens** | ~33,000 tokens | ~19,900 tokens | -40% |
| **M1-M2 Content** | ~432 lines | 0 lines | -100% |
| **Prerequisites** | 0 checklists | 3 mandatory checklists | NEW |
| **Real-World Sections** | 0 sections | 5 templates × 3-5 items | NEW |

### Development Time:

| Phase | Duration | Completed |
|-------|----------|-----------|
| **P0: Critical Files** | 2 hours | ✅ 2025-10-17 |
| **P1: High Priority** | 1.25 hours | ✅ 2025-10-17 |
| **P2: Context Optimization** | 2.5 hours | ✅ 2025-10-17 |
| **P3: Final Validation** | 1 hour | ✅ 2025-10-17 |
| **Total** | 6.75 hours | ✅ COMPLETED |

---

## 🎉 BENEFITS OF v3.0

### For Users:

1. **Faster Time to Value**: Skip M1-M2 → Start testing immediately (1-2 weeks vs 2-4 weeks)
2. **Real-World Focus**: Handles staging environment challenges (authentication, VPN, shared data)
3. **Better Context**: Smaller files (40% reduction) → Faster AI loading
4. **Clearer Scope**: No confusion about "am I building app or test?"
5. **Professional Output**: Real URLs, proper test data cleanup, stakeholder-ready reports

### For AI Chatbots:

1. **Reduced Token Usage**: 33k → 19.9k tokens (-40%)
2. **Clearer Instructions**: FATAL ERRORS prevent scope creep
3. **Mandatory Gates**: Prerequisites check before starting
4. **Better Structure**: Split files = focused context loading
5. **Consistent Output**: Markdown tables, code blocks, real URLs

### For Teams:

1. **Production-Ready**: Tests work on actual staging environments
2. **Scalable**: Real-world considerations prevent common pitfalls
3. **Maintainable**: Modular files easier to update
4. **Professional**: ExtentReports, proper documentation, cleanup
5. **Onboarding**: Clear "Real-World Mode" distinction

---

## 🚀 WHAT'S NEXT

### Immediate Actions (Post-Migration):

- [x] Complete all P0-P3 priorities
- [x] Validate forbidden terms removed
- [x] Fix bugs (Milestone 1 reference)
- [x] Create MIGRATION-GUIDE.md (this file)
- [ ] Commit to git: `git commit -m "feat: Migrate to Real-World Mode v3.0 - Breaking Change"`
- [ ] Tag release: `git tag -a v3.0.0 -m "Real-World Mode Only"`
- [ ] Archive v2.x: `git branch archive/v2-practice-mode`
- [ ] Update documentation README (if exists)

### Future Enhancements (v3.1+):

**Potential Additions:**
1. **More Templates**: API-only testing, Mobile app testing, E2E workflows
2. **CI/CD Integration**: Jenkins, GitHub Actions, GitLab CI examples
3. **Cloud Testing**: BrowserStack, Sauce Labs integration guides
4. **Performance Testing**: JMeter integration for load testing
5. **Security Testing**: OWASP ZAP integration examples

**Optimization Opportunities:**
1. Further file splitting if needed (keep < 3,000 tokens per file)
2. Add video tutorials/walkthroughs
3. Interactive decision tree tool
4. Template generator CLI tool
5. Automated validation scripts

---

## 📖 REFERENCE LINKS

### Internal Documentation:

- **Getting Started**: `00-MASTER-INDEX.md`
- **System Prompt**: `CLAUDE.md`
- **Problem Analysis**: `03-Problem-Analysis-Framework.md`
- **Template Selection**: `04-Decision-Tree.md`
- **Implementation**: `08-Process-Step-3-Overview.md`
- **Templates**: `10-Template-Library-Index.md`
- **Best Practices**: `11-Best-Practices.md`
- **Troubleshooting**: `12-Troubleshooting-Guide.md`

### Migration Files:

- **Checklist**: `MIGRATION-CHECKLIST.md` (execution tracking)
- **Guide**: `MIGRATION-GUIDE.md` (this file)
- **Plan**: `Temple\2.md` (detailed migration plan)

### External Resources:

- Selenium 4 Docs: https://www.selenium.dev/documentation/
- TestNG 7 Docs: https://testng.org/doc/documentation-main.html
- ExtentReports: https://www.extentreports.com/
- Spring Boot 3.2: https://spring.io/projects/spring-boot

---

## 🙏 ACKNOWLEDGMENTS

**Migration Completed By:** Claude Code
**Migration Plan:** Temple\2.md (Final Migration Plan v3.0)
**Execution Time:** 2025-10-17 (6.75 hours total)
**Context Reduction Achieved:** 40% (33k → 19.9k tokens)
**Files Created:** 10 new files
**Files Deleted:** 2 large files
**Bugs Fixed:** 1 (Milestone 1 reference)
**Validation Status:** ✅ ALL PASS

---

**Last Updated:** 2025-10-17
**Version:** 3.0.0
**Status:** ✅ COMPLETED

**🎉 Welcome to Real-World Mode! Happy Testing! 🚀**

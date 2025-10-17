# 📝 TEMPLATE D: CRUD OPERATIONS

**SEE ALSO**:
- [10-Template-Library-Index.md](10-Template-Library-Index.md) for template selection guide
- [04-Decision-Tree.md](04-Decision-Tree.md) for decision tree
- [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md) for feature selection

---

## Overview

**Use Case**: Quản lý entities với Create, Read, Update, Delete

**Characteristics**:
- List/Table page với pagination, sorting, search
- Create page/modal
- Edit page/modal
- Delete confirmation
- Database operations testing

**Timeline**: 2-3 weeks

**Features Included**: 7/8 Level 2 Features
- ✅ All Phase 1 & 2 features
- ✅ Retry Mechanism (DB operations có thể timeout)
- ❌ Parallel Execution (DB state conflicts)

---

## Architecture

```
CRUDProject/
├── src/
│   ├
│   │
│   └── test/
│       ├── java/
│       │   ├── pages/
│       │   │   ├── ListPage.java              # Table operations
│       │   │   ├── CreatePage.java            # Create form
│       │   │   └── EditPage.java              # Edit form
│       │   └── tests/
│       │       ├── CreateTest.java            # Create tests
│       │       ├── ReadTest.java              # List/Search tests
│       │       ├── UpdateTest.java            # Edit tests
│       │       ├── DeleteTest.java            # Delete tests
│       │       └── IntegrationTest.java       # Full CRUD flow
│       └── resources/
│           └── testdata/
│               ├── create_data.json
│               └── update_data.json
```

---

## Milestones Breakdown

⚠️ **PREREQUISITES** (User responsibility - 0 days):

Before starting M3, verify the CRUD application ALREADY EXISTS and is accessible:

**Required Information**:
- ✅ Application URLs (List, Create, Edit pages)
- ✅ Entity details (fields, types, validation rules)
- ✅ Table/List page features (pagination, sorting, search)
- ✅ CRUD operations behavior
- ✅ Test credentials
- ✅ Database cleanup strategy

**CRUD Flow Documentation Example**:
| Operation | URL | Key Elements | Expected Behavior |
|-----------|-----|--------------|-------------------|
| CREATE | /entity/create | field1, field2, submitBtn | Success → redirect to list |
| READ | /entity/list | searchInput, table, paginationLinks | Display all entities |
| UPDATE | /entity/edit/{id} | field1, field2, saveBtn | Success → redirect to list |
| DELETE | /entity/delete/{id} | deleteBtn, confirmDialog | Confirmation → remove from DB |

---

### M3: TAF - CRUD Tests (2-3 days)
**Deliverables**:
- Create tests
- Read/List/Search tests
- Update tests
- Delete tests
- Integration tests (full CRUD cycle)

---

### M4: Level 2 Features & Documentation (2 days)
**Deliverables**:
- ExtentReports
- Data-driven tests
- Retry mechanism
- Test cases documentation
- README

---

## Common Patterns cho Template D

### Pattern 1: Verify Create Reflected in List
```java
@Test
public void testCreateReflectedInList() {
    // Create entity
    String uniqueId = createEntity();

    // Go to list
    driver.get(listUrl);
    listPage.searchFor(uniqueId);

    // Verify present
    Assert.assertTrue(listPage.isRowPresent(uniqueId));
}
```

### Pattern 2: Verify Delete Removes from List
```java
@Test
public void testDeleteRemovesFromList() {
    // Create entity
    String id = createEntity();

    // Delete it
    driver.get(listUrl);
    listPage.searchFor(id);
    listPage.clickDelete(0);

    // Verify removed
    listPage.searchFor(id);
    Assert.assertEquals(listPage.getRowCount(), 0);
}
```

### Pattern 3: Concurrent Modification
```java
@Test
public void testConcurrentModification() {
    // User A edits entity
    editPage1.updateField("Value A");

    // User B also edits same entity (different session)
    editPage2.updateField("Value B");

    // User A saves first
    editPage1.clickSave();
    Assert.assertTrue(editPage1.isSuccessDisplayed());

    // User B saves later
    editPage2.clickSave();

    // Should show conflict error
    Assert.assertTrue(editPage2.getErrorMessage().contains("modified"));
}
```

---

## Real-World Considerations for Template D

### 1. Shared Database Environment Challenges
**Problem**: Multiple testers or automated tests writing to same staging database

**Solutions**:
```java
// Strategy 1: Unique test data per test run
String uniquePrefix = "TEST_" + System.currentTimeMillis() + "_";
String entityName = uniquePrefix + "Student";

// Strategy 2: Test data isolation with cleanup
@AfterMethod
public void cleanupTestData() {
    if (createdIds != null && !createdIds.isEmpty()) {
        for (Long id : createdIds) {
            deleteEntityViaAPI(id); // Cleanup via API, not UI
        }
    }
}
```

### 2. Pagination Testing in Real Environment
**Challenge**: Test data mixed with production/staging data

**Solution**:
```java
// Search for specific test data instead of assuming page 1
public void testCRUDwithIsolation() {
    String uniqueId = "AUTO_TEST_" + UUID.randomUUID();

    // Create
    createEntity(uniqueId);

    // Search to find it (not browse page 1)
    listPage.searchFor(uniqueId);
    Assert.assertTrue(listPage.isRowPresent(uniqueId));
}
```

### 3. Delete Confirmation Dialogs
**Real-World Variation**: JavaScript confirm() vs modal dialog vs inline confirmation

**Solution**: Handle different confirmation types dynamically

### 4. Soft Delete vs Hard Delete
**Real-World Issue**: Some apps use soft delete (status=deleted) instead of removing from DB

**Test Consideration**: Verify actual deletion behavior via DB query or API

---

## Pitfalls & Solutions

### Pitfall 1: Test Data Pollution
**Problem**: Tests leave data in DB, affect other tests

**Solution**: Cleanup after each test using @AfterMethod

### Pitfall 2: Pagination Issues
**Problem**: Test assumes all data on page 1

**Solution**: Iterate through all pages to find test data

### Pitfall 3: Stale Element Reference
**Problem**: Element becomes stale sau delete/update

**Solution**: Re-fetch elements before actions

---

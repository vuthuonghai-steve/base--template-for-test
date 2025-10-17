# 📝 TEMPLATE E: SEARCH & FILTER

**SEE ALSO**:
- [10-Template-Library-Index.md](10-Template-Library-Index.md) for template selection guide
- [04-Decision-Tree.md](04-Decision-Tree.md) for decision tree
- [05-Feature-Selection-Matrix.md](05-Feature-Selection-Matrix.md) for feature selection

---

## Overview

**Use Case**: Search page với results, filters, sorting

**Characteristics**:
- Search box/form
- Results table/list
- Multiple filters (category, price, date, etc.)
- Sorting options
- Pagination for results

**Timeline**: 1-2 weeks

**Features Included**: 6/8 Level 2 Features
- ✅ All Phase 1 features
- ✅ Data-Driven Testing
- ✅ ExtentReports
- ✅ Retry (Optional)
- ✅ Parallel Execution (Optional)

---

## Architecture

```
SearchProject/
├── src/
│   ├──
│   │
│   └── test/
│       ├── java/
│       │   ├── pages/
│       │   │   └── SearchPage.java           # Search + Results
│       │   └── tests/
│       │       ├── SearchTest.java           # Search tests
│       │       ├── FilterTest.java           # Filter tests
│       │       └── SortingTest.java          # Sorting tests
│       └── resources/
│           └── testdata/
│               └── search_queries.json       # Test queries
```

---

## Milestones Breakdown

⚠️ **PREREQUISITES** (User responsibility - 0 days):

Before starting M3, verify the SEARCH application ALREADY EXISTS and is accessible:

**Required Information**:
- ✅ Application URL: `https://staging.example.com/search`
- ✅ Search functionality details
- ✅ Available filters
- ✅ Sorting options
- ✅ Expected behavior
- ✅ Test data

**Search Page Documentation Example**:
| Feature | Locator/Details | Expected Behavior |
|---------|-----------------|-------------------|
| Search input | id="searchQuery" | Accept any text, max 255 chars |
| Search button | css=".search-btn" | Submit search, show loading |
| Category filter | id="categoryFilter" | Dropdown, multi-select |
| Price range filter | class="price-slider" | Min/max inputs, apply button |
| Sort dropdown | id="sortBy" | Options: Price, Name, Relevance |
| Results container | css=".result-item" | List of result cards |
| Pagination | css=".pagination" | Page numbers, prev/next |
| No results message | css=".no-results" | "No results found for..." |

---

### M3: TAF - Search & Filter Tests (1-2 days)
**Deliverables**:
- SearchPage.java with all locators and actions
- Search tests (valid, invalid, empty, special characters)
- Filter tests (single filter, multiple filters, reset)
- Sorting tests (ascending, descending, verify order)
- Pagination tests
- Integration tests (search + filter + sort)

---

### M4: Level 2 Features & Documentation (1-2 days)
**Deliverables**:
- Data-Driven Testing với search queries và filter combinations
- ExtentReports
- Retry Mechanism (optional for flaky AJAX-heavy pages)
- Test cases documentation
- README

---

## Common Patterns

### Pattern 1: No Results Handling
```java
@Test
public void testNoResults() {
    searchPage.searchFor("xyznonexistentproduct123");

    Assert.assertEquals(searchPage.getResultCount(), 0);
    Assert.assertTrue(searchPage.isNoResultsMessageDisplayed());
    Assert.assertEquals(searchPage.getNoResultsMessage(),
        "No results found for your search");
}
```

### Pattern 2: Autocomplete/Suggestions
```java
@Test
public void testSearchSuggestions() {
    searchPage.typeInSearch("lap"); // Partial query

    // Wait for suggestions to appear
    waitHelper.waitForElementVisible(searchPage.getSuggestionsDropdown());

    List<String> suggestions = searchPage.getSuggestions();
    Assert.assertTrue(suggestions.contains("Laptop"));
    Assert.assertTrue(suggestions.contains("Laptop Bag"));
}
```

### Pattern 3: Filter Combinations
```java
@Test(dataProvider = "filterCombinations")
public void testFilterCombinations(String category, int minPrice, int maxPrice) {
    searchPage.selectCategory(category);
    searchPage.setPriceRange(minPrice, maxPrice);
    searchPage.clickApplyFilters();

    // Verify results
    Assert.assertTrue(searchPage.getResultCount() > 0 ||
                     searchPage.isNoResultsMessageDisplayed());
}
```

---

## Real-World Considerations for Template E

### 1. Dynamic Results Loading (AJAX)
**Challenge**: Search pages heavily rely on AJAX for loading results, filters, and pagination

**Solutions**:
```java
public void waitForSearchResultsToLoad() {
    // Wait for loading indicator to disappear
    waitHelper.waitForElementNotVisible(By.cssSelector(".loading-spinner"));

    // Wait for results container to be populated
    waitHelper.waitForElementsVisible(By.cssSelector(".result-item"));
}
```

### 2. Filter State Persistence
**Real-World Issue**: Filter state may persist via URL parameters, session storage, or cookies

**Solutions**:
```java
@BeforeMethod
public void resetSearchState() {
    // Navigate to clean search page
    driver.get(baseUrl + "/search");

    // Clear all filters explicitly
    if (searchPage.isClearFiltersButtonVisible()) {
        searchPage.clickClearFilters();
    }

    // Clear browser storage
    ((JavascriptExecutor) driver).executeScript("sessionStorage.clear();");
    ((JavascriptExecutor) driver).executeScript("localStorage.clear();");
}
```

### 3. Search Result Caching
**Challenge**: Search results may be cached (browser, CDN, application level)

**Test Consideration**: Use cache-busting parameters for testing

### 4. Test Data Strategy for Search
**Problem**: Real staging environment has unknown/unpredictable data

**Strategy A - Known Test Data**: Create known test data before search tests
**Strategy B - Unknown Data**: Verify behavior without asserting exact counts

### 5. Autocomplete/Suggestions Timing
**Challenge**: Search suggestions appear with variable delay (debouncing, network latency)

**Solution**: Type character by character and wait for suggestions dropdown

---

## Pitfalls & Solutions

### Pitfall 1: Dynamic Results Loading
**Problem**: Results load via AJAX, test runs before loading complete

**Solution**: Wait for loading spinner to disappear and results to appear

### Pitfall 2: Filter State Not Reset
**Problem**: Filters from previous test affect current test

**Solution**: Reset filters in @BeforeMethod

---

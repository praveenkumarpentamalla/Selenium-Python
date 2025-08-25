# **Data-Driven Testing with Pytest: Parameterization and Fixtures**

## **Table of Contents**
1. [Introduction to Data-Driven Testing](#1-introduction-to-data-driven-testing)
2. [Parameterization with Pytest](#2-parameterization-with-pytest)
3. [Creating a Data-Driven Fixture](#3-creating-a-data-driven-fixture)
4. [Handling Multiple Datasets](#4-handling-multiple-datasets)
5. [Best Practices](#5-best-practices)
6. [Summary](#6-summary)

---

## **1. Introduction to Data-Driven Testing**

Data-driven testing allows you to run the same test with multiple datasets. This is useful for:
- Testing different input combinations.
- Reducing code duplication.
- Improving test coverage.

### **Example Scenario:**
- Test a login form with multiple username/password combinations.
- Test a registration form with different user details.

---

## **2. Parameterization with Pytest**

Pytest provides the `@pytest.mark.parametrize` decorator to pass multiple datasets to a test.

### **Syntax:**
```python
import pytest

@pytest.mark.parametrize("input1, input2, expected", [
    (1, 2, 3),
    (4, 5, 9),
    (10, 20, 30)
])
def test_addition(input1, input2, expected):
    assert input1 + input2 == expected
```

### **Benefits:**
- Runs the test for each dataset.
- Clear and concise syntax.

---

## **3. Creating a Data-Driven Fixture**

### **Why Use Fixtures for Data-Driven Testing?**
- Fixtures can be reused across multiple tests.
- Better organization of test data.

### **Example Fixture:**
```python
import pytest

@pytest.fixture(params=[
    ("Rahul", "Sharma", "Male"),
    ("Sheena", "Thomas", "Female")
])
def get_data(request):
    return request.param
```

### **Explanation:**
- `params`: List of datasets. Each tuple is one dataset.
- `request.param`: Returns the current dataset.

---

## **4. Handling Multiple Datasets**

### **Using the Fixture in a Test:**
```python
def test_form_submission(self, get_data):
    first_name = get_data[0]
    last_name = get_data[1]
    gender = get_data[2]
    
    # Use the data in your test
    self.driver.find_element(By.NAME, "firstname").send_keys(first_name)
    self.driver.find_element(By.NAME, "lastname").send_keys(last_name)
    # ... and so on
```

### **Issue with Class-Level Setup:**
- If using `setUpClass` (executed once), the browser won't refresh between datasets.
- Solution: Refresh the page or navigate back to the form before each test.

### **Refreshing the Page:**
```python
def test_form_submission(self, get_data):
    self.driver.refresh()  # Refresh the page
    # Now enter data
```

---

## **5. Best Practices**

1. **Use Descriptive Data:**
   - Instead of indexes like `get_data[0]`, use dictionaries for clarity.
   - Example:
     ```python
     @pytest.fixture(params=[
         {"first_name": "Rahul", "last_name": "Sharma", "gender": "Male"},
         {"first_name": "Sheena", "last_name": "Thomas", "gender": "Female"}
     ])
     def get_data(request):
         return request.param
     ```

2. **Avoid Class-Level Setup for Data-Driven Tests:**
   - Use `setUp` (executed before each test) instead of `setUpClass`.

3. **Organize Data in External Files:**
   - Store test data in CSV, JSON, or Excel files for maintainability.

4. **Use Meaningful Names:**
   - Name fixtures and parameters clearly.

---

## **6. Summary**

- **Data-Driven Testing**: Run tests with multiple datasets using `@pytest.mark.parametrize` or fixtures.
- **Fixtures**: Use `params` in fixtures to pass datasets.
- **Page Refresh**: Refresh the page between tests if using class-level setup.
- **Best Practices**: Use dictionaries for readable data and avoid class-level setup for data-driven tests.

---

## **Next Steps**
- Practice parameterizing tests with different datasets.
- Explore external data sources (e.g., reading data from CSV files).
- Implement data-driven tests in your Selenium projects.

---

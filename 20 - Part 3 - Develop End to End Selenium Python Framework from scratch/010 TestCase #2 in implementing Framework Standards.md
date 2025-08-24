# **Selenium Automation Testing: Data-Driven Testing with Page Object Model**

## **Table of Contents**
1. [Introduction to Data-Driven Testing](#1-introduction-to-data-driven-testing)  
2. [Organizing Test Data from External Files](#2-organizing-test-data-from-external-files)  
3. [Creating a HomePage Test Class](#3-creating-a-homepage-test-class)  
4. [Implementing Page Object Model for HomePage](#4-implementing-page-object-model-for-homepage)  
5. [Data-Driven Approach with External Sources](#5-data-driven-approach-with-external-sources)  
6. [Best Practices and Summary](#6-best-practices-and-summary)  

---

## **1. Introduction to Data-Driven Testing**

Data-driven testing is a technique where test data is separated from the test scripts. This allows the same test to be run with multiple sets of data, improving reusability and maintainability. In this lecture, we focus on filling a form on the Rahul Shetty Academy practice website using data from external sources.

---

## **2. Organizing Test Data from External Files**

Instead of hardcoding data within test scripts, we store it in external files (e.g., CSV, Excel, JSON). This approach:
- Simplifies test maintenance.
- Allows running tests with different data sets without changing the code.
- Enhances scalability and reusability.

---

## **3. Creating a HomePage Test Class**

To organize tests logically, we create a dedicated class for homepage-related test cases.

### **Example: HomePage Test Class**
```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select

class TestHomePage:
    def test_form_submission(self):
        # Test code for form submission
        pass
```

### **Explanation**
- The class `TestHomePage` groups all homepage-related tests.
- The method `test_form_submission` will contain the logic for submitting the form.

---

## **4. Implementing Page Object Model for HomePage**

We use the Page Object Model (POM) to encapsulate homepage elements and actions.

### **Step 1: Create a HomePage Class**
Define a class `HomePage` with locators and methods for interacting with the form.

```python
class HomePage:
    def __init__(self, driver):
        self.driver = driver

    def get_name(self):
        return self.driver.find_element(By.NAME, "name")

    def get_email(self):
        return self.driver.find_element(By.NAME, "email")

    # Add more methods for other form fields
```

### **Step 2: Use HomePage in Test**
In the test class, initialize the `HomePage` object and use its methods.

```python
class TestHomePage:
    def test_form_submission(self):
        homepage = HomePage(self.driver)
        homepage.get_name().send_keys("John Doe")
        homepage.get_email().send_keys("john@example.com")
        # Continue with other fields and submission
```

### **Benefits of POM**
- Improves code readability and maintainability.
- Reduces code duplication.
- Makes tests more robust to UI changes.

---

## **5. Data-Driven Approach with External Sources**

### **Using Fixtures for Data Injection**
In pytest, use fixtures to load data from external files and pass it to tests.

#### **Example: Loading Data from a JSON File**
```python
import json

@pytest.fixture
def form_data():
    with open('test_data.json') as f:
        data = json.load(f)
    return data

def test_form_submission(self, form_data):
    homepage = HomePage(self.driver)
    homepage.get_name().send_keys(form_data['name'])
    homepage.get_email().send_keys(form_data['email'])
    # Use other data fields
```

### **Example test_data.json**
```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "gender": "Male",
    "dob": "1990-01-01",
    "employment_status": "Employed"
}
```

### **Advantages**
- Tests can be run with multiple data sets by simply updating the external file.
- Separation of data and logic makes tests easier to manage.

---

## **6. Best Practices and Summary**

### **Best Practices**
1. **Use Descriptive Names**: Name test classes and methods clearly (e.g., `TestHomePage`, `test_form_submission`).
2. **Centralize Locators**: Store all element locators in the page object class.
3. **Leverage Fixtures**: Use pytest fixtures to load and manage test data.
4. **Maintain Separation**: Keep test data, page objects, and test scripts in separate files/modules.
5. **Handle Dynamic Data**: Use parameterized tests for running the same test with different data sets.

### **Summary**
- Data-driven testing enhances test flexibility and maintainability.
- The Page Object Model pattern helps in organizing code and reducing duplication.
- External data sources (e.g., JSON files) allow tests to be easily configured with different inputs.
- By following these practices, you can build a scalable and robust test automation framework.

---

## **Next Steps**
- Complete the implementation for all form fields (e.g., password, gender, date of birth).
- Explore other data formats like CSV or Excel for storing test data.
- Implement parameterized tests in pytest to run the same test with multiple data sets.

---

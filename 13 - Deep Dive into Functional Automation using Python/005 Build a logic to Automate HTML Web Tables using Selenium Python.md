# **Selenium Python Mastery: A Comprehensive Guide**  
*Automating Web Testing with Practical Examples*  

---

## **Table of Contents**  
1. **Introduction to Web Automation with Selenium**  
2. **Locating Web Elements**  
   - Basic Locators (ID, Class, XPath, CSS)  
   - Advanced XPath Strategies  
3. **Handling Dynamic Elements**  
4. **Validating Web Page Data**  
   - Price Matching in Checkout  
   - Summing Values from Multiple Elements  
5. **Practical Assignments & Cheat Sheet**  

---

# **Chapter 1: Introduction to Web Automation**  
### **What is Selenium?**  
Selenium is an open-source framework for automating web browsers. It supports multiple programming languages (Python, Java, C#) and is widely used for testing web applications.  

### **Why Python with Selenium?**  
- Easy-to-read syntax  
- Rich ecosystem (PyTest, unittest)  
- Cross-platform compatibility  

---

# **Chapter 2: Locating Web Elements**  
### **Basic Locators**  
1. **By ID**  
   ```python
   driver.find_element(By.ID, "username")
   ```  
2. **By Class Name**  
   ```python
   driver.find_element(By.CLASS_NAME, "amount")
   ```  
3. **By XPath**  
   ```python
   driver.find_element(By.XPATH, "//div[@class='amount']")
   ```  

### **Advanced XPath Strategies**  
**Problem:**  
When multiple elements share the same class (e.g., `class="amount"`), a simple locator selects all matching elements.  

**Solution:**  
Use **parent-child traversal** to uniquely identify elements.  

**Example:**  
```python
# Grandparent → Parent → Child strategy
prices = driver.find_elements(By.XPATH, "//table//tr/td[5]/b[@class='amount']")
```  
**Explanation:**  
- Start from a unique parent (`<table>`).  
- Navigate to the specific column (`td[5]`).  
- Filter by `<b class="amount">`.  

---

# **Chapter 3: Handling Dynamic Elements**  
### **Dynamic Checkout Prices**  
**Scenario:**  
Verify if the sum of product prices matches the total amount displayed.  

**Steps:**  
1. **Extract Prices:**  
   ```python
   amounts = driver.find_elements(By.XPATH, "//table//tr/td[5]/b[@class='amount']")
   ```  
2. **Sum the Values:**  
   ```python
   total = 0
   for amount in amounts:
       price = int(amount.text)  # Convert text to integer
       total += price
   ```  
3. **Compare with Total:**  
   ```python
   displayed_total = int(driver.find_element(By.CLASS_NAME, "total").text)
   assert total == displayed_total, "Price mismatch!"
   ```  

---

# **Chapter 4: Validating Web Page Data**  
### **Real-World Example: Checkout Validation**  
**Objective:**  
Ensure the sum of individual product prices (`180 + 160 + 48`) matches the total (`388`).  

**Code Implementation:**  
```python
# Step 1: Grab all product prices
amounts = driver.find_elements(By.XPATH, "//table//tr/td[5]/b[@class='amount']")

# Step 2: Calculate sum
sum = 0
for amount in amounts:
    sum += int(amount.text)

# Step 3: Compare with displayed total
total_amount = int(driver.find_element(By.CLASS_NAME, "total").text)
assert sum == total_amount, f"Expected {total_amount}, but got {sum}"
```  

**Output:**  
```
Test Passed! Sum (388) matches the total amount.
```

---

# **Chapter 5: Practical Assignments**  
### **Assignment: Search Functionality Test**  
**Goal:**  
Verify that searching for a keyword (e.g., "Strawberry") displays the correct products.  

**Steps:**  
1. **Define Expected Products:**  
   ```python
   expected_products = ["Strawberry", "Raspberry", "Blueberry"]
   ```  
2. **Search and Fetch Results:**  
   ```python
   search_box = driver.find_element(By.ID, "search")
   search_box.send_keys("Berry")
   products = driver.find_elements(By.CLASS_NAME, "product-name")
   actual_products = [product.text for product in products]
   ```  
3. **Validate:**  
   ```python
   assert expected_products == actual_products, "Search results mismatch!"
   ```  

---

# **Bonus: Selenium Python Cheat Sheet**  
| **Action**               | **Code Example**                          |
|---------------------------|------------------------------------------|
| Open Browser              | `driver = webdriver.Chrome()`            |
| Find Element by XPath     | `driver.find_element(By.XPATH, "//div")` |
| Get Text                  | `element.text`                           |
| Click Button              | `button.click()`                         |
| Assert Equality           | `assert a == b, "Error message"`         |

---

# **Conclusion**  
This guide covered:  
✅ Locating elements with XPath  
✅ Summing dynamic values  
✅ Validating checkout totals  
✅ Practical assignments  

---  
**Feedback?** Let us know how we can improve this book!

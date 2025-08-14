# **Complete E-commerce Checkout Automation with Selenium Python**

## **Table of Contents**
1. **Product Selection Logic**
2. **Checkout Process Automation**
3. **Dynamic Dropdown Handling**
4. **Final Purchase Flow**
5. **Complete Code Implementation**
6. **Best Practices & Enhancements**

---

## **1. Product Selection Logic**
### **Locating and Selecting Products**
```python
# Find all product cards
products = driver.find_elements(By.XPATH, "//div[@class='card']")

# Iterate through products to find BlackBerry
for product in products:
    product_name = product.find_element(By.XPATH, ".//h4/a").text
    if product_name == "Blackberry":
        # Click "Add to Cart" button
        product.find_element(By.XPATH, ".//div[@class='card-footer']/button").click()
        break
```

**Key Features**:
- Uses relative XPath (`./`) to traverse from parent to child elements
- Checks product name text conditionally
- Clicks "Add to Cart" only for matching products

---

## **2. Checkout Process Automation**
### **Navigating to Cart**
```python
# Click cart button
driver.find_element(By.CSS_SELECTOR, "a.btn-primary").click()

# Proceed to checkout
driver.find_element(By.XPATH, "//button[text()='Checkout']").click()
```

### **Verification (Optional)**
```python
# Verify product in cart
cart_items = driver.find_elements(By.XPATH, "//tr[@class='ng-star-inserted']")
assert len(cart_items) == 1, "Expected 1 item in cart"
```

---

## **3. Dynamic Dropdown Handling**
### **Country Selection with Explicit Wait**
```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Enter country partial text
driver.find_element(By.ID, "country").send_keys("Ind")

# Wait for suggestions to appear
wait = WebDriverWait(driver, 10)
country_option = wait.until(EC.presence_of_element_located((By.LINK_TEXT, "India")))

# Select country
country_option.click()
```

**Why Explicit Wait?**
- Handles dynamic loading of suggestions
- More reliable than static `time.sleep()`
- Fails fast if element doesn't appear within timeout

---

## **4. Final Purchase Flow**
### **Terms Acceptance and Purchase**
```python
# Check terms checkbox
driver.find_element(By.XPATH, "//label[@for='checkbox2']").click()

# Complete purchase
driver.find_element(By.CSS_SELECTOR, "input[value='Purchase']").click()
```

### **Success Verification**
```python
success_text = driver.find_element(By.CSS_SELECTOR, "div.alert-success").text
assert "Success" in success_text
print("Order placed successfully!")
```

---

## **5. Complete Code Implementation**
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Initialize driver
driver = webdriver.Chrome()
driver.get("https://rahulshettyacademy.com/angularpractice/")

# 1. Navigate to shop
driver.find_element(By.CSS_SELECTOR, "a[href*='shop']").click()

# 2. Find and add BlackBerry to cart
products = driver.find_elements(By.XPATH, "//div[@class='card']")
for product in products:
    product_name = product.find_element(By.XPATH, ".//h4/a").text
    if product_name == "Blackberry":
        product.find_element(By.XPATH, ".//div[@class='card-footer']/button").click()
        break

# 3. Proceed to checkout
driver.find_element(By.CSS_SELECTOR, "a.btn-primary").click()
driver.find_element(By.XPATH, "//button[text()='Checkout']").click()

# 4. Enter country details
driver.find_element(By.ID, "country").send_keys("Ind")
wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.LINK_TEXT, "India")))
driver.find_element(By.LINK_TEXT, "India").click()

# 5. Complete purchase
driver.find_element(By.XPATH, "//label[@for='checkbox2']").click()
driver.find_element(By.CSS_SELECTOR, "input[value='Purchase']").click()

# 6. Verify success
success_text = driver.find_element(By.CSS_SELECTOR, "div.alert-success").text
assert "Success" in success_text

driver.quit()
```

---

## **6. Best Practices & Enhancements**
### **Improvement Opportunities**
1. **Page Object Model**: Separate classes for shop, cart, checkout pages
2. **Custom Waits**: Implement reusable wait methods
3. **Data-Driven Testing**: Externalize test data (products, countries)
4. **Logging**: Add detailed execution logs
5. **Screenshots**: Capture on failure/success

### **Pro Tips**
✔ Use relative locators for stable element identification  
✔ Implement explicit waits for dynamic content  
✔ Parameterize search terms and assertions  
✔ Add try-catch blocks for graceful failure handling  


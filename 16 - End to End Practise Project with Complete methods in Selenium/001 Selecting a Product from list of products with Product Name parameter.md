# **Complete End-to-End E-commerce Test Automation with Selenium Python**

## **Table of Contents**
1. **Test Scenario Overview**
2. **Locating and Selecting Products**
3. **Adding to Cart Logic**
4. **Checkout Process Automation**
5. **Complete Code Implementation**
6. **Best Practices & Enhancements**

---

## **1. Test Scenario Overview**
### **User Flow to Automate**
1. Navigate to e-commerce site
2. Click "Shop" link
3. Find and select "BlackBerry" product
4. Add to cart
5. Proceed to checkout
6. Enter country and accept terms
7. Verify success message

### **Technical Challenges**
- Dynamic product positioning
- Parent-child element traversal
- Conditional product selection
- Form handling and validation

---

## **2. Locating and Selecting Products**
### **Finding All Products**
```python
products = driver.find_elements(By.XPATH, "//div[@class='card']")
```

### **Product Selection Logic**
```python
for product in products:
    product_name = product.find_element(By.XPATH, ".//h4/a").text
    if product_name == "Blackberry":
        product.find_element(By.XPATH, ".//button[contains(text(),'Add')]").click()
        break
```

**Key Points**:
- Uses relative XPath (`./`) to traverse from parent to child
- Checks product name text conditionally
- Clicks "Add to Cart" only for matching products

---

## **3. Adding to Cart Logic**
### **Cart Interaction**
```python
# Click cart button
driver.find_element(By.CSS_SELECTOR, "a[class*='btn-primary']").click()

# Verify product in cart
cart_items = driver.find_elements(By.XPATH, "//tr[@class='ng-star-inserted']")
assert len(cart_items) == 1, "Expected 1 item in cart"
```

### **Checkout Initialization**
```python
driver.find_element(By.XPATH, "//button[text()='Checkout']").click()
```

---

## **4. Checkout Process Automation**
### **Country Selection**
```python
driver.find_element(By.ID, "country").send_keys("India")

# Wait for suggestions (using explicit wait)
wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.LINK_TEXT, "India")))

driver.find_element(By.LINK_TEXT, "India").click()
```

### **Terms Acceptance and Purchase**
```python
driver.find_element(By.XPATH, "//label[@for='checkbox2']").click()
driver.find_element(By.CSS_SELECTOR, "input[value='Purchase']").click()
```

### **Success Verification**
```python
success_text = driver.find_element(By.CSS_SELECTOR, "div.alert-success").text
assert "Success" in success_text
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
        product.find_element(By.XPATH, ".//button[contains(text(),'Add')]").click()
        break

# 3. Proceed to checkout
driver.find_element(By.CSS_SELECTOR, "a[class*='btn-primary']").click()
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
1. **Page Object Model**: Separate page classes for shop, cart, checkout
2. **Explicit Waits**: More robust element waiting
3. **Data-Driven**: Externalize test data (products, countries)
4. **Logging**: Add detailed execution logs
5. **Screenshots**: Capture on failure

### **Pro Tips**
✔ Use relative XPaths for stable element location  
✔ Implement custom wait conditions for dynamic content  
✔ Parameterize search terms and assertions  
✔ Add try-catch blocks for graceful failure handling  

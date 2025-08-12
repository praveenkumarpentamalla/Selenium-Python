# **Comprehensive Guide to Price Validation in Selenium Test Automation**

## **Table of Contents**
1. [Introduction to Price Validation](#1-introduction-to-price-validation)
2. [Extracting and Converting Price Values](#2-extracting-and-converting-price-values)
3. [Implementing Discount Verification](#3-implementing-discount-verification)
4. [Complete Test Case Example](#4-complete-test-case-example)
5. [Best Practices](#5-best-practices)
6. [Summary](#6-summary)

---

## **1. Introduction to Price Validation**

### **Testing Scenario**
- **Objective**: Verify price decreases after applying promo code
- **Challenge**: Need to compare numerical values extracted from web elements
- **Solution**: Convert string prices to numerical values for comparison

### **Key Concepts**
```python
# Example price extraction and conversion
price_text = "349.2"  # From web element
price_value = float(price_text)  # Convert to numerical value
```

---

## **2. Extracting and Converting Price Values**

### **Step 1: Locate Price Elements**
```python
# CSS selector for total amount
total_amount_locator = "span.totAmt"

# Get price before discount
original_amount = driver.find_element(By.CSS_SELECTOR, total_amount_locator).text
print("Original Amount:", original_amount)  # Output: "388"
```

### **Step 2: Convert String to Numerical Value**
```python
# Convert to float for decimal values
original_value = float(original_amount)

# Alternative for integer values
# original_value = int(float(original_amount)) 
```

### **Step 3: Apply Discount and Get New Price**
```python
# Apply promo code
driver.find_element(By.CLASS_NAME, "promoCode").send_keys("rahulshettyacademy")
driver.find_element(By.CSS_SELECTOR, ".promoBtn").click()

# Wait for discount to apply
wait = WebDriverWait(driver, 10)
wait.until(EC.text_to_be_present_in_element(
    (By.CLASS_NAME, "promoInfo"), 
    "Code applied successfully"
))

# Get discounted price
discounted_amount = driver.find_element(By.CSS_SELECTOR, total_amount_locator).text
discounted_value = float(discounted_amount)
print("Discounted Amount:", discounted_value)  # Output: "349.2"
```

---

## **3. Implementing Discount Verification**

### **Method 1: Simple Assertion**
```python
assert discounted_value < original_value, \
    f"Price didn't decrease ({discounted_value} >= {original_value})"
```

### **Method 2: Percentage Verification**
```python
discount_percentage = ((original_value - discounted_value) / original_value) * 100
assert discount_percentage > 0, "No discount applied"
print(f"Discount: {discount_percentage:.2f}%")  # Output: "Discount: 10.00%"
```

### **Method 3: Exact Discount Validation**
```python
expected_discount = 0.1  # 10%
expected_discounted = original_value * (1 - expected_discount)
assert abs(discounted_value - expected_discounted) < 0.01, \
    "Discount not applied correctly"
```

---

## **4. Complete Test Case Example**

```python
def test_discount_application():
    # Get original price
    original_amount = driver.find_element(
        By.CSS_SELECTOR, "span.totAmt"
    ).text
    original_value = float(original_amount)
    
    # Apply promo code
    driver.find_element(By.CLASS_NAME, "promoCode").send_keys("rahulshettyacademy")
    driver.find_element(By.CSS_SELECTOR, ".promoBtn").click()
    
    # Wait for discount
    wait = WebDriverWait(driver, 10)
    wait.until(EC.text_to_be_present_in_element(
        (By.CLASS_NAME, "promoInfo"), 
        "Code applied successfully"
    ))
    
    # Get discounted price
    discounted_amount = driver.find_element(
        By.CSS_SELECTOR, "span.totAmt"
    ).text
    discounted_value = float(discounted_amount)
    
    # Validate discount
    assert discounted_value < original_value, \
        f"Price didn't decrease ({discounted_value} >= {original_value})"
    
    # Bonus: Verify discount percentage
    discount_percentage = ((original_value - discounted_value) / original_value) * 100
    assert 9.9 < discount_percentage < 10.1, \
        f"Discount percentage {discount_percentage:.2f}% out of expected range"
```

---

## **5. Best Practices**

### **Price Validation Do's and Don'ts**
| **Best Practice** | **Example** | **Why?** |
|------------------|------------|----------|
| **Use float() for prices** | `float(element.text)` | Handles decimal values |
| **Add tolerance for calculations** | `abs(a - b) < 0.01` | Accounts for floating point precision |
| **Wait for price updates** | `WebDriverWait` | Ensures dynamic changes complete |
| **Clean price strings** | `text.strip('$')` | Removes currency symbols |

### **Common Pitfalls**
❌ **Direct string comparison**: `"349" < "388"` works but is unreliable  
❌ **Ignoring currency symbols**: `float("$349.00")` raises ValueError  
❌ **No wait for price updates**: Comparing before AJAX completes  

---

## **6. Summary**

### **Key Takeaways**
1. **Always convert prices** from strings to numerical types before comparison  
2. **Use floating-point** for accurate decimal calculations  
3. **Implement proper waits** for dynamic price changes  
4. **Multiple validation methods** exist depending on test requirements  


```python
def get_float_price(element):
    """Extract float value from price element"""
    return float(element.text.replace('$', '').strip())

# Usage:
price = get_float_price(driver.find_element(By.CSS_SELECTOR, ".price"))
```

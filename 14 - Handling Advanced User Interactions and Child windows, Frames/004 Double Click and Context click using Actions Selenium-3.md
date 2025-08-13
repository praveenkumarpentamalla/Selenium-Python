# **Mastering Advanced Selenium Interactions: Double-Click, Right-Click, and Element Visibility**  

## **Table of Contents**  
1. **Handling Double-Click Actions**  
2. **Working with Right-Click (Context Click)**  
3. **Validating Element Visibility with `is_displayed()`**  
4. **Practical Code Examples**  
5. **Best Practices & Common Pitfalls**  

---

## **1. Handling Double-Click Actions**  
### **Scenario**  
Automate a double-click action that triggers a JavaScript alert.  

### **Solution**  
Use Selenium's `ActionChains` to perform a double-click:  
```python
from selenium import webdriver  
from selenium.webdriver.common.action_chains import ActionChains  
from selenium.webdriver.common.by import By  

driver = webdriver.Chrome()  
driver.get("https://example.com/double-click")  

# Locate the element to double-click  
double_click_btn = driver.find_element(By.ID, "doubleClickBtn")  

# Perform double-click  
actions = ActionChains(driver)  
actions.double_click(double_click_btn).perform()  

# Handle the alert  
alert = driver.switch_to.alert  
print(alert.text)  # Output: "You double-clicked me!"  
alert.accept()  
```  

### **Key Points**  
✔ **`double_click()`** simulates two rapid clicks.  
✔ Always chain actions with **`perform()`** to execute them.  

---

## **2. Right-Click (Context Click) with ActionChains**  
### **Scenario**  
Trigger a right-click menu on an element.  

### **Solution**  
```python
right_click_element = driver.find_element(By.ID, "rightClickBtn")  
actions.context_click(right_click_element).perform()  
```  

### **How It Works**  
- **`context_click()`** emulates a right-click.  
- Useful for testing context menus (e.g., "Save As" in browsers).  

---

## **3. Validating Element Visibility with `is_displayed()`**  
### **Scenario**  
Verify if an element is visible after clicking a "Hide" button.  

### **Solution**  
```python
textbox = driver.find_element(By.ID, "displayed-text")  

# Check visibility before hiding  
assert textbox.is_displayed()  # Returns True  

# Click "Hide" button  
driver.find_element(By.ID, "hide-textbox").click()  

# Check visibility after hiding  
assert not textbox.is_displayed()  # Returns False  
```  

### **Critical Notes**  
⚠️ **`is_displayed()`** checks visibility but **not presence in DOM**.  
- If the element is removed from DOM, use **`try-except`** with `NoSuchElementException`.  

---

## **4. Practical Code Example (Combined Workflow)**  
```python
from selenium import webdriver  
from selenium.webdriver.common.action_chains import ActionChains  
from selenium.webdriver.common.by import By  

driver = webdriver.Chrome()  
driver.get("https://example.com/interactions")  

# 1. Double-click to trigger alert  
actions = ActionChains(driver)  
actions.double_click(driver.find_element(By.ID, "doubleClickBtn")).perform()  
driver.switch_to.alert.accept()  

# 2. Right-click to open context menu  
actions.context_click(driver.find_element(By.ID, "rightClickBtn")).perform()  

# 3. Validate visibility of an element  
element = driver.find_element(By.ID, "toggle-element")  
assert element.is_displayed()  
driver.find_element(By.ID, "toggle-button").click()  
assert not element.is_displayed()  

driver.quit()  
```  

---

## **5. Best Practices & Common Pitfalls**  
### **Best Practices**  
✔ **Chain actions logically**: Hover → Click → Double-Click.  
✔ **Add delays** for dynamic elements:  
   ```python
   from time import sleep  
   actions.move_to_element(menu).pause(1).click().perform()  
   ```  

### **Common Errors**  
| Error                        | Solution                                  |  
|------------------------------|-------------------------------------------|  
| `ElementNotInteractable`     | Ensure element is visible *before* interaction. |  
| `NoSuchElementException`     | Use explicit waits (`WebDriverWait`).     |  

---

## **Cheat Sheet**  
| **Action**               | **Code**                                  |  
|--------------------------|-------------------------------------------|  
| Double-Click            | `actions.double_click(element).perform()` |  
| Right-Click             | `actions.context_click(element).perform()`|  
| Check Visibility        | `element.is_displayed()`                 |  

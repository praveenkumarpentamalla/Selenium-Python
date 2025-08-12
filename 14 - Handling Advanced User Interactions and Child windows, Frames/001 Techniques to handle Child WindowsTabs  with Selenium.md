# **Handling Multiple Windows in Selenium Python**  
*A Step-by-Step Guide with Practical Examples*  

---

## **Table of Contents**  
1. **Introduction to Window Handling**  
2. **Understanding Parent and Child Windows**  
3. **Switching Between Windows**  
4. **Practical Implementation**  
5. **Closing Child Windows**  
6. **Summary & Best Practices**  

---

## **1. Introduction to Window Handling**  
### **Why Handle Multiple Windows?**  
In web automation, clicking a link/button may open a **new window/tab**. By default, Selenium’s `driver` object controls only the **parent window** and cannot interact with child windows unless explicitly switched.  

### **Key Concepts**  
- **Parent Window**: The initial browser window opened by Selenium.  
- **Child Window**: A new window/tab opened via user action (e.g., clicking a link).  
- **Window Handles**: Unique identifiers for all open windows.  

---

## **2. Understanding Parent and Child Windows**  
### **Example Scenario**  
- **Website**: [The Internet Herokuapp](https://the-internet.herokuapp.com/windows)  
- **Action**: Clicking "Click Here" opens a child window.  

### **Default Behavior**  
```python
driver = webdriver.Chrome()
driver.get("https://the-internet.herokuapp.com/windows")
driver.find_element(By.LINK_TEXT, "Click Here").click()
```  
- The `driver` still points to the **parent window** and cannot access the child window.  

---

## **3. Switching Between Windows**  
### **Step 1: Get All Window Handles**  
```python
all_windows = driver.window_handles  # Returns a list of window IDs
parent_window = all_windows[0]       # First window is the parent
child_window = all_windows[1]        # Second window is the child
```  

### **Step 2: Switch to Child Window**  
```python
driver.switch_to.window(child_window)
print(driver.find_element(By.TAG_NAME, "h3").text)  # Output: "New Window"
```  

### **Step 3: Switch Back to Parent Window**  
```python
driver.switch_to.window(parent_window)
print(driver.find_element(By.TAG_NAME, "h3").text)  # Output: "Opening a new window"
```  

---

## **4. Practical Implementation**  
### **Full Code Example**  
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

# Initialize driver and open parent window
driver = webdriver.Chrome()
driver.get("https://the-internet.herokuapp.com/windows")

# Click link to open child window
driver.find_element(By.LINK_TEXT, "Click Here").click()

# Get window handles
all_windows = driver.window_handles
parent_window = all_windows[0]
child_window = all_windows[1]

# Switch to child window and print text
driver.switch_to.window(child_window)
assert "New Window" in driver.find_element(By.TAG_NAME, "h3").text

# Switch back to parent window and validate
driver.switch_to.window(parent_window)
assert "Opening a new window" in driver.find_element(By.TAG_NAME, "h3").text

# Close child window
driver.switch_to.window(child_window)
driver.close()

# Ensure focus is back on parent window
driver.switch_to.window(parent_window)
driver.quit()
```  

---

## **5. Closing Child Windows**  
### **Method 1: Close Specific Window**  
```python
driver.switch_to.window(child_window)
driver.close()  # Closes only the child window
```  

### **Method 2: Close All Windows**  
```python
driver.quit()  # Terminates the entire session (all windows)
```  

---

## **6. Summary & Best Practices**  
### **Key Takeaways**  
1. **`window_handles`** returns a list of all open windows.  
2. **`switch_to.window()`** shifts focus to a specific window.  
3. Always **switch back to the parent window** after child window operations.  

### **Best Practices**  
✔ **Use assertions** to validate window titles/text.  
✔ **Close child windows** after use to avoid memory leaks.  
✔ **Avoid hardcoding indices** (e.g., `all_windows[1]`) if multiple child windows exist.  

---

## **Assignment**  
**Task**: Automate a scenario where:  
1. Open [Multi-Window Example](https://the-internet.herokuapp.com/windows).  
2. Click "Click Here" to open a child window.  
3. Extract text from the child window.  
4. Verify the parent window title.  


---

## **Cheat Sheet**  
| **Action**               | **Code**                               |
|--------------------------|----------------------------------------|
| Get all windows          | `driver.window_handles`                |
| Switch to child window   | `driver.switch_to.window(child_id)`    |
| Close child window       | `driver.close()`                       |
| Return to parent window  | `driver.switch_to.window(parent_id)`   |

**Next**: Learn how to handle **frames** and **alerts** in Selenium! 🚀

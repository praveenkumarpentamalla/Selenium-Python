# **Selenium with Python: Navigation and Browser Controls**  
**Author: Your Name**  

## **Table of Contents**  
1. [Browser Navigation Methods](#browser-navigation-methods)  
2. [Window Management](#window-management)  
3. [Debugging Selenium Tests](#debugging-selenium-tests)  
4. [Running Tests in Different Browsers](#running-tests-in-different-browsers)  
5. [Conclusion](#conclusion)  

---

## **1. Browser Navigation Methods**  
Selenium provides methods to navigate between web pages, similar to using browser buttons.  

### **Key Navigation Methods:**  

| Method | Description | Example |  
|--------|------------|---------|  
| `driver.get(url)` | Opens a specified URL | `driver.get("https://www.example.com")` |  
| `driver.back()` | Navigates to the previous page (like the browser’s back button) | `driver.back()` |  
| `driver.forward()` | Navigates forward (like the browser’s forward button) | `driver.forward()` |  
| `driver.refresh()` | Reloads the current page | `driver.refresh()` |  

### **Example: Navigating Between Pages**  
```python
from selenium import webdriver  

driver = webdriver.Chrome(executable_path="C:\\chromedriver.exe")  

# Open first website  
driver.get("https://www.rahulshettyacademy.com")  

# Navigate to a practice site  
driver.get("https://www.rahulshettyacademy.com/practice")  

# Go back to the previous page  
driver.back()  

# Refresh the page  
driver.refresh()  

driver.quit()  
```

---

## **2. Window Management**  
Selenium allows controlling browser window size (maximize, minimize, fullscreen).  

### **Window Control Methods:**  
| Method | Description | Example |  
|--------|------------|---------|  
| `driver.maximize_window()` | Maximizes the browser window | `driver.maximize_window()` |  
| `driver.minimize_window()` | Minimizes the browser window | `driver.minimize_window()` |  
| `driver.fullscreen_window()` | Makes the browser fullscreen | `driver.fullscreen_window()` |  

### **Example: Maximizing and Minimizing Windows**  
```python
from selenium import webdriver  

driver = webdriver.Chrome(executable_path="C:\\chromedriver.exe")  

driver.get("https://www.rahulshettyacademy.com")  

# Maximize the window  
driver.maximize_window()  

# Minimize the window  
driver.minimize_window()  

driver.quit()  
```

---

## **3. Debugging Selenium Tests**  
Debugging helps identify issues in test execution.  

### **How to Debug in PyCharm/VSCode?**  
1. **Set a Breakpoint** (Click on the left gutter near the line number).  
2. **Run in Debug Mode** (Click the "Debug" button instead of "Run").  
3. **Step Through Code** (Use "Step Over" to execute line by line).  

### **Example: Debugging a Navigation Test**  
```python
from selenium import webdriver  

driver = webdriver.Chrome(executable_path="C:\\chromedriver.exe")  

driver.get("https://www.rahulshettyacademy.com")  
# Set breakpoint here  
driver.get("https://www.rahulshettyacademy.com/practice")  

driver.back()  # Debugger stops here  
driver.refresh()  

driver.quit()  
```

---

## **4. Running Tests in Different Browsers**  
Selenium supports multiple browsers (Chrome, Firefox, Edge, etc.).  

### **Example: Running Tests in Firefox & Edge**  
```python
# Firefox  
from selenium import webdriver  

driver = webdriver.Firefox(executable_path="C:\\geckodriver.exe")  
driver.get("https://www.rahulshettyacademy.com")  
driver.quit()  

# Edge  
driver = webdriver.Edge(executable_path="C:\\msedgedriver.exe")  
driver.get("https://www.rahulshettyacademy.com")  
driver.quit()  
```

---

## **5. Conclusion**  
- **Navigation:** Use `get()`, `back()`, `forward()`, `refresh()`.  
- **Window Control:** `maximize_window()`, `minimize_window()`.  
- **Debugging:** Use breakpoints and step execution.  
- **Cross-Browser Testing:** Works with Chrome, Firefox, Edge.  

### **Next Steps:**  
- Learn **locating elements** (`find_element_by_id`, `find_element_by_xpath`).  
- Explore **handling alerts, dropdowns, and frames**.  

---

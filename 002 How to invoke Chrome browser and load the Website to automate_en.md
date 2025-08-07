# **Selenium with Python: A Comprehensive Guide**  


## **Table of Contents**  
1. [Introduction to Selenium](#introduction-to-selenium)  
2. [Setting Up Selenium with Python](#setting-up-selenium-with-python)  
3. [Invoking Browsers in Selenium](#invoking-browsers-in-selenium)  
4. [Basic Selenium Operations](#basic-selenium-operations)  
5. [Closing the Browser](#closing-the-browser)  
6. [Handling Multiple Windows](#handling-multiple-windows)  
7. [Conclusion](#conclusion)  

---

## **1. Introduction to Selenium**  
Selenium is an open-source automation tool used for testing web applications. It supports multiple programming languages, including Python, Java, and C#.  

### **Key Features:**  
- Cross-browser compatibility (Chrome, Firefox, Edge, etc.)  
- Supports multiple operating systems  
- Automates form filling, clicking buttons, navigation, and more  

---

## **2. Setting Up Selenium with Python**  
### **Prerequisites:**  
- Python installed  
- Selenium library installed (`pip install selenium`)  
- WebDriver for the browser (ChromeDriver, GeckoDriver, etc.)  

### **Installing Selenium:**  
```bash
pip install selenium
```

### **Downloading WebDriver**  
Each browser requires a specific WebDriver executable:  
| Browser | WebDriver Download Link |  
|---------|------------------------|  
| Chrome | [ChromeDriver](https://sites.google.com/chromium.org/driver/) |  
| Firefox | [GeckoDriver](https://github.com/mozilla/geckodriver) |  
| Edge | [EdgeDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/) |  

**Steps to Set Up ChromeDriver:**  
1. Download the correct version matching your Chrome browser.  
2. Extract the `.exe` (Windows) or binary (Mac/Linux).  
3. Place it in a known directory (e.g., `C:\chromedriver.exe`).  

---

## **3. Invoking Browsers in Selenium**  
### **Opening Chrome Browser**  
```python
from selenium import webdriver  

# Initialize Chrome WebDriver  
driver = webdriver.Chrome(executable_path="C:\\chromedriver.exe")  

# Open a website  
driver.get("https://www.rahulshettyacademy.com")  
```

### **Explanation:**  
- `webdriver.Chrome()` initializes the Chrome browser.  
- `executable_path` points to the ChromeDriver location.  
- `driver.get(url)` opens the specified URL.  

### **Opening Firefox & Edge**  
```python
# Firefox  
driver = webdriver.Firefox(executable_path="C:\\geckodriver.exe")  

# Edge  
driver = webdriver.Edge(executable_path="C:\\msedgedriver.exe")  
```

---

## **4. Basic Selenium Operations**  
### **Getting the Page Title**  
```python
print(driver.title)  # Output: "Rahul Shetty Academy | A World Leader in Software Testing"
```

### **Getting the Current URL**  
```python
print(driver.current_url)  # Output: "https://www.rahulshettyacademy.com/"
```

### **Why Check URL & Title?**  
- Ensures the correct page loaded (prevents phishing/hacking).  
- Validates navigation success.  

---

## **5. Closing the Browser**  
### **Methods to Close the Browser:**  
| Method | Description |  
|--------|------------|  
| `driver.close()` | Closes the current browser window |  
| `driver.quit()` | Closes all browser windows and ends the WebDriver session |  

**Example:**  
```python
driver.close()  # Closes only the active tab  
driver.quit()   # Shuts down the entire browser  
```

### **When to Use Which?**  
- Use `close()` when working with a single window.  
- Use `quit()` when multiple windows are open.  

---

## **6. Handling Multiple Windows**  
If a link opens a new tab/window:  
```python
# Click a link that opens a new window  
driver.find_element_by_link_text("Open New Window").click()  

# Switch to the new window  
driver.switch_to.window(driver.window_handles[1])  

# Close only the new window  
driver.close()  

# Switch back to the original window  
driver.switch_to.window(driver.window_handles[0])  
```

---

## **7. Conclusion**  
- Selenium automates browser actions using WebDriver.  
- Always use the correct WebDriver version for your browser.  
- `get()`, `title`, `current_url`, `close()`, and `quit()` are essential methods.  

### **Next Steps:**  
- Learn locating elements (`find_element_by_id`, `find_element_by_xpath`).  
- Explore advanced interactions (dropdowns, alerts, frames).  

---

### **Appendix: Sample Code**  
```python
from selenium import webdriver  

# Initialize Chrome  
driver = webdriver.Chrome(executable_path="C:\\chromedriver.exe")  

# Open a website  
driver.get("https://www.rahulshettyacademy.com")  

# Print title and URL  
print("Title:", driver.title)  
print("URL:", driver.current_url)  

# Close the browser  
driver.quit()  
```




This book provides a structured approach to learning Selenium with Python. Happy automating! 🚀

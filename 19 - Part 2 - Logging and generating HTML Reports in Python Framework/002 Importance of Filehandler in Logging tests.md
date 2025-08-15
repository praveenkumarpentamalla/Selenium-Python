# **Hands-On Guide to Implementing Logging in Python**  

## **Table of Contents**  
1. [Setting Up Basic Logging](#setting-up-basic-logging)  
2. [Creating a Logger Object](#creating-a-logger-object)  
3. [Logging Levels in Action](#logging-levels-in-action)  
4. [Configuring File Handlers](#configuring-file-handlers)  
5. [Setting Log Message Formats](#setting-log-message-formats)  
6. [Complete Logging Example](#complete-logging-example)  

---

## **1. Setting Up Basic Logging**  
**Objective:** Learn how to initialize logging in Python.  

### **Step 1: Import the Logging Module**  
Python’s `logging` module is built-in—no external installation needed.  
```python
import logging
```

### **Step 2: Create a Logger Object**  
Use `logging.getLogger()` to create a logger.  
- `__name__` captures the current module’s name (useful for tracking logs).  
```python
logger = logging.getLogger(__name__)
```

---

## **2. Logging Levels in Action**  
Python supports five log levels:  

| Level      | Usage Example                          |  
|------------|---------------------------------------|  
| **DEBUG**  | Detailed technical logs (`logger.debug("Fetching data")`) |  
| **INFO**   | General execution flow (`logger.info("Test started")`) |  
| **WARNING**| Potential issues (`logger.warning("Slow response")`) |  
| **ERROR**  | Test failures (`logger.error("Login failed")`) |  
| **CRITICAL**| Severe errors (`logger.critical("DB crash")`) |  

### **Example: Logging Messages**  
```python
logger.debug("Debug message - For developers")  
logger.info("Info message - Test step passed")  
logger.warning("Warning - Low balance detected")  
logger.error("Error - Payment failed")  
logger.critical("Critical - System crash")  
```
**Output:** *(Only `WARNING` and above display by default)*  
```
WARNING: Warning - Low balance detected  
ERROR: Error - Payment failed  
CRITICAL: Critical - System crash  
```

---

## **3. Configuring File Handlers**  
**Problem:** Logs should be saved in a file, not just the console.  

### **Step 1: Create a File Handler**  
```python
file_handler = logging.FileHandler("test_execution.log")  
logger.addHandler(file_handler)  
```
Now, logs will be saved in `test_execution.log`.  

### **Step 2: Set Log Levels for File vs. Console**  
```python
# Log DEBUG and above to file  
file_handler.setLevel(logging.DEBUG)  

# Log only WARNING+ to console  
console_handler = logging.StreamHandler()  
console_handler.setLevel(logging.WARNING)  
logger.addHandler(console_handler)  
```

---

## **4. Setting Log Message Formats**  
**Goal:** Include timestamps, log levels, and module names.  

### **Step 1: Define a Format**  
```python
formatter = logging.Formatter(  
    "%(asctime)s - %(levelname)s - %(name)s - %(message)s",  
    datefmt="%Y-%m-%d %H:%M:%S"  
)  

file_handler.setFormatter(formatter)  
console_handler.setFormatter(formatter)  
```
**Output in `test_execution.log`:**  
```
2023-10-05 14:30:45 - INFO - __main__ - Test started  
2023-10-05 14:30:46 - ERROR - __main__ - Payment failed  
```

### **Key Format Variables**  
| Variable       | Description                |  
|----------------|----------------------------|  
| `%(asctime)s`  | Timestamp                 |  
| `%(levelname)s`| Log level (DEBUG/INFO/etc.)|  
| `%(name)s`     | Module name               |  
| `%(message)s`  | Log message               |  

---

## **5. Complete Logging Example**  
### **Final Script**  
```python
import logging  

# 1. Create Logger  
logger = logging.getLogger(__name__)  
logger.setLevel(logging.DEBUG)  # Capture all levels  

# 2. Create Handlers  
file_handler = logging.FileHandler("test.log")  
console_handler = logging.StreamHandler()  

# 3. Set Levels  
file_handler.setLevel(logging.DEBUG)  
console_handler.setLevel(logging.WARNING)  

# 4. Define Format  
formatter = logging.Formatter(  
    "%(asctime)s - %(levelname)s - %(name)s - %(message)s",  
    datefmt="%Y-%m-%d %H:%M:%S"  
)  
file_handler.setFormatter(formatter)  
console_handler.setFormatter(formatter)  

# 5. Add Handlers  
logger.addHandler(file_handler)  
logger.addHandler(console_handler)  

# Example Logs  
logger.info("Test case started")  
logger.warning("Slow network detected")  
logger.error("Login failed: Invalid credentials")  
```

### **Outputs**  
**Console:** *(Only WARNING+)*  
```
2023-10-05 14:35:00 - WARNING - __main__ - Slow network detected  
2023-10-05 14:35:01 - ERROR - __main__ - Login failed  
```

**File (`test.log`):** *(All levels)*  
```
2023-10-05 14:35:00 - INFO - __main__ - Test case started  
2023-10-05 14:35:00 - WARNING - __main__ - Slow network detected  
2023-10-05 14:35:01 - ERROR - __main__ - Login failed  
```

---

## **Key Takeaways**  
✔ **Use `getLogger(__name__)`** for module-aware logging.  
✔ **FileHandler** saves logs to a file; **StreamHandler** prints to console.  
✔ **Formatters** structure logs with timestamps, levels, and context.  
✔ **Set different log levels** for files (`DEBUG`) vs. console (`WARNING`).  

---

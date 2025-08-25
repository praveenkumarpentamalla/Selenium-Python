# **Implementing Logging in Selenium Framework with Pytest**

## **Table of Contents**
1. [Introduction to Logging in Automation](#1-introduction-to-logging-in-automation)
2. [Integrating Logging Utility into BaseClass](#2-integrating-logging-utility-into-baseclass)
3. [Using Logging in Test Cases](#3-using-logging-in-test-cases)
4. [Viewing and Analyzing Logs](#4-viewing-and-analyzing-logs)
5. [Best Practices for Logging](#5-best-practices-for-logging)
6. [Summary](#6-summary)

---

## **1. Introduction to Logging in Automation**

Logging is crucial for:
- Tracking test execution steps.
- Debugging failures by reviewing logs.
- Generating reports for stakeholders.

### **Log Levels:**
- `DEBUG`: Detailed information for debugging.
- `INFO`: General information about test execution.
- `WARNING`: Indicates potential issues.
- `ERROR`: Logs errors that occurred but didn’t halt the test.
- `CRITICAL`: Severe errors that lead to test termination.

---

## **2. Integrating Logging Utility into BaseClass**

### **Step 1: Add Logging Utility to BaseClass**
```python
import inspect
import logging

class BaseClass:
    def get_logger(self):
        # Get the name of the class/method that called this
        logger_name = inspect.stack()[1][3]
        logger = logging.getLogger(logger_name)
        
        # Set log level to DEBUG to capture all messages
        logger.setLevel(logging.DEBUG)
        
        # Create file handler
        file_handler = logging.FileHandler("logfile.log")
        
        # Create formatter
        formatter = logging.Formatter("%(asctime)s : %(levelname)s : %(name)s : %(message)s")
        file_handler.setFormatter(formatter)
        
        # Add handler to logger
        logger.addHandler(file_handler)
        
        return logger
```

### **Explanation:**
- `inspect.stack()`: Dynamically gets the name of the test method.
- `FileHandler`: Writes logs to `logfile.log`.
- `Formatter`: Structures log messages with timestamp, level, name, and message.

---

## **3. Using Logging in Test Cases**

### **Step 1: Inherit BaseClass in Test Class**
```python
import pytest
from utilities.BaseClass import BaseClass

class TestExample(BaseClass):
    def test_form_submission(self, get_data):
        # Initialize logger
        log = self.get_logger()
        
        log.info("Getting all card details")
        # Test steps here...
        
        log.info("Country name is India")
        log.info("Text received from application: Success")
```

### **Step 2: Log Key Actions**
```python
def test_homepage(self):
    log = self.get_logger()
    log.info("First name is Rahul")
    log.debug("Debug message: Entering details")
    log.error("Error message: Element not found")
```

### **Benefits:**
- Clear audit trail of test execution.
- Easy debugging when tests fail.

---

## **4. Viewing and Analyzing Logs**

### **Log File Output:**
```
2023-10-19 10:45:30 : INFO : test_form_submission : Getting all card details
2023-10-19 10:45:32 : INFO : test_form_submission : Country name is India
2023-10-19 10:45:33 : INFO : test_form_submission : Text received from application: Success
```

### **Tips:**
- Use `grep` or text editors to filter logs.
- Integrate with tools like Splunk or ELK for advanced analysis.

---

## **5. Best Practices for Logging**

1. **Use Appropriate Log Levels**:
   - `INFO` for general steps.
   - `DEBUG` for detailed debugging.
   - `ERROR` for failures.

2. **Avoid Over-Logging**:
   - Log only meaningful steps.
   - Exclude redundant information.

3. **Customize Log Paths**:
   - Store logs in a dedicated `logs/` directory.
   - Use dynamic file names (e.g., `testname_timestamp.log`).

4. **Integrate with Reporting**:
   - Use logs to generate HTML reports.
   - Combine with screenshots for failure analysis.

---

## **6. Summary**

- **Logging Utility**: Centralized in `BaseClass` for reuse.
- **Test Cases**: Call `self.get_logger()` to log steps.
- **Log Analysis**: Review `logfile.log` for execution details.
- **Best Practices**: Use appropriate levels and avoid clutter.

---

## **Next Steps**
- Enhance the logger to support dynamic log file names.
- Integrate with CI/CD pipelines for automated log analysis.
- Explore structured logging with JSON formats.

---

**Note**: Logging is a powerful tool for maintaining and debugging test suites. Implement it consistently across your framework!

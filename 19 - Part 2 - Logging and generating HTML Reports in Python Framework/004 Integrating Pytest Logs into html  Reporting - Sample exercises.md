# **Implementing Reusable Logging in Test Automation Frameworks**

## **Comprehensive Guide to Logging Architecture**

### **Table of Contents**
1. [Creating a Base Logging Class](#creating-a-base-logging-class)
2. [Implementing Inheritance for Logging](#implementing-inheritance-for-logging)
3. [Dynamic Test Name Capture](#dynamic-test-name-capture)
4. [Integrating Logs with Test Reports](#integrating-logs-with-test-reports)
5. [Complete Implementation Example](#complete-implementation-example)

---

## **1. Creating a Base Logging Class**
**Objective:** Centralize logging configuration for reuse across all tests.

### **BaseLogger Class Implementation**
```python
import logging
import inspect

class BaseLogger:
    def get_logger(self):
        # 1. Get calling test name dynamically
        caller_name = inspect.stack()[1][3]
        
        # 2. Create logger with test name
        logger = logging.getLogger(caller_name)
        logger.setLevel(logging.DEBUG)
        
        # 3. Configure file handler
        file_handler = logging.FileHandler("test_execution.log")
        file_handler.setLevel(logging.DEBUG)
        
        # 4. Set log format
        formatter = logging.Formatter(
            "%(asctime)s - %(levelname)s - %(name)s - %(message)s",
            datefmt="%Y-%m-%d %H:%M:%S"
        )
        file_handler.setFormatter(formatter)
        
        # 5. Add handler
        logger.addHandler(file_handler)
        
        return logger
```

**Key Features:**
- Dynamic test name capture using `inspect.stack()`
- Configurable log levels
- Structured format with timestamps
- Single log file for all test executions

---

## **2. Implementing Inheritance for Logging**
**Pattern:** Inherit `BaseLogger` in test classes for instant logging capability.

### **Test Class Example**
```python
class TestUserProfile(BaseLogger):  # Inherits logging functionality
    def test_edit_profile(self):
        logger = self.get_logger()  # Access parent class method
        
        logger.info("Navigating to profile page")
        # Test steps...
        logger.debug("Entering user details")
        
        try:
            # Test action
            logger.info("Profile updated successfully")
        except Exception as e:
            logger.error(f"Profile update failed: {str(e)}")
            raise
```

**Benefits:**
✅ Single source of truth for logging configuration  
✅ No code duplication across test classes  
✅ Consistent log format enterprise-wide  

---

## **3. Dynamic Test Name Capture**
**Solution:** Use Python's `inspect` module to identify calling test.

### **How It Works**
```python
import inspect

# Returns the name of the calling function/method
caller_name = inspect.stack()[1][3]  # Gets 'test_edit_profile' in our example
```

**Output in Logs:**
```
2023-10-05 14:30:45 - INFO - test_edit_profile - Navigating to profile page
```

**Why It Matters:**  
- Eliminates manual test name maintenance  
- Accurate traceability in logs  
- Works with any test naming convention  

---

## **4. Integrating Logs with Test Reports**
**pytest Integration:** Logs automatically appear in HTML reports when using `logger` objects.

### **Comparison: print vs logging**
| Approach | Console Output | Log File | HTML Report |
|----------|----------------|----------|-------------|
| `print()` | ✅ Yes         | ❌ No    | ❌ No       |
| `logging` | ✅ Yes         | ✅ Yes   | ✅ Yes      |

**Example Report Output:**
```html
<!-- In pytest-html report -->
<div class="log">
  <div class="log-message">INFO - test_edit_profile: Profile updated successfully</div>
</div>
```

**Pro Tip:** Combine with `pytest.ini` for enhanced logging:
```ini
[pytest]
log_cli = true
log_cli_level = INFO
```

---

## **5. Complete Implementation Example**
### **File Structure**
```
framework/
├── utilities/
│   └── logger.py  # BaseLogger class
└── tests/
    └── test_profile.py  # Test classes
```

### **logger.py**
```python
import logging
import inspect

class BaseLogger:
    def get_logger(self):
        caller_name = inspect.stack()[1][3]
        logger = logging.getLogger(caller_name)
        
        if not logger.handlers:  # Prevent duplicate handlers
            logger.setLevel(logging.DEBUG)
            formatter = logging.Formatter(
                "%(asctime)s - %(levelname)s - %(name)s - %(message)s",
                datefmt="%Y-%m-%d %H:%M:%S"
            )
            
            # File handler
            fh = logging.FileHandler("automation.log")
            fh.setFormatter(formatter)
            logger.addHandler(fh)
            
            # Console handler
            ch = logging.StreamHandler()
            ch.setLevel(logging.INFO)
            ch.setFormatter(formatter)
            logger.addHandler(ch)
            
        return logger
```

### **test_profile.py**
```python
from utilities.logger import BaseLogger

class TestUserProfile(BaseLogger):
    def test_password_change(self):
        log = self.get_logger()
        
        log.info("Starting password change test")
        try:
            log.debug("Entering current password")
            # Test steps...
            log.info("Password changed successfully")
        except Exception as e:
            log.error(f"Test failed: {str(e)}")
            raise
```

**Execution & Output:**
1. **Console:**
   ```
   2023-10-05 14:35:00 - INFO - test_password_change - Starting password change test
   2023-10-05 14:35:01 - INFO - test_password_change - Password changed successfully
   ```

2. **automation.log:**
   ```
   2023-10-05 14:35:00 - DEBUG - test_password_change - Entering current password
   2023-10-05 14:35:01 - INFO - test_password_change - Password changed successfully
   ```

3. **HTML Report:** Includes all log messages

---

## **Key Takeaways**
✔ **Centralized logging** through base class inheritance  
✔ **Dynamic test identification** with `inspect` module  
✔ **Dual output** to both files and console  
✔ **Seamless integration** with pytest reporting  
✔ **Configurable log levels** for different environments  


# **Introduction to pytest: Python Testing Framework**

## **Table of Contents**
1. [What is pytest?](#1-what-is-pytest)
2. [Installation & Setup](#2-installation--setup)
3. [Basic Test Structure](#3-basic-test-structure)
4. [Running Tests](#4-running-tests)
5. [Key pytest Features](#5-key-pytest-features)
6. [Next Steps](#6-next-steps)

---

## **1. What is pytest?**
pytest is a **testing framework** for Python that:
✔ Simplifies test writing with minimal boilerplate  
✔ Provides **detailed failure reports**  
✔ Supports **fixtures, parameterization, and plugins**  
✔ Integrates with **Selenium, Django, Flask**, etc.  

**Comparison with Java Frameworks:**
| Framework | Language | Key Use Case |
|-----------|----------|-------------|
| pytest    | Python   | Unit/Functional Testing |
| JUnit     | Java     | Unit Testing |
| TestNG    | Java     | Functional/Integration Testing |

---

## **2. Installation & Setup**
### **Install pytest**
```bash
pip install pytest
```

### **Verify Installation**
```bash
pytest --version
# Output: pytest 7.x.x
```

---

## **3. Basic Test Structure**
### **File Naming Convention**
- Files must start/end with `test_` (e.g., `test_demo.py`).  
- Example:
  ```
  /project
  └── tests/
      ├── test_login.py
      └── test_checkout.py
  ```

### **Test Function Rules**
1. Functions must start with `test_`.  
2. Code must be wrapped in functions.  

**Example:**
```python
# test_demo.py
def test_print_hello():
    print("Hello")  # Passes if no error occurs
```

---

## **4. Running Tests**
### **Method 1: PyCharm Test Runner**
1. Go to **Run → Edit Configurations**.  
2. Add a **Python tests → pytest** configuration.  
3. Select the test file and run.  

**Output:**  
```
========================= test session starts =========================
test_demo.py::test_print_hello PASSED                            [100%]
========================== 1 passed in 0.01s ==========================
```

### **Method 2: Command Line**
```bash
pytest test_demo.py  # Run specific file
pytest               # Run all tests in project
```

---

## **5. Key pytest Features**
### **1. Assertions**
No need for `assert` methods—use plain Python:
```python
def test_addition():
    assert 1 + 1 == 2  # Passes
```

### **2. Fixtures**
Reusable setup/teardown (covered later):
```python
import pytest

@pytest.fixture
def setup():
    print("\nSetup")  # Runs before each test

def test_example(setup):
    assert True
```

### **3. Parameterization**
Run the same test with different inputs:
```python
@pytest.mark.parametrize("a,b,expected", [(1, 2, 3), (4, 5, 9)])
def test_add(a, b, expected):
    assert a + b == expected
```

---

## **6. Next Steps**
1. **Command-Line Execution**: Learn advanced pytest CLI options.  
2. **Fixtures**: Understand setup/teardown workflows.  
3. **Integration with Selenium**: Apply pytest to UI automation.  

> **Quote from Lecture**:  
> *"All Selenium tests must be wrapped in pytest functions for framework compatibility."*  

---

### **Cheat Sheet**
| Task | Code |  
|------|------|  
| Create test file | `test_*.py` |  
| Write test function | `def test_*():` |  
| Run tests | `pytest` or PyCharm runner |  

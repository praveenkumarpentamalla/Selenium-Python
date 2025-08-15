# **Running pytest Tests from the Command Line**

## **Table of Contents**
1. [Running All Tests in a Package](#1-running-all-tests-in-a-package)
2. [Verbose Mode (-v)](#2-verbose-mode--v)
3. [Showing Console Output (-s)](#3-showing-console-output--s)
4. [Handling Test Failures](#4-handling-test-failures)
5. [Best Practices](#5-best-practices)
6. [Key Takeaways](#6-key-takeaways)

---

## **1. Running All Tests in a Package**
To run **all** pytest tests in a directory/package:
```bash
cd path/to/tests
pytest
```
- Scans all files starting/ending with `test_`  
- Executes all functions starting with `test_`  

**Example Structure:**
```
/test_package
├── test_demo.py
└── test_assertions.py
```

---

## **2. Verbose Mode (-v)**
Add `-v` for detailed output:
```bash
pytest -v
```
**Output:**
```
========================= test session starts =========================
test_demo.py::test_print_hello PASSED                            [33%]
test_demo.py::test_print_good_morning PASSED                     [66%] 
test_assertions.py::test_message FAILED                          [100%]
========================== 3 tests in 0.02s ==========================
```

---

## **3. Showing Console Output (-s)**
Use `-s` to print console logs (e.g., `print()` statements):
```bash
pytest -v -s
```
**Output:**
```
test_demo.py::test_print_hello Hello
PASSED
test_demo.py::test_print_good_morning Good morning
PASSED
```

---

## **4. Handling Test Failures**
### **Assertion Example**
```python
# test_assertions.py
def test_message():
    message = "Hello"
    assert message == "Hi", "Strings do not match"  # Fails
```
**Failure Output:**
```
test_assertions.py::test_message FAILED
AssertionError: Strings do not match
assert 'Hello' == 'Hi'
```

### **Rules for Test Methods**
- **Unique Names**: Avoid duplicate test names (overrides results).  
- **Indentation**: Maintain proper Python indentation.  

**Fix Duplicate Names:**
```python
def test_print_hello(): ...    # ✅ Unique
def test_print_hello(): ...    # ❌ Duplicate (ignored)
```

---

## **5. Best Practices**
1. **Directory Structure**:  
   ```
   /tests
   ├── __init__.py
   ├── test_login.py
   └── test_checkout.py
   ```

2. **Command Flags**:  
   | Flag | Purpose |  
   |------|---------|  
   | `-v` | Verbose output |  
   | `-s` | Show console logs |  
   | `-k` | Filter tests by name |  

3. **CI/CD Integration**:  
   ```bash
   # Jenkins/GitHub Actions command
   pytest -v -s /tests
   ```

---

## **6. Key Takeaways**
✔ Use `pytest` to run all tests in a directory.  
✔ `-v` shows detailed results; `-s` displays `print()` output.  
✔ Unique test names are **mandatory**.  
✔ Assertions automatically report failures with diffs.  

> **Next**: Learn about **pytest fixtures** for setup/teardown!  

**Example Command:**  
```bash
pytest -v -s /tests  # Run all tests with logs
```  

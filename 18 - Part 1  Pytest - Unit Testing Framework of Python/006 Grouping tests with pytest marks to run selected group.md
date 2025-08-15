# **Mastering pytest Markers: Skip, Xfail, and Custom Tags**

## **Table of Contents**
1. [Introduction to pytest Markers](#1-introduction-to-pytest-markers)
2. [Custom Markers (Tags)](#2-custom-markers-tags)
3. [Skipping Tests](#3-skipping-tests)
4. [Expected Failures (Xfail)](#4-expected-failures-xfail)
5. [Practical Examples](#5-practical-examples)
6. [Key Takeaways](#6-key-takeaways)

---

## **1. Introduction to pytest Markers**
Markers are like "tags" that let you:
✔ Group tests by category (e.g., `smoke`, `regression`)  
✔ Skip tests conditionally  
✔ Mark expected failures  

**Basic Syntax:**
```python
import pytest

@pytest.mark.<marker_name>
def test_function(): ...
```

---

## **2. Custom Markers (Tags)**
### **Creating Custom Markers**
```python
@pytest.mark.smoke
def test_login(): ...

@pytest.mark.regression
def test_checkout(): ...
```

**Run marked tests:**
```bash
pytest -m smoke -v  # Runs only smoke tests
```

**Output:**
```
collected 5 items / 3 deselected
test_login.py::test_login PASSED  [100%]
```

### **Registering Markers (Avoid Warnings)**
Create `pytest.ini`:
```ini
[pytest]
markers =
    smoke: Run smoke tests
    regression: Run regression tests
```

---

## **3. Skipping Tests**
### **Skip a Test Entirely**
```python
@pytest.mark.skip(reason="Bug #123 not fixed")
def test_payment(): ...
```

**Output:**
```
test_payment.py::test_payment SKIPPED (Bug #123 not fixed)
```

### **Skip Conditionally**
```python
import sys

@pytest.mark.skipif(sys.version_info < (3, 8), reason="Requires Python 3.8+")
def test_feature(): ...
```

---

## **4. Expected Failures (Xfail)**
Use when a test **should fail** (e.g., known bugs):

```python
@pytest.mark.xfail
def test_broken_feature():
    assert 1 == 2  # Expected to fail
```

**Behavior:**
- Runs the test but **doesn't report as failure**  
- Shows `XFAIL` in output  
- Useful for tests that are **blocking dependencies**  

**Output:**
```
test_broken.py::test_broken_feature XFAIL
```

---

## **5. Practical Examples**
### **Test Suite Organization**
```python
# test_banking.py
@pytest.mark.smoke
def test_login(): ...

@pytest.mark.regression
def test_transfer(): ...

@pytest.mark.skip("UI not ready")
def test_new_dashboard(): ...

@pytest.mark.xfail(reason="API bug #456")
def test_currency_conversion(): ...
```

**Run Commands:**
```bash
pytest -m smoke            # Run smoke tests
pytest -m "not regression" # Skip regression tests
pytest --runxfail          # Force-run xfail tests
```

---

## **6. Key Takeaways**
| Marker | Purpose | Command |
|--------|---------|---------|
| `@pytest.mark.<name>` | Custom tags | `pytest -m <name>` |
| `@pytest.mark.skip` | Skip unconditionally | Shows as `SKIPPED` |
| `@pytest.mark.xfail` | Expected failure | Shows as `XFAIL` |
| `@pytest.mark.skipif` | Conditional skip | Skips based on condition |

**Best Practices:**
1. Use **consistent naming** for markers (`smoke`, `api`, etc.)  
2. **Register markers** in `pytest.ini` to avoid warnings  
3. Combine markers with `-m "smoke and not slow"`  

> **Next**: Learn about **pytest fixtures** for setup/teardown!  

**Example Workflow:**
```bash
pytest -m "regression and not xfail" --html=report.html
``` 

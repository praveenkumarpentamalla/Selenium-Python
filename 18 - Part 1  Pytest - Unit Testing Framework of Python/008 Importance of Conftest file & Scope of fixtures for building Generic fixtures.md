# **Optimizing pytest Fixtures: conftest.py and Scopes**

## **Table of Contents**
1. [The Power of conftest.py](#1-the-power-of-conftestpy)
2. [Class-Level Fixtures](#2-class-level-fixtures)
3. [Fixture Scopes](#3-fixture-scopes)
4. [Practical Implementation](#4-practical-implementation)
5. [Key Takeaways](#5-key-takeaways)

---

## **1. The Power of conftest.py**
### **Why Use conftest.py?**
- Centralizes **shared fixtures** across multiple test files  
- Avoids duplicate fixture definitions  
- Automatically discovered by pytest  

### **Basic Structure**
```python
# conftest.py
import pytest

@pytest.fixture
def setup():
    print("\nSetup")  # Runs before each test
    yield
    print("\nTeardown")  # Runs after each test
```

**Usage in Test Files:**
```python
def test_example(setup):  # No need to import fixture
    print("Executing test")
```

---

## **2. Class-Level Fixtures**
### **Applying Fixtures to All Class Methods**
```python
import pytest

@pytest.mark.usefixtures("setup")
class TestExample:
    def test_method1(self):
        print("Method 1")

    def test_method2(self):
        print("Method 2")
```

**Behavior:**
- `setup()` runs **before each test method**  
- `teardown()` runs **after each test method**  

---

## **3. Fixture Scopes**
Control how often fixtures execute:

| Scope | Description | Analogous to |
|-------|-------------|--------------|
| `function` (default) | Runs for each test | `@BeforeEach`/`@AfterEach` |
| `class` | Runs once per class | `@BeforeClass`/`@AfterClass` |
| `module` | Runs once per module | - |
| `session` | Runs once per test run | - |

### **Class Scope Example**
```python
# conftest.py
@pytest.fixture(scope="class")
def class_setup():
    print("\nClass setup")
    yield
    print("\nClass teardown")
```

**Output for Class Tests:**
```
Class setup
Method 1
Method 2
Class teardown
```

---

## **4. Practical Implementation**
### **Selenium Example**
```python
# conftest.py
@pytest.fixture(scope="class")
def driver():
    driver = webdriver.Chrome()
    yield driver
    driver.quit()

# test_login.py
@pytest.mark.usefixtures("driver")
class TestLogin:
    def test_valid_login(self, driver):
        driver.get("https://example.com")
        # Test steps

    def test_invalid_login(self, driver):
        # Reuses same browser instance
```

**Benefits:**
- Single browser instance for all class tests  
- Automatic cleanup after all tests  

---

## **5. Key Takeaways**
| Concept | Implementation | Use Case |
|---------|----------------|----------|
| Shared Fixtures | `conftest.py` | Cross-file setups (e.g., DB connections) |
| Class Fixtures | `@pytest.mark.usefixtures` | Group related tests |
| Scoped Fixtures | `scope="class"` | Optimize slow setups |

**Best Practices:**
1. Place **global fixtures** in `conftest.py`  
2. Use **class scope** for expensive setups (e.g., browser launch)  
3. Keep **function scope** for isolated test dependencies  

> **Next**: Learn about **parameterized fixtures** for dynamic test data!  

**Example Project Structure:**
```
/tests
├── conftest.py
├── test_login.py
└── test_checkout.py
``` 

# **Parameterized Testing with pytest Fixtures**

## **Table of Contents**
1. [Introduction to Parameterization](#1-introduction-to-parameterization)
2. [Basic Parameterization](#2-basic-parameterization)
3. [Multiple Parameters](#3-multiple-parameters)
4. [Practical Browser Example](#4-practical-browser-example)
5. [Key Takeaways](#5-key-takeaways)

---

## **1. Introduction to Parameterization**
Parameterization allows:
✔ Running the **same test** with **different inputs**  
✔ Reducing code duplication  
✔ Supporting **data-driven testing**  

**Analogous to:**
- `@DataProvider` in TestNG (Java)  
- `@ParameterizedTest` in JUnit 5  

---

## **2. Basic Parameterization**
### **Single Parameter Fixture**
```python
@pytest.fixture(params=["chrome", "firefox", "ie"])
def browser(request):
    return request.param  # Returns one value at a time
```

**Test Usage:**
```python
def test_browser(browser):
    print(f"Running test on {browser}")
    # Test logic here
```

**Output:**
```
Running test on chrome
Running test on firefox 
Running test on ie
```

---

## **3. Multiple Parameters**
### **Tuple Parameterization**
```python
@pytest.fixture(params=[
    ("chrome", "user1", "pass123"),
    ("firefox", "user2", "pass456")
])
def login_data(request):
    return request.param  # Returns tuple
```

**Test Usage:**
```python
def test_login(login_data):
    browser, username, password = login_data
    print(f"Login with {browser} as {username}")
```

---

## **4. Practical Browser Example**
### **Complete Implementation**
```python
@pytest.fixture(params=[
    ("chrome", "admin", "admin123"),
    ("firefox", "guest", "guest123"),
    ("ie", "test", "test123")
])
def credentials(request):
    return request.param

def test_cross_browser(credentials):
    browser, user, pwd = credentials
    print(f"\nTesting {browser} with {user}/{pwd}")
    # Actual test would:
    # 1. Launch browser
    # 2. Enter credentials
    # 3. Verify login
```

**Output:**
```
Testing chrome with admin/admin123
Testing firefox with guest/guest123  
Testing ie with test/test123
```

---

## **5. Key Takeaways**
| Concept | Implementation | Use Case |
|---------|----------------|----------|
| Single Param | `params=["val1", "val2"]` | Browser testing |
| Multiple Params | `params=[("a","b"), ("c","d")]` | Login combinations |
| Tuple Unpacking | `browser, user = credentials` | Multi-value handling |

**Best Practices:**
1. Use **descriptive param names** (`browser` vs `param1`)  
2. **Limit combinations** to meaningful test cases  
3. Combine with **markers** for selective execution  

> **Next**: Learn about **HTML report generation** for test results!  

**Example Project Structure:**
```
/tests
├── conftest.py          # Parameterized fixtures
├── test_login.py        # Uses parameterization
└── test_browsers.py
``` 

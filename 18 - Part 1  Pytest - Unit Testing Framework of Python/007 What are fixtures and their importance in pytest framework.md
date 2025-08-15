# **Mastering pytest Fixtures: Setup and Teardown**

## **Table of Contents**
1. [Introduction to Fixtures](#1-introduction-to-fixtures)
2. [Basic Fixture Structure](#2-basic-fixture-structure)
3. [Setup and Teardown with `yield`](#3-setup-and-teardown-with-yield)
4. [Practical Applications](#4-practical-applications)
5. [Key Takeaways](#5-key-takeaways)

---

## **1. Introduction to Fixtures**
Fixtures in pytest provide:
✔ **Reusable setup/teardown** logic  
✔ **Dependency injection** for tests  
✔ **Modularity** (avoid duplicate code)  

**Analogous to:**
- `@BeforeTest`/`@AfterTest` in TestNG (Java)  
- `setUp()`/`tearDown()` in unittest (Python)  

---

## **2. Basic Fixture Structure**
### **Define a Fixture**
```python
import pytest

@pytest.fixture
def setup():
    print("\nI will execute first")  # Setup code
    yield
    print("\nI will execute last")   # Teardown code
```

### **Use in Tests**
```python
def test_demo(setup):  # Pass fixture as argument
    print("Executing test steps")
```

**Execution Flow:**
1. `setup()` runs before the test  
2. `test_demo()` executes  
3. Teardown code after `yield` runs  

---

## **3. Setup and Teardown with `yield`**
### **How `yield` Works**
- Code **before** `yield` = Setup  
- Code **after** `yield` = Teardown  

**Example:**
```python
@pytest.fixture
def browser_setup():
    print("\nLaunching browser")  # Setup
    yield
    print("\nClosing browser")   # Teardown
```

### **Output**
```
Launching browser
Executing test steps
Closing browser
```

---

## **4. Practical Applications**
### **Selenium Example**
```python
@pytest.fixture
def driver():
    driver = webdriver.Chrome()  # Setup
    yield driver
    driver.quit()               # Teardown

def test_login(driver):
    driver.get("https://example.com")
    assert "Example" in driver.title
```

### **Database Connection**
```python
@pytest.fixture
def db_connection():
    conn = create_db_connection()  # Setup
    yield conn
    conn.close()                  # Teardown
```

---

## **5. Key Takeaways**
| Feature | Description |  
|---------|-------------|  
| `@pytest.fixture` | Marks a function as a fixture |  
| `yield` | Separates setup/teardown logic |  
| Test Args | Fixtures are injected via test arguments |  

**Best Practices:**
1. Name fixtures descriptively (e.g., `browser_setup`)  
2. Keep fixtures in `conftest.py` for project-wide reuse  
3. Combine with scope (e.g., `@pytest.fixture(scope="module")`)  

> **Next**: Learn about **fixture scopes** (module, session)!  

**Example Workflow:**
```bash
pytest -v test_login.py  # Runs with fixture
``` 

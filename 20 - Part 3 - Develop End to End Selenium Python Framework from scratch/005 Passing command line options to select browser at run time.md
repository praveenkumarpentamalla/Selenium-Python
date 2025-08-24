# **Chapter 4: Adding Command-Line Browser Selection**

## **Index**

**Chapter 4: Adding Command-Line Browser Selection**
*   **4.1 The Need for Cross-Browser Testing**
    *   Moving Beyond Hardcoded Chrome
    *   The Goal: Runtime Browser Control
*   **4.2 Defining a Custom Pytest Command-Line Option**
    *   The `pytest_addoption` Hook
    *   Syntax and Parameters: `dest`, `action`, `default`, `help`
    *   Implementing the `--browser_name` Option
*   **4.3 Retrieving the Option Value in a Fixture**
    *   Using `request.config.getoption()`
    *   Storing the Value in a Variable
*   **4.4 Implementing Conditional Browser Invocation**
    *   Using `if-elif` Logic to Choose a Browser
    *   Adding Chrome, Firefox, and Edge Options
    *   Maintaining Common Setup Steps
*   **4.5 Running Tests with the New Option**
    *   Syntax: `pytest --browser_name=<browser>`
    *   Verifying Chrome, Firefox, and Default Behavior
*   **4.6 Chapter Summary & What's Next?**
    *   Recap: Dynamic Browser Control via CLI
    *   Preview: Integrating Logging and Reporting

---

## **4.1 The Need for Cross-Browser Testing**

So far, our framework has been hardcoded to run tests exclusively in the Chrome browser. While this is fine for development, a robust automation framework must support **cross-browser testing**. You should be able to run your tests on Chrome, Firefox, Edge, or any other browser effortlessly.

The goal is to control the browser **at runtime** from the command line, like so:
```bash
# To run tests in Chrome
pytest --browser_name=chrome

# To run tests in Firefox
pytest --browser_name=firefox
```
If no browser is specified, the framework should default to a pre-defined choice (e.g., Chrome). This approach provides maximum flexibility without changing any code.

## **4.2 Defining a Custom Pytest Command-Line Option**

To achieve this, we use a special pytest hook called `pytest_addoption`. This hook allows us to register custom command-line arguments that pytest will recognize.

We add this code to our `conftest.py` file.

**File: `conftest.py` (Partial)**
```python
# File: conftest.py
import pytest

def pytest_addoption(parser):
    parser.addoption(
        "--browser_name", 
        action="store", 
        default="chrome", 
        help="Choose browser: chrome or firefox"
    )
```

### **Parameter Breakdown:**

*   **`--browser_name`**: This is the name of the command-line option you will use.
*   **`action="store"`**: This tells pytest to store the value that follows the option (e.g., `--browser_name firefox` stores the string `"firefox"`).
*   **`default="chrome"`**: If the `--browser_name` option is not provided at all, the value will default to `"chrome"`.
*   **`help="Choose browser..."`**: This text provides a description of the option when someone runs `pytest --help`.

## **4.3 Retrieving the Option Value in a Fixture**

Once the option is defined, we need to access its value inside our `setup` fixture. We do this using the `request.config.getoption()` method.

We modify the `setup` fixture in `conftest.py` to retrieve the value.

**File: `conftest.py` (Partial - inside fixture)**
```python
@pytest.fixture(scope="class")
def setup(request):
    # Retrieve the value passed from the command line
    browser_name = request.config.getoption("--browser_name")
    # Now, 'browser_name' will be either "chrome", "firefox", or any other value passed
```

## **4.4 Implementing Conditional Browser Invocation**

Now that we have the browser name stored in the `browser_name` variable, we can use conditional logic (`if-elif` statements) to initialize the correct WebDriver.

**Final Implementation in `conftest.py`:**
```python
# File: conftest.py
import pytest
from selenium import webdriver

def pytest_addoption(parser):
    parser.addoption(
        "--browser_name", action="store", default="chrome", help="Choose browser: chrome or firefox"
    )

@pytest.fixture(scope="class")
def setup(request):
    browser_name = request.config.getoption("--browser_name")
    driver = None  # Initialize driver to None

    # Conditional Logic to Invoke Browser
    if browser_name == "chrome":
        driver = webdriver.Chrome()
    elif browser_name == "firefox":
        driver = webdriver.Firefox()
    elif browser_name == "edge":
        driver = webdriver.Edge()
    else:
        print(f"Browser {browser_name} is not supported. Defaulting to Chrome.")
        driver = webdriver.Chrome()

    # Common Setup Steps for ALL Browsers
    driver.get("https://www.rahulshettyacademy.com")
    driver.maximize_window()

    # Pass driver to the test class and yield for teardown
    request.cls.driver = driver
    yield
    driver.quit()
```

### **Code Explanation:**

1.  **`driver = None`**: We initialize the `driver` variable to `None`. This is a good practice before defining it conditionally.
2.  **`if-elif-else` Block**: This block checks the value of `browser_name` and initializes the corresponding WebDriver.
    *   The `else` block acts as a fallback, defaulting to Chrome if an unrecognized browser name is provided.
3.  **Common Steps**: The steps to navigate to the URL and maximize the window are **outside** the `if-elif` block. This ensures they run for every browser, avoiding code duplication.
4.  **Unchanged Fixture Logic**: The mechanism for passing the driver to the test class (`request.cls.driver`) and the teardown step (`yield` and `driver.quit()`) remains the same and works for any browser.

## **4.5 Running Tests with the New Option**

You can now control the browser directly from the terminal.

**1. Run tests in Firefox:**
```bash
pytest --browser_name=firefox
```

**2. Run tests in Chrome (explicitly):**
```bash
pytest --browser_name=chrome
```

**3. Run tests using the default browser (Chrome, because of `default="chrome"`):**
```bash
pytest
# This is equivalent to: pytest --browser_name=chrome
```

When you run with `--browser_name=firefox`, you should see the Firefox browser open and execute the test, proving that our command-line control is working perfectly.

## **4.6 Chapter Summary & What's Next?**

In this chapter, we significantly enhanced our framework's capabilities:
*   We moved away from a **hardcoded browser** to a **dynamically selected** one.
*   Learned to use the **`pytest_addoption`** hook to define custom command-line arguments.
*   Used **`request.config.getoption()`** to retrieve the argument's value within a fixture.
*   Implemented **conditional logic** to initialize the correct WebDriver based on the user's input.
*   Maintained **clean code** by keeping common setup steps outside the conditional block.

Our framework now supports true cross-browser testing, a critical requirement for any real-world automation project.

In the next chapter, we will integrate **logging** into our framework. Logging is essential for creating detailed execution records, debugging failures, and generating meaningful reports. We will learn how to create log files that capture the journey of our tests, from browser initialization to every action and assertion.

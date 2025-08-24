# **Building a Selenium pytest Framework from Scratch**

## **Index**

**Chapter 1: Laying the Foundation - Your First Selenium pytest Test**
*   **1.1 Introduction to the Framework Building Journey**
    *   Goals of a Robust Automation Framework
    *   What You Will Learn (POM, Utilities, Data-Driven Testing, Logging, Reporting, CI/CD)
*   **1.2 The First Step: pytest Standards for Selenium**
    *   Why Use a Testing Framework?
    *   Benefits of pytest (Simple Syntax, Fixtures, Parameterization, Plugins)
    *   Creating a Dedicated Project Structure
*   **1.3 Writing Your First pytest Test Case**
    *   `pytest` Naming Conventions (`test_` prefix for files and functions, `Test` prefix for classes)
    *   Converting a Linear Script to a Structured Test
    *   Code Example: `test_end_to_end.py`
    *   Explanation of Key Components: Class Organization, The `self` Parameter, Assertions
*   **1.4 Managing Project Dependencies**
    *   Understanding Isolated Python Environments
    *   How to Install Packages with `pip` (Selenium, pytest)
    *   Verifying Installations in Your IDE
*   **1.5 Running Your First Test**
    *   How to Execute Tests from the Command Line
    *   How to Execute Tests from an IDE (PyCharm Example)
    *   Understanding `pytest` Output (PASSED, FAILED)


---

# **Chapter 1: Laying the Foundation - Your First Selenium pytest Test**

## **1.1 Introduction to the Framework Building Journey**

Welcome! In this section, we will embark on a comprehensive journey to build a robust and scalable **Selenium WebDriver automation framework** from the ground up. We will move beyond writing simple, linear scripts and learn industry-standard practices that make automation efficient, maintainable, and powerful.

Our goal is to implement a framework that includes:
*   **Standards for Writing Tests:** Using the `pytest` framework.
*   **Page Object Model (POM):** A design pattern to make your code more readable and maintainable.
*   **Utilities & Helpers:** Reusable functions for common tasks.
*   **Parameterization & Data-Driven Testing:** Running tests with multiple sets of data.
*   **Logging:** Keeping detailed records of test execution.
*   **Reporting:** Generating clear and informative test reports.
*   **Jenkins Integration:** Running your tests automatically in a CI/CD pipeline.

We will build this step-by-step, introducing each concept clearly with practical examples.

---

## **1.2 The First Step: pytest Standards for Selenium**

### **Why pytest?**

In your initial Selenium learning, you likely wrote scripts that ran directly. `pytest` is a testing framework that provides a much more structured and powerful way to organize and execute your tests. It offers features like:
*   **Simple Syntax:** Tests are easy to write and read.
*   **Fixtures:** For setup and teardown (e.g., initializing and closing the browser).
*   **Parameterization:** To run the same test with different inputs.
*   **Rich Plugin Ecosystem:** For enhanced reporting and functionality.

### **Setting Up a Dedicated Project**

It's a best practice to keep your framework project separate from your learning or experimental code. This keeps the environment clean and manageable.

**Example: Creating a New Project Structure**

Let's create a new project directory specifically for our framework.

1.  Open your IDE (like PyCharm).
2.  Create a new project. Let's name it `selenium_pytest_framework`.
3.  Inside this project, create a Python package specifically for your tests. It's common to name this `tests`.

Your initial project structure should look like this:
```
selenium_pytest_framework/
│
└───tests/
    │   __init__.py
    │
```

*   The `__init__.py` file (which can be empty) tells Python that the `tests` directory is a package, allowing us to import modules from it later.

---

## **1.3 Writing Your First pytest Test Case**

A "test case" in `pytest` is essentially a function whose name starts with `test_`. For better organization, we can group related test functions inside a **class**. The class name should also start with `Test`.

Let's convert a simple Selenium script into a proper `pytest` test class.

**Before (Plain Selenium Script):**
```python
from selenium import webdriver

driver = webdriver.Chrome()
driver.get("https://www.example.com")
title = driver.title
print(title)
assert "Example" in title
driver.quit()
```

**After (Structured pytest Test Class):**

Create a new file inside the `tests` package named `test_end_to_end.py`.

```python
# File: tests/test_end_to_end.py
from selenium import webdriver

class TestEndToEnd:
    # This is a test method. pytest will recognize it because it starts with 'test_'
    def test_example_website_title(self):
        # 1. Setup: Initialize the WebDriver
        driver = webdriver.Chrome()
        
        # 2. Test Action: Navigate to a webpage
        driver.get("https://www.example.com")
        
        # 3. Assertion: Verify the expected condition
        assert "Example" in driver.title
        
        # 4. Teardown: Clean up and close the browser
        driver.quit()
```

### **Key Differences Explained:**

1.  **Class Organization:** The test is wrapped inside a class `TestEndToEnd`. This is good practice for grouping related tests and using shared setup/teardown methods later.
2.  **The `self` Parameter:** Because the test method is inside a class, it must have `self` as its first parameter. This gives it access to the class's attributes and methods.
3.  **Test Identification:** The method name `test_example_website_title` starts with `test_`, which is how `pytest` automatically discovers and runs it.
4.  **Assert Statement:** We use Python's built-in `assert` statement for checks. `pytest` will beautifully report any failures from this assertion.

---

## **1.4 Managing Project Dependencies**

A new project has its own isolated Python environment. This means even if you have `selenium` and `pytest` installed elsewhere, you need to install them for *this specific project*.

### **How to Install Dependencies:**

Open your terminal or command prompt and navigate to your project's root directory (`selenium_pytest_framework`). Then run the following commands:

```bash
# Install the Selenium WebDriver package
pip install selenium

# Install the pytest testing framework
pip install pytest
```

**Verifying in the IDE:**
Most modern IDEs (like PyCharm) show which packages are available in the current project's interpreter. After running the commands above, you should see `selenium` and `pytest` listed in your project's settings/interpreter section. If you see "Unresolved reference" errors in your code, it usually means the package isn't installed in this environment.

---

## **1.5 Running Your First Test**

Now that everything is set up, you can run your test.

1.  **Using the Command Line:**
    *   Open a terminal in your project root.
    *   Simply run the command: `pytest`
    *   `pytest` will automatically discover all files starting with `test_` and functions/methods starting with `test_`, and then execute them.

2.  **Using Your IDE (Recommended for Beginners):**
    *   In PyCharm, you can right-click on the test file (`test_end_to_end.py`) or the specific test method and choose **Run 'pytest in...'**.
    *   The IDE will handle the command for you and show the output in a dedicated "Run" window.

You should see output indicating that your test passed (`PASSED`) or failed (`FAILED`), along with the name of the test that was run.

---


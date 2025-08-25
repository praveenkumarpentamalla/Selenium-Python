# **Advanced Reporting and Screenshots in Selenium with Pytest**

## **Table of Contents**
1. [Introduction to Advanced Reporting](#1-introduction-to-advanced-reporting)
2. [Generating HTML Reports with pytest-html](#2-generating-html-reports-with-pytest-html)
3. [Capturing Screenshots on Test Failures](#3-capturing-screenshots-on-test-failures)
4. [Integrating Logs and Screenshots into Reports](#4-integrating-logs-and-screenshots-into-reports)
5. [Best Practices](#5-best-practices)
6. [Summary](#6-summary)

---

## **1. Introduction to Advanced Reporting**

Advanced reporting enhances test visibility by:
- Generating HTML reports with detailed test results.
- Capturing screenshots on failures for debugging.
- Integrating logs directly into reports.

### **Tools Used:**
- **pytest-html**: Plugin for generating HTML reports.
- **Custom Hooks**: To capture screenshots on failures.

---

## **2. Generating HTML Reports with pytest-html**

### **Installation:**
```bash
pip install pytest-html
```

### **Usage:**
Run tests with the `--html` flag to generate a report:
```bash
pytest --html=report.html
```

### **Customizing Report Path:**
```bash
pytest --html=reports/report.html
```

### **Benefits:**
- Easy to share with stakeholders.
- Includes pass/fail summaries and logs.

---

## **3. Capturing Screenshots on Test Failures**

### **Step 1: Modify `conftest.py` to Capture Screenshots**
Add the following code to `conftest.py`:

```python
import pytest
from selenium import webdriver

# Declare global driver variable
driver = None

@pytest.hookimpl(hookwrapper=True)
def pytest_runtest_makereport(item, call):
    """
    Hook to capture screenshot on test failure and attach it to the report.
    """
    pytest_html = item.config.pluginmanager.getplugin("html")
    outcome = yield
    report = outcome.get_result()
    extra = getattr(report, "extra", [])
    
    if report.when == "call" and report.failed:
        # Capture screenshot
        file_name = f"{item.name}.png"
        driver.save_screenshot(file_name)
        
        # Attach screenshot to report
        if pytest_html:
            extra.append(pytest_html.extras.png(file_name))
        report.extra = extra

@pytest.fixture(scope="session", autouse=True)
def browser_setup():
    """
    Global setup for WebDriver.
    """
    global driver
    driver = webdriver.Chrome()
    yield driver
    driver.quit()
```

### **Explanation:**
- `pytest_runtest_makereport`: Hook to capture screenshots on test failures.
- `driver.save_screenshot`: Saves the screenshot with the test name.
- `pytest_html.extras.png`: Attaches the screenshot to the HTML report.

---

## **4. Integrating Logs and Screenshots into Reports**

### **Step 1: Ensure Logs Are Generated**
Use the logging utility from `BaseClass` to generate logs:

```python
class BaseClass:
    def get_logger(self):
        # Logger setup code here
        return logger
```

### **Step 2: Run Tests with Logs and Reports**
```bash
pytest --html=report.html -v
```

### **Viewing the Report:**
- Open `report.html` in a browser.
- Screenshots and logs are embedded in the report for failed tests.

---

## **5. Best Practices**

1. **Organize Reports and Screenshots:**
   - Store reports in a `reports/` directory.
   - Store screenshots in a `screenshots/` directory.

2. **Use Descriptive Names:**
   - Name screenshots with the test method name for easy identification.

3. **Clean Up Old Reports:**
   - Automatically delete old reports before generating new ones.

4. **Integrate with CI/CD:**
   - Generate reports automatically in CI/CD pipelines.

---

## **6. Summary**

- **HTML Reports**: Use `pytest-html` to generate detailed reports.
- **Screenshots on Failure**: Capture screenshots using hooks in `conftest.py`.
- **Log Integration**: Ensure logs are included in reports for debugging.
- **Global Driver**: Use a global driver variable to access WebDriver in hooks.

---

## **Next Steps**
- Automate report generation in CI/CD pipelines.
- Explore advanced reporting tools like Allure.
- Implement video recording for test executions.

---

**Note**: This setup provides a robust reporting mechanism that includes screenshots and logs, making debugging easier and reports more informative.

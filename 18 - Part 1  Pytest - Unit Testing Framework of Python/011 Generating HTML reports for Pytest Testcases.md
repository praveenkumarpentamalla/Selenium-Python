# **Generating HTML Test Reports with pytest**

## **Table of Contents**
1. [Installing pytest-html](#1-installing-pytest-html)
2. [Basic Report Generation](#2-basic-report-generation)
3. [Customizing Reports](#3-customizing-reports)
4. [Viewing Report Output](#4-viewing-report-output)
5. [Key Takeaways](#5-key-takeaways)

---

## **1. Installing pytest-html**
First, install the HTML reporting plugin:
```bash
pip install pytest-html
```

**Verification:**
```bash
pytest --version
# Should show pytest-html in plugins list
```

---

## **2. Basic Report Generation**
### **Command Line Usage**
Generate an HTML report for all tests:
```bash
pytest --html=report.html
```

**Example Output Structure:**
```
Results (1.23s):
    5 passed
    1 failed
    2 skipped
```

### **Report File Location**
- Creates `report.html` in current directory
- Customize path: `--html=reports/my_report.html`

---

## **3. Customizing Reports**
### **Add Environment Info**
```bash
pytest --html=report.html --metadata "Browser" "Chrome" --metadata "OS" "Windows"
```

### **Include Logs in Report**
```bash
pytest --html=report.html -v -s
```
- `-s` captures print statements
- `-v` shows detailed output

---

## **4. Viewing Report Output**
### **Report Sections**
1. **Summary Dashboard**
   - Pass/Fail/Skip counts
   - Execution time
2. **Test Details**
   - Full test names
   - Failure traces
3. **Environment**
   - Python/pytest versions
   - Plugins
   - Custom metadata

**Sample Report View:**
```html
<div class="results">
  <div class="passed">✓ test_login (Chrome)</div>
  <div class="failed">✗ test_checkout (Firefox)</div>
</div>
```

---

## **5. Key Takeaways**
| Feature | Command | Benefit |
|---------|---------|---------|
| Basic Report | `--html=report.html` | Quick test visualization |
| Metadata | `--metadata KEY VALUE` | Track test environment |
| Log Capture | `-s` flag | Include debug prints |

**Best Practices:**
1. Store reports in `reports/` directory  
2. Add timestamps to filenames:  
   ```bash
   pytest --html=reports/report_$(date +%Y%m%d).html
   ```
3. Integrate with CI/CD pipelines  

> **Next**: Learn how to **capture and embed logs** in HTML reports!

**Example Workflow:**
```bash
# Run tests and generate timestamped report
pytest --html=reports/$(date +%Y%m%d_%H%M).html -v -s
```

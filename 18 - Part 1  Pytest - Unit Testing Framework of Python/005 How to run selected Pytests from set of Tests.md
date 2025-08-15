# **Running Selective pytest Tests: A Comprehensive Guide**

## **Table of Contents**
1. [Running Specific Test Files](#1-running-specific-test-files)
2. [Running Tests by Keyword Matching](#2-running-tests-by-keyword-matching)
3. [Organizing Tests with Naming Conventions](#3-organizing-tests-with-naming-conventions)
4. [Command Line Flags Cheat Sheet](#4-command-line-flags-cheat-sheet)
5. [Practical Examples](#5-practical-examples)
6. [Key Takeaways](#6-key-takeaways)

---

## **1. Running Specific Test Files**
To run tests from a **single file**:
```bash
pytest path/to/test_file.py
```

**Example:**
```bash
pytest tests/test_demo.py -v -s
```
- Runs only tests in `test_demo.py`
- `-v` for verbose output
- `-s` to show print statements

---

## **2. Running Tests by Keyword Matching**
Use `-k` to run tests matching a **name pattern**:
```bash
pytest -k "credit_card" -v
```

**What it does:**
- Scans all test methods/files
- Runs only tests containing "credit_card" in their names
- Skips non-matching tests

**Example Test Structure:**
```python
def test_credit_card_payment(): ...  # Will run
def test_savings_account(): ...     # Will skip
```

---

## **3. Organizing Tests with Naming Conventions**
### **Best Practices for Test Names**
1. **Module-Specific Prefixes**:
   ```python
   def test_cc_validate_card(): ...       # Credit card
   def test_ba_check_balance(): ...       # Bank account
   ```
2. **Behavior-Driven Names**:
   ```python
   def test_login_with_valid_credentials(): ...
   def test_login_locks_after_3_attempts(): ...
   ```

### **Why Good Names Matter**
✔ Enables targeted test execution (`-k "cc_"`)  
✔ Makes reports self-documenting  
✔ Helps in CI/CD pipeline filtering  

---

## **4. Command Line Flags Cheat Sheet**
| Command | Purpose | Example |
|---------|---------|---------|
| `pytest path/file.py` | Run specific file | `pytest tests/login_test.py` |
| `pytest -k "pattern"` | Run matching tests | `pytest -k "smoke"` |
| `pytest -v` | Verbose output | `pytest -v` |
| `pytest -s` | Show print logs | `pytest -s` |
| `pytest -m marker` | Run marked tests | `pytest -m slow` |

---

## **5. Practical Examples**
### **Banking Application Tests**
```python
# test_credit_cards.py
def test_cc_minimum_payment(): ...
def test_cc_late_fee_charge(): ...

# test_accounts.py  
def test_ba_overdraft(): ...
def test_ba_transfer(): ...
```

**Run only credit card tests:**
```bash
pytest -k "cc_" -v
```

**Output:**
```
collected 4 items / 2 deselected
test_credit_cards.py::test_cc_minimum_payment PASSED
test_credit_cards.py::test_cc_late_fee_charge PASSED
```

---

## **6. Key Takeaways**
1. **Target Execution**: Use `pytest file.py` for single-file runs  
2. **Smart Filtering**: `-k` selects tests by name patterns  
3. **Clear Naming**: Follow `<module>_<behavior>` conventions  
4. **CI/CD Ready**: These commands work in Jenkins/GitHub Actions  

> **Pro Tip**: Combine flags for powerful control:  
> ```bash 
> pytest -k "login" -v -s --html=report.html
> ```

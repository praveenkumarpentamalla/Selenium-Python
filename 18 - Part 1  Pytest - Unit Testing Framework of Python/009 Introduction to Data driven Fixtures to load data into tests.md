# **Data-Driven Testing with pytest Fixtures**

## **Table of Contents**
1. [Fixtures for Test Data](#1-fixtures-for-test-data)
2. [Returning Data from Fixtures](#2-returning-data-from-fixtures)
3. [Accessing Fixture Data in Tests](#3-accessing-fixture-data-in-tests)
4. [Practical Example: User Profile Data](#4-practical-example-user-profile-data)
5. [Key Differences in Fixture Usage](#5-key-differences-in-fixture-usage)
6. [Key Takeaways](#6-key-takeaways)

---

## **1. Fixtures for Test Data**
Fixtures can **generate and return test data** before test execution:
```python
@pytest.fixture
def user_profile_data():
    print("\nCreating user profile data")
    return ["Rahul", "Chitti", "rahul@academy.com"]
```

**Why Use Fixtures for Data?**
✔ Centralizes test data management  
✔ Avoids hardcoding values in tests  
✔ Enables reusable data setups  

---

## **2. Returning Data from Fixtures**
### **Returning a Tuple**
```python
@pytest.fixture
def data_load():
    return ("Rahul", "Chitti", "rahul@academy.com")
```

### **Returning a Dictionary (Recommended)**
```python
@pytest.fixture 
def user_data():
    return {
        "first_name": "Rahul",
        "last_name": "Chitti",
        "email": "rahul@academy.com"
    }
```

---

## **3. Accessing Fixture Data in Tests**
### **Method 1: Direct Parameter Injection**
```python
def test_profile(data_load):
    print(data_load[0])  # Rahul
    print(data_load[2])  # rahul@academy.com
```

### **Method 2: Class-Level with usefixtures**
```python
@pytest.mark.usefixtures("data_load")
class TestProfile:
    def test_name(self, data_load):
        assert data_load[0] == "Rahul"
```

---

## **4. Practical Example: User Profile Data**
### **Test Implementation**
```python
def test_user_profile(user_data):
    # Simulate form filling
    print(f"Entering first name: {user_data['first_name']}")
    print(f"Entering email: {user_data['email']}")
    assert user_data["first_name"] == "Rahul"
```

### **Output**
```
Creating user profile data
Entering first name: Rahul  
Entering email: rahul@academy.com
```

---

## **5. Key Differences in Fixture Usage**
| Scenario | Fixture Declaration | Test Parameter Required? |
|----------|---------------------|--------------------------|
| Setup/Teardown | No return value | No |
| Data Provision | Returns value | **Yes** |
| Class-level (usefixtures) | Either | Only if accessing returned data |

**Interview Tip**:  
> *"You must pass the fixture as a test parameter when you need to access its returned data, even if declared globally."*

---

## **6. Key Takeaways**
1. **Data Fixtures** centralize test data generation  
2. **Return dictionaries** for readable key-value access  
3. **Always pass** fixtures as parameters when using returned data  
4. **Combine** with `@pytest.mark.usefixtures` for class-wide availability  

**Next Steps**:  
- Learn about **parameterized fixtures** for dynamic datasets  
- Explore **external data sources** (JSON, Excel)  

**Example Project Structure**:
```
/tests
├── conftest.py          # Shared fixtures
├── test_profiles.py     # Uses data fixtures
└── test_checkout.py
```

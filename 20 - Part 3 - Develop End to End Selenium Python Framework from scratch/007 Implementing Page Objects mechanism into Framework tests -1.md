Of course. Here is the next chapter of the book, detailing the implementation of the Page Object Model for the CheckoutPage and the optimization of object creation.

***

# **Chapter 6: Completing the Page Objects and Optimizing Object Creation**

## **Index**

**Chapter 6: Completing the Page Objects and Optimizing Object Creation**
*   **6.1 Expanding the Page Object Model**
    *   Assigning Locators to Their Correct Pages
    *   Creating the `CheckoutPage` Class
    *   The Constructor: Injecting the Driver
*   **6.2 Building Page Object Methods**
    *   From Locators to Action Methods
    *   The `*` Operator for Tuple Unpacking
    *   Example: `getCardTitles()` and `checkoutItems()`
*   **6.3 Using Page Objects in Tests**
    *   Instantiating Page Objects in the Test
    *   The Current "Inefficient" Approach
*   **6.4 The Next Level of Optimization**
    *   The Problem: Multiple Object Instantiations
    *   The Goal: A Centralized, Efficient Way to Manage Pages
*   **6.5 Chapter Summary & What's Next?**
    *   Recap: A Functional but Cluttered POM Implementation
    *   Preview: Introducing a Page Object Manager

---

## **6.1 Expanding the Page Object Model**

In the previous chapter, we created the `HomePage` class. Now, we must continue this process for every distinct page in our application's flow. After clicking the "Shop" link on the homepage, the user is taken to the **Checkout Page** (or a Shopping Cart page). All elements on this new page belong to a new page object class.

**Remember the Rule of POM:** Each unique web page (or major component) gets its own class. The locators for elements on the Checkout Page should not be in `HomePage`; they belong in `CheckoutPage`.

## **6.2 Building the `CheckoutPage` Class**

Let's build the `CheckoutPage` class step-by-step, following the same pattern as `HomePage`.

**File: `page_objects/checkout_page.py`**
```python
# File: page_objects/checkout_page.py
from selenium.webdriver.common.by import By

class CheckoutPage:

    def __init__(self, driver):  # Constructor to receive the driver
        self.driver = driver     # Assign it to an instance variable

    # 1. LOCATOR for the product cards (Finds multiple elements)
    card_title = (By.CSS_SELECTOR, ".card-title a")
    # This locator finds all the product names on the page.

    # 2. LOCATOR for the "Add" button for a specific product (Finds multiple elements)
    card_footer = (By.CSS_SELECTOR, ".card-footer button")
    # This locator finds all the "Add" buttons.

    # 3. LOCATOR for the final checkout button (Finds a single element)
    checkout_button = (By.XPATH, "//button[@class='btn btn-success']")

    # 4. ACTION METHOD: Get all product titles
    def getCardTitles(self):
        # find_elements returns a list of WebElements
        return self.driver.find_elements(*self.card_title)

    # 5. ACTION METHOD: Click the "Add" button for a specific product
    def getCardFooter(self):
        # find_elements returns a list of WebElements (all "Add" buttons)
        return self.driver.find_elements(*self.card_footer)

    # 6. ACTION METHOD: Click the final checkout button
    def checkOutItems(self):
        # find_element is used for a single, unique element
        self.driver.find_element(*self.checkout_button).click()
```

### **Code Explanation:**

1.  **`def __init__(self, driver):`** The constructor is identical to `HomePage`. It ensures the `driver` object is passed from the test and stored for use in all methods.
2.  **Locators as Tuples:** Each locator is defined as a class variable using a tuple `(By.STRATEGY, "locator_value")`.
3.  **`find_elements` vs. `find_element`:**
    *   `getCardTitles()` and `getCardFooter()` use `find_elements` (plural) because they need to get a **list** of all product titles or buttons on the page. We will loop through this list in our test.
    *   `checkOutItems()` uses `find_element` (singular) because the final checkout button is a unique element on the page.
4.  **The `*` Operator:** Just like before, the `*` unpacks the tuple into the two arguments required by `find_element` and `find_elements`.

## **6.3 Using Page Objects in Tests (The Current Way)**

Now, let's see how we use these page objects in our test. We create instances of the page classes and call their methods.

**File: `tests/test_end_to_end.py` (Partial Example)**
```python
# File: tests/test_end_to_end.py
from utilities.base_class import BaseClass
from page_objects.home_page import HomePage
from page_objects.checkout_page import CheckoutPage

class TestEndToEnd(BaseClass):

    def test_e2e(self):
        # 1. Create HomePage Object and perform an action
        homepage = HomePage(self.driver) # Pass the driver from the test
        homepage.clickShopLink()         # Use the page object method

        # 2. Create CheckoutPage Object and perform actions
        checkoutpage = CheckoutPage(self.driver) # Pass the same driver
        products = checkoutpage.getCardTitles()  # Get list of products

        # ... (test logic to iterate through products would go here)
        checkoutpage.checkOutItems() # Click the checkout button
```

## **6.4 The Next Level of Optimization**

The current approach works, but it has a minor inefficiency: we are creating a new instance of a page object (e.g., `checkoutpage = CheckoutPage(self.driver)`) every time we need to use it. While not a huge problem for small tests, it can be optimized.

**The goal for the next optimization** is to manage our page objects more efficiently. Instead of instantiating them manually throughout the test, we can create a central **"Page Object Manager"**. This manager would be responsible for providing the correct page object, potentially handling the creation logic and ensuring we don't create unnecessary duplicate objects.

This advanced pattern further cleans up the test code, but the basic pattern of creating objects with `PageName(self.driver)` is perfectly valid and widely used.

## **6.5 Chapter Summary & What's Next?**

In this chapter, we solidified our understanding of the Page Object Model:
*   We created the **`CheckoutPage`** class, following the same structure as `HomePage`.
*   Defined **locators** specific to the checkout process.
*   Wrote **action methods** that use the locators and the injected `driver` to interact with the page.
*   Learned the difference between **`find_element`** (for single elements) and **`find_elements`** (for lists of elements).
*   Saw how to use these page objects in a test by **instantiating them** with `self.driver`.

Our test is now much more maintainable. If the locator for the checkout button changes, we only update it in one place: the `CheckoutPage` class.

In the next chapter, we will complete the final page object for the confirmation page and, more importantly, we will **rewrite our complete end-to-end test** to use all the page objects we've created. This will demonstrate the full power of POM, transforming our test from a script of low-level commands into a clean, readable, and robust user story.

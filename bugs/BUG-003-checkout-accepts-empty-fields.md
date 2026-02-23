# BUG-003: Checkout Form Accepts Submission With Empty Required Fields

## Summary
On the checkout information page, clicking "Continue" with all fields left blank does not display a validation error. The form submits and navigates the user forward in the checkout flow despite missing required information.

---

## Environment
| Field | Details |
|---|---|
| **Application** | SauceDemo (https://www.saucedemo.com) |
| **Browser** | Google Chrome 121.0.6167.160 |
| **Operating System** | Windows 11 |
| **Test Date** | 2026-02-23 |
| **Tester** | Kavya |

---

## Severity & Priority
| Field | Rating |
|---|---|
| **Severity** | High |
| **Priority** | High |

> **Justification:** Allowing checkout without required information breaks the order fulfilment process and could result in invalid orders being placed.

---

## Steps to Reproduce
1. Navigate to https://www.saucedemo.com and log in with `standard_user` / `secret_sauce`
2. Add any item to cart
3. Click the cart icon and click **Checkout**
4. On the "Checkout: Your Information" page, leave **First Name**, **Last Name**, and **Postal Code** all blank
5. Click the **Continue** button

---

## Expected Result
Form validation triggers and displays error messages for each empty required field, preventing the user from proceeding. Example:
> *"Error: First Name is required"*

## Actual Result
The form submits successfully with no validation errors. The user is advanced to the "Checkout: Overview" page despite all fields being empty.

---

## Additional Notes
- Reproducible 100% of the time
- Tested with all fields empty and with partial fields empty — both bypass validation
- This is a data integrity issue that could result in incomplete order records
- **Workaround:** None — server-side validation should be implemented as a fallback

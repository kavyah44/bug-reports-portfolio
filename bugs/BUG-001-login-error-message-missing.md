# BUG-001: Login Page Shows No Error on Invalid Credentials

## Summary
When a user enters an incorrect password on the login page and clicks the login button, no error message is displayed. The page reloads silently, leaving the user with no feedback.

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

> **Justification:** Users have no way to understand why login failed. This breaks the core authentication flow and could cause users to abandon the application.

---

## Steps to Reproduce
1. Navigate to https://www.saucedemo.com
2. Enter a valid username: `standard_user`
3. Enter an **incorrect** password: `wrong_password`
4. Click the **Login** button

---

## Expected Result
An error message is displayed informing the user that the credentials are incorrect, e.g.:
> *"Username and password do not match any user in this service."*

## Actual Result
The page reloads with no error message. The username and password fields are cleared. The user is left with no feedback indicating what went wrong.

---

## Additional Notes
- This issue is reproducible 100% of the time
- Affects all invalid credential combinations tested
- The error message **does** appear correctly for the `locked_out_user` account, suggesting the error component exists but is not triggered consistently
- Workaround: None available for end users

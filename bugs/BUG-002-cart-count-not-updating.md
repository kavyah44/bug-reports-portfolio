# BUG-002: Cart Item Count Badge Not Updating After Item Removal

## Summary
When a user removes an item from the shopping cart, the cart icon badge count in the navigation bar does not update immediately. The stale count persists until the page is manually refreshed.

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
| **Severity** | Medium |
| **Priority** | Medium |

> **Justification:** Users cannot trust the cart badge count, leading to confusion about the contents of their cart. This affects the core shopping flow.

---

## Steps to Reproduce
1. Navigate to https://www.saucedemo.com and log in with `standard_user` / `secret_sauce`
2. Click **Add to cart** on any two products
3. Verify the cart badge shows **2**
4. Click the cart icon to open the cart page
5. Click **Remove** on one of the items
6. Observe the cart badge count in the top-right navigation

---

## Expected Result
The cart badge immediately updates to show **1** after removing an item.

## Actual Result
The cart badge continues to display **2** after the item is removed. The count only updates after a full page refresh.

---

## Additional Notes
- Reproducible 100% of the time
- Affects all combinations of add/remove actions
- The cart page itself correctly shows the updated item count — only the badge is affected
- **Workaround:** Manually refresh the page after removing an item

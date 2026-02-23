# BUG-004: Price Sort (High to Low) Returns Incorrect Order

## Summary
When sorting products by "Price (high to low)" on the inventory page, the items are not correctly ordered by descending price. Some items appear out of sequence, indicating the sort algorithm is not functioning correctly.

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

> **Justification:** Incorrect sort order misleads users when comparing prices, potentially causing them to miss better-priced options or lose trust in the platform.

---

## Steps to Reproduce
1. Navigate to https://www.saucedemo.com and log in with `standard_user` / `secret_sauce`
2. On the inventory page, locate the sort dropdown (top-right, defaulting to "Name (A to Z)")
3. Select **Price (high to low)** from the dropdown
4. Observe the order of product prices displayed

---

## Expected Result
Products are displayed in strictly descending price order:
- $49.99 → $29.99 → $15.99 → $9.99 → $7.99 → $7.99

## Actual Result
Products are not in correct descending order. Example observed order:
- $49.99 → $15.99 → $29.99 → $9.99 → $7.99 → $7.99

The `$15.99` item appears before the `$29.99` item, which is incorrect.

---

## Additional Notes
- The "Price (low to high)" sort also exhibits similar issues
- "Name (A to Z)" and "Name (Z to A)" sorts appear to work correctly
- Reproducible 100% of the time with the same incorrect order
- **Workaround:** Users must manually verify and compare prices

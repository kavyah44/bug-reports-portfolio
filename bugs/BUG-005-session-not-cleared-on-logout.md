# BUG-005: Session Not Cleared on Logout — Back Button Exposes Previous Session

## Bug ID
BUG-005

## Title
Session Not Cleared on Logout — Back Button Exposes Previous Session

## Severity
🔴 Critical

## Priority
🔴 High

## Status
Open

## Environment
| Field        | Detail                         |
|--------------|-------------------------------|
| Application  | SauceDemo (saucedemo.com)     |
| Browser      | Chrome 121, Firefox 122       |
| OS           | Windows 11, macOS Ventura     |
| Test Account | standard_user                 |
| Date Found   | 2026-02-23                    |
| Reported By  | Kavya H                       |

---

## Summary
After a user logs out of SauceDemo, clicking the browser's **Back** button navigates back to the inventory/products page without requiring re-authentication. The previous session content is fully accessible, representing a critical security vulnerability in session management.

---

## Steps to Reproduce

1. Open https://www.saucedemo.com
2. Log in with username: `standard_user` and password: `secret_sauce`
3. Verify the Products page loads successfully
4. Click the hamburger menu (☰) in the top-left corner
5. Click **Logout**
6. Verify you are redirected to the login page
7. Click the browser **Back** button

**Expected Result:** The application should redirect the user back to the login page and display a session-expired or authentication-required message. The previous session data should not be accessible.

**Actual Result:** The browser navigates back to the Products (inventory) page and all products are fully visible and interactive — without any re-authentication required. The user remains effectively logged in.

---

## Evidence

### Screenshots / Screen Recording
```
Step 6: User is on login page after logout
Step 7: Clicking Back — Products page loads without login prompt
         [Attach screenshot showing inventory page accessible post-logout]
```

### Console / Network Evidence
- No redirect or 401 Unauthorized response is triggered on back-navigation
- Session cookie or token appears to remain valid after logout action

---

## Root Cause (Hypothesis)
The application likely:
- Does not **invalidate the session token server-side** on logout
- Does not clear the **browser history state** to prevent back-navigation
- Does not set **Cache-Control: no-store** headers to prevent caching of authenticated pages

A robust logout implementation should:
1. Invalidate the server-side session/token
2. Clear all client-side session data (cookies, localStorage, sessionStorage)
3. Implement cache-control headers to prevent page caching
4. Redirect to login and verify authentication on each protected page load

---

## Impact

| Impact Area       | Description                                                              |
|-------------------|--------------------------------------------------------------------------|
| Security          | Unauthorized access to user data after logout on shared/public devices   |
| Privacy           | Another person using the same device can access the previous user's data |
| Compliance        | Violates OWASP A07:2021 – Identification and Authentication Failures     |
| User Trust        | Users expect logout to fully terminate their session                     |

### Risk Scenario
A user logs out of a shared computer in a library or office. The next person simply presses the Back button and gains full access to the previous user's session — including personal data, cart contents, and all protected pages.

---

## Workaround
None available to end users. Users on shared devices are at high risk.

---

## Recommended Fix
```javascript
// On logout, clear all session storage
sessionStorage.clear();
localStorage.clear();

// Invalidate cookies
document.cookie.split(";").forEach(cookie => {
  document.cookie = cookie.replace(/^ +/, "")
    .replace(/=.*/, "=;expires=" + new Date().toUTCString() + ";path=/");
});

// Prevent back-navigation to authenticated pages
window.history.pushState(null, null, window.location.href);
window.onpopstate = function () {
  window.history.pushState(null, null, window.location.href);
};
```

Additionally, the server should:
- Invalidate session tokens in the backend on logout
- Return **401 Unauthorized** for all API requests using invalidated tokens
- Set `Cache-Control: no-store, no-cache, must-revalidate` headers on all protected routes

---

## Related Bugs
- BUG-001: Login Error Message Missing (authentication-related)

---

## Test Cases to Add
- [ ] TC-001: Verify back button redirects to login after logout
- [ ] TC-002: Verify session token is invalidated after logout (API check)
- [ ] TC-003: Verify protected pages are inaccessible without authentication
- [ ] TC-004: Verify localStorage/sessionStorage are cleared on logout
- [ ] TC-005: Verify direct URL access to /inventory.html redirects to login when not authenticated

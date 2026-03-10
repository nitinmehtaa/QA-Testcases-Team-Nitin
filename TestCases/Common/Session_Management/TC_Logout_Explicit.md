# Test Case: TC_Logout_Explicit

**Story ID:** 1301 **Feature:** Session Management **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that explicit (manual) logout correctly clears the user session, tokens, and cookies — ensuring no residual authentication state remains after logout.

## Preconditions:
1. User is logged into the application
2. Application is accessible
3. Database is up and running
4. Browser developer tools are available for cookie/storage inspection

## Test Steps:

### Scenario 1: Basic Logout Flow
1. Log in with valid credentials
2. Click the Logout button/link
3. Verify user is redirected to the login page
4. Verify session token/cookie is cleared from browser storage
5. Attempt to navigate to a protected page via browser back button
6. Verify user is NOT able to access protected content

### Scenario 2: Logout Clears Local Storage & Session Storage
1. Log in with valid credentials
2. Open browser DevTools → Application → LocalStorage / SessionStorage
3. Note down all stored tokens/keys
4. Click Logout
5. Check LocalStorage and SessionStorage again
6. Verify all auth-related keys (e.g., `auth_token`, `user_id`, `refresh_token`) are deleted

### Scenario 3: Logout Clears Cookies
1. Log in with valid credentials
2. Open DevTools → Application → Cookies
3. Note session cookie values
4. Click Logout
5. Verify session cookies are expired/deleted
6. Verify HttpOnly cookies are also cleared server-side

### Scenario 4: Logout from Multiple Tabs
1. Log in on Tab A
2. Open the same application in Tab B (same browser)
3. Log out from Tab A
4. Switch to Tab B and try to perform any action (e.g., refresh or click a link)
5. Verify Tab B also redirects to login (session invalidated across tabs)

### Scenario 5: API Token Invalidation on Logout
1. Log in and capture the Bearer token from Network tab
2. Log out from the UI
3. Use Postman/curl to make an API call using the captured token
4. Verify API returns `401 Unauthorized`

### Scenario 6: Logout Button Accessibility
1. Log in as a valid user
2. Verify the logout button is visible and accessible from all major pages (Dashboard, Profile, Settings)
3. Verify keyboard navigation reaches the logout button (Tab key)
4. Verify logout works on mobile/responsive view

## Expected Results:

1. **Basic Logout:** User is redirected to login page; back-button navigation does not expose protected pages
2. **LocalStorage/SessionStorage:** All auth-related keys are completely removed post-logout
3. **Cookies:** Session cookies are deleted/expired; no valid session persists
4. **Multi-tab Logout:** All tabs reflect logged-out state; no cross-tab session leak
5. **API Token:** Previously issued token returns `401 Unauthorized` after logout
6. **Accessibility:** Logout button reachable via keyboard and visible across all pages

## Test Data:
| Field | Value |
|---|---|
| Valid Username | testuser@example.com |
| Valid Password | Test@12345 |
| Protected URL | /dashboard, /profile, /settings |

## Notes:
- Distinguish this from `TC_Session_Timeout_Auto_Logout` — this TC covers **user-initiated** logout only
- Verify CSRF token is also invalidated post-logout
- Check server-side session store (Redis/DB) to confirm session record is deleted

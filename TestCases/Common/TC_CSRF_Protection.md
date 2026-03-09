# Test Case: TC_CSRF_Protection

**Story ID:** 1305 **Feature:** Security - CSRF Protection **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that the application implements proper Cross-Site Request Forgery (CSRF) protection. All state-changing requests must include and validate a unique CSRF token to prevent unauthorized actions on behalf of authenticated users.

## Preconditions:
1. Application is accessible
2. Valid test user account exists
3. User is logged in with an active session
4. Burp Suite or browser DevTools available to inspect request headers and form data

## Test Steps:

### Scenario 1: Verify CSRF Token Presence in Forms
1. Log in as a valid user
2. Navigate to forms that perform state-changing operations:
   - Profile Update form
   - Password Change form
   - Account Settings form
   - Any data submission form
3. Inspect the HTML source of each form (Right-click > View Page Source)
4. Verify a hidden CSRF token field is present (e.g., `<input type="hidden" name="_csrf" value="...">`)
5. Verify the token value is unique per session/request

### Scenario 2: CSRF Token Validated on Form Submission
1. Log in as a valid user
2. Open Profile Update form in DevTools > Network tab
3. Submit the form with valid data
4. Capture the POST request in Network tab
5. Verify the CSRF token is included in the request headers or body
6. Confirm the server responds with 200 OK and accepts the update

### Scenario 3: Request Rejected Without CSRF Token
1. Log in as a valid user
2. Use Burp Suite or Postman to craft a POST request to a state-changing endpoint (e.g., `/profile/update`)
3. Send the request WITHOUT the CSRF token
4. Verify the server returns `403 Forbidden` or `400 Bad Request`
5. Verify no data changes are made in the database

### Scenario 4: Request Rejected With Invalid/Tampered CSRF Token
1. Log in as a valid user
2. Capture a valid POST request (e.g., profile update) using DevTools
3. Modify the CSRF token to an invalid value (e.g., `abc123`)
4. Re-submit the request
5. Verify the server rejects the request with `403 Forbidden`
6. Verify no changes are persisted

### Scenario 5: CSRF Token is Session-Specific
1. Log in as User A and capture the CSRF token from any form
2. Log out and log in as User B
3. Attempt to use User A's CSRF token in a state-changing request for User B's session
4. Verify the request is rejected (CSRF token is not cross-session valid)

### Scenario 6: Simulated CSRF Attack from External Site
1. Create a simple HTML page on a local/test server:
   ```html
   <form action="https://app.example.com/profile/update" method="POST">
     <input type="hidden" name="email" value="attacker@evil.com">
     <input type="submit" value="Click me">
   </form>
   ```
2. Open the page in a browser while logged into the application in another tab
3. Submit the attacker's form
4. Verify the application rejects the request (missing/invalid CSRF token)
5. Verify the user's email is NOT changed to the attacker's value

### Scenario 7: CSRF Protection on API Endpoints
1. Identify all API endpoints that accept POST/PUT/DELETE requests (e.g., `/api/user/update`)
2. Send requests without CSRF token or using `Origin: https://evil.com` header
3. Verify the API checks and enforces CSRF protection or uses token-based auth (Bearer/JWT)
4. Verify API returns 403 for unauthorized cross-origin state-changing requests

## Expected Results:

1. **Token Presence:** All state-changing forms contain a hidden CSRF token field
2. **Token Validation:** Server validates CSRF token on every POST/PUT/DELETE request
3. **Missing Token:** Request rejected with 403 without CSRF token
4. **Invalid Token:** Request rejected with 403 for tampered/invalid CSRF token
5. **Session-Specific:** CSRF tokens are bound to sessions and not interchangeable
6. **Simulated Attack:** External form submission fails; no unauthorized data change
7. **API Endpoints:** CSRF protection enforced on all state-changing API calls

## Test Data:
| Field | Value |
|---|---|
| Valid User | testuser@example.com / Test@12345 |
| State-Changing Endpoints | /profile/update, /settings, /password/change |
| Tampered Token | `abc123fake` |
| Attacker Origin | `https://evil-site.com` |

## Notes:
- CSRF protection is required for all cookie-based authentication sessions
- APIs using stateless JWT Bearer tokens in Authorization header are generally not vulnerable to CSRF
- Verify `SameSite=Strict` or `SameSite=Lax` attribute is set on session cookies as an additional defense
- Double Submit Cookie pattern is an acceptable alternative to synchronizer tokens
- All CSRF token failures should be logged in the security audit log

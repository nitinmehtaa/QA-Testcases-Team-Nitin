# Test Case: TC_Authorization_Bypass

**Story ID:** 1306 **Feature:** Security - Access Control **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that users cannot bypass authorization controls to access resources, data, or functionality that belong to other users or require elevated privileges. This covers Insecure Direct Object Reference (IDOR) and horizontal/vertical privilege escalation.

## Preconditions:
1. At least two test user accounts exist with different roles (e.g., User A = regular user, User B = regular user, Admin = admin)
2. Each user has unique resources (profile, orders, documents, etc.)
3. Application is accessible
4. DevTools or Postman available for URL/request manipulation

## Test Steps:

### Scenario 1: IDOR - Accessing Another User's Profile via URL Manipulation
1. Log in as User A
2. Navigate to User A's profile page (e.g., `/profile?id=101`)
3. Note the user ID in the URL
4. Modify the URL to use User B's ID (e.g., `/profile?id=102`)
5. Verify User A CANNOT view User B's private profile data
6. Verify the application returns `403 Forbidden` or redirects to an error page

### Scenario 2: IDOR - Accessing Another User's Orders/Documents
1. Log in as User A
2. Navigate to a specific order/document (e.g., `/orders/555`)
3. Modify the order ID in the URL to another user's order (e.g., `/orders/556`)
4. Verify User A CANNOT access User B's order details
5. Verify appropriate error response (403 / 404 / redirect)

### Scenario 3: Horizontal Privilege Escalation - Edit Another User's Data
1. Log in as User A
2. Capture the profile update request in DevTools/Postman
3. Modify the user ID parameter in the request body/URL to User B's ID
4. Send the modified request
5. Verify the update is rejected; User A CANNOT modify User B's data
6. Verify no data changes are made to User B's account

### Scenario 4: Vertical Privilege Escalation - Regular User Accessing Admin Endpoints
1. Log in as a regular user (non-admin)
2. Attempt to navigate to admin-only URLs directly:
   - `/admin/dashboard`
   - `/admin/users`
   - `/admin/settings`
   - `/admin/reports`
3. Verify each page returns `403 Forbidden` or redirects to a non-admin landing page
4. Verify no admin data is visible to the regular user

### Scenario 5: Forced Browsing - Accessing Pages Without Authentication
1. Log out of the application
2. Attempt to directly navigate to authenticated pages:
   - `/dashboard`
   - `/profile/edit`
   - `/settings`
   - `/orders`
3. Verify the user is redirected to the login page for each URL
4. Verify no protected data is accessible without authentication

### Scenario 6: Manipulating Role Parameter in API Request
1. Log in as a regular user
2. Capture any API request that includes a `role` or `isAdmin` parameter
3. Modify the value to `admin` or `true` (e.g., `{"role": "admin"}`)
4. Send the modified API request
5. Verify the server ignores the client-supplied role and uses the server-side role
6. Verify no privilege escalation occurs

### Scenario 7: Bypassing Authorization via Parameter Tampering
1. Log in as a regular user
2. Identify any API call with access control parameters (e.g., `?access=read` or `?level=1`)
3. Modify the parameter to gain higher access (e.g., `?access=write` or `?level=99`)
4. Verify the application enforces server-side authorization and rejects the manipulation

## Expected Results:

1. **IDOR - Profile:** User A cannot view User B's private profile; 403 returned
2. **IDOR - Orders:** User A cannot access User B's orders; 403/404 returned
3. **Horizontal Escalation:** User A cannot modify User B's data via request manipulation
4. **Vertical Escalation:** Regular user cannot access admin pages; 403 returned
5. **Forced Browsing:** Unauthenticated access to protected pages redirects to login
6. **Role Parameter Tampering:** Server-side role enforcement prevents escalation
7. **Parameter Tampering:** Access control parameters are validated server-side

## Test Data:
| Field | Value |
|---|---|
| User A | usera@example.com / Test@12345 |
| User B | userb@example.com / Test@12345 |
| Admin User | admin@example.com / Admin@12345 |
| User A ID | 101 |
| User B ID | 102 |
| Admin URLs | /admin/dashboard, /admin/users |

## Notes:
- IDOR is consistently in the OWASP Top 10 (Broken Access Control - A01:2021)
- Authorization checks MUST be enforced server-side, never rely on client-side checks alone
- Ensure object-level, field-level, and function-level access control are all tested
- Log all unauthorized access attempts in the security audit log
- Use sequential/guessable IDs with caution; consider UUIDs for resource identifiers

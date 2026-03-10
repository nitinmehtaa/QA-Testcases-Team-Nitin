# Test Case: TC_SQL_Injection

**Story ID:** 1303 **Feature:** Security - Input Validation **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that all user-facing input fields are protected against SQL Injection attacks. The application must sanitize and validate all inputs to prevent unauthorized database access, data leakage, or data manipulation.

## Preconditions:
1. Application is accessible
2. Test user account exists
3. Tester has access to login, search, registration, and profile pages
4. Burp Suite or browser DevTools available for request inspection (optional)

## Test Steps:

### Scenario 1: SQL Injection in Login - Username Field
1. Navigate to the login page
2. Enter the following payloads in the Username field (one at a time):
   - `' OR '1'='1`
   - `' OR '1'='1' --`
   - `admin'--`
   - `' OR 1=1--`
   - `" OR ""="`
3. Enter any value in the Password field
4. Click Login
5. Verify the application does NOT log in without valid credentials
6. Verify an appropriate error message is shown (e.g., "Invalid credentials")
7. Verify no database error or stack trace is exposed

### Scenario 2: SQL Injection in Login - Password Field
1. Navigate to the login page
2. Enter a valid username
3. Enter the following payloads in the Password field:
   - `' OR '1'='1`
   - `password' OR '1'='1`
   - `anything' OR 'x'='x`
4. Click Login
5. Verify authentication fails
6. Verify no sensitive DB information is returned in the response

### Scenario 3: SQL Injection in Search Functionality
1. Navigate to the Search feature
2. Enter the following payloads in the search box:
   - `' UNION SELECT null, username, password FROM users--`
   - `%' AND 1=1--`
   - `1; DROP TABLE users--`
3. Submit the search
4. Verify no database records are returned unexpectedly
5. Verify no table is dropped or modified
6. Verify the application returns a safe error or empty result

### Scenario 4: SQL Injection in Registration Form
1. Navigate to the Registration page
2. Enter SQL payloads in:
   - First Name: `Robert'); DROP TABLE users;--`
   - Email: `test@test.com' OR '1'='1`
   - Username: `admin'--`
3. Submit the form
4. Verify the registration fails gracefully or accepts only sanitized input
5. Verify no unintended records are created in the database

### Scenario 5: SQL Injection in Profile Update Form
1. Log in as a valid user
2. Navigate to Profile settings
3. Enter SQL payloads in editable fields (bio, address, display name, etc.):
   - `'; UPDATE users SET role='admin' WHERE '1'='1`
   - `test'; SELECT * FROM users--`
4. Save the changes
5. Verify the application rejects or sanitizes the payload
6. Verify no privilege escalation occurs

### Scenario 6: Blind SQL Injection via URL Parameter
1. Observe URL patterns (e.g., `/profile?id=1`)
2. Modify the URL parameter to:
   - `/profile?id=1 AND 1=1`
   - `/profile?id=1 AND 1=2`
   - `/profile?id=1; DROP TABLE sessions--`
3. Verify both `1=1` and `1=2` return the same safe response
4. Verify no server error (500) is triggered
5. Verify the database is not affected

## Expected Results:

1. **Login Fields:** All SQL payloads are rejected; no unauthorized login occurs; no DB info exposed
2. **Password Field:** Authentication fails; no sensitive data returned
3. **Search:** No unintended records returned; no table manipulation possible
4. **Registration:** Payloads are sanitized or rejected; no unintended DB records created
5. **Profile Update:** Input sanitized; no privilege escalation
6. **URL Parameters:** Application returns consistent safe response; no DB errors exposed

## Test Data:
| Payload | Type |
|---|---|
| `' OR '1'='1` | Classic Auth Bypass |
| `admin'--` | Comment Injection |
| `1; DROP TABLE users--` | Destructive Injection |
| `' UNION SELECT null,username,password FROM users--` | UNION-based Injection |
| `Robert'); DROP TABLE users;--` | Stacked Queries |

## Notes:
- All inputs must be parameterized/prepared statements server-side
- WAF (Web Application Firewall) rules should block these if configured
- Log all injection attempts in the security audit log
- Automated SQL injection scan can also be done using OWASP ZAP or SQLMap in a controlled environment
- Never run destructive payloads (`DROP`, `DELETE`) on production databases

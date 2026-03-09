# Test Case: TC_XSS_Cross_Site_Scripting

**Story ID:** 1304 **Feature:** Security - Input Validation **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that the application is protected against Cross-Site Scripting (XSS) attacks. All user-supplied input must be properly escaped/encoded before rendering in the browser to prevent script execution.

## Preconditions:
1. Application is accessible
2. Test user account exists
3. Tester has access to all input forms (login, registration, profile, search, comments)
4. Browser DevTools available for DOM inspection

## Test Steps:

### Scenario 1: Reflected XSS - Search Field
1. Navigate to the Search page
2. Enter the following payloads in the search box:
   - `<script>alert('XSS')</script>`
   - `<img src=x onerror=alert('XSS')>`
   - `<svg onload=alert(1)>`
   - `javascript:alert('XSS')`
3. Submit the search
4. Verify no alert popup appears
5. Verify the script tags are rendered as plain text or escaped in the HTML output
6. Check page source to confirm encoding (e.g., `&lt;script&gt;`)

### Scenario 2: Stored XSS - Profile/Bio Field
1. Log in as a valid user
2. Navigate to Profile settings
3. Enter XSS payloads in the Bio/Description/Display Name fields:
   - `<script>alert('StoredXSS')</script>`
   - `<img src=x onerror=alert('StoredXSS')>`
   - `<a href=javascript:void(0) onclick=alert('XSS')>Click Me</a>`
4. Save the profile
5. Log in as a different user and view the first user's profile
6. Verify no script executes when viewing the page
7. Verify the payload is displayed as plain/escaped text

### Scenario 3: DOM-Based XSS - URL Parameter
1. Identify pages that reflect URL parameters in the DOM (e.g., `/welcome?name=John`)
2. Modify the URL parameter to:
   - `/welcome?name=<script>alert('DOM-XSS')</script>`
   - `/welcome?name=<img src=x onerror=alert(1)>`
3. Navigate to the modified URL
4. Verify no alert executes
5. Verify the input is sanitized before being written to the DOM

### Scenario 4: XSS in Registration Form Fields
1. Navigate to the Registration page
2. Enter XSS payloads in:
   - First Name: `<script>alert('XSS')</script>`
   - Last Name: `<img src=x onerror=alert(1)>`
   - Username: `<svg onload=alert(1)>`
   - Email: `xss@test.com<script>alert(1)</script>`
3. Submit the form
4. Verify the payloads are rejected or sanitized
5. Verify no script executes on any page that displays this user's name

### Scenario 5: XSS in Comment/Feedback Fields
1. Navigate to any comment or feedback section
2. Submit a comment with:
   - `<b onmouseover=alert('XSS')>Hover here</b>`
   - `<script>document.cookie</script>`
   - `<iframe src=javascript:alert('XSS')></iframe>`
3. View the submitted comment (as the same or different user)
4. Verify no script executes; content is displayed safely

### Scenario 6: XSS in HTTP Headers (Advanced)
1. Use Burp Suite/Postman to send requests with XSS payloads in headers:
   - `User-Agent: <script>alert(1)</script>`
   - `Referer: javascript:alert(1)`
2. Verify the application does not render these values unsafely
3. Verify no script executes if headers are reflected in any response page

## Expected Results:

1. **Reflected XSS:** Payload is not executed; search results show escaped text
2. **Stored XSS:** Payload stored as plain text; no script executed when viewed by any user
3. **DOM-Based XSS:** URL parameters are sanitized before DOM insertion; no alert triggered
4. **Registration Form:** XSS payloads are rejected or encoded; no execution on any page
5. **Comment Fields:** Comments with scripts are stored/displayed safely without execution
6. **HTTP Headers:** Header values with scripts are not rendered in unsafe contexts

## Test Data:
| Payload | XSS Type |
|---|---|
| `<script>alert('XSS')</script>` | Basic Reflected/Stored |
| `<img src=x onerror=alert(1)>` | Event Handler |
| `<svg onload=alert(1)>` | SVG-based |
| `javascript:alert('XSS')` | Protocol Handler |
| `<iframe src=javascript:alert(1)>` | iframe Injection |
| `<b onmouseover=alert(1)>text</b>` | Mouse Event |

## Notes:
- Use Content Security Policy (CSP) headers as a defense-in-depth measure
- All output should be HTML-encoded before rendering
- Verify `httpOnly` and `secure` flags on cookies to mitigate XSS cookie theft
- Use OWASP ZAP or Burp Suite scanner for comprehensive automated XSS testing
- Check both GET and POST parameters for all scenarios

# TC_Two_Factor_Authentication

**Story ID:** STR-005  
**Feature:** Security - Two-Factor Authentication  
**Priority:** High  
**Owner:** QA Team  
**State:** Active  
**Sprint:** Sprint 2  
**Regression:** Yes

---

## Preconditions

1. User must have a valid account with username and password
2. User must have access to their registered email or phone number
3. Two-Factor Authentication (2FA) must be enabled in the system
4. User's account must have 2FA activated
5. Authentication application (e.g., Google Authenticator, Authy) or access to SMS/Email must be available

---

## Test Steps

1. Navigate to the login page of the application
2. Enter valid username in the "Username" field
3. Enter valid password in the "Password" field
4. Click on the "Login" button
5. Verify that the 2FA verification page is displayed
6. Enter the verification code received via the configured 2FA method (SMS/Email/Authentication App)
7. Click on the "Verify" or "Submit" button
8. Verify that the user is successfully logged in
9. Navigate to the user dashboard or homepage

---

## Expected Results

1. Login page should be accessible and display username and password fields
2. Username should be accepted and displayed (masked or visible based on configuration)
3. Password should be accepted and displayed as masked characters
4. After clicking "Login", the system should proceed to 2FA verification page without logging the user in
5. 2FA verification page should display:
   - A field to enter the verification code
   - Information about where the code was sent (e.g., "Code sent to \*\*\*\*@email.com" or "Enter code from your authenticator app")
   - A "Verify" or "Submit" button
   - Optional: "Resend Code" or "Use different method" options
6. System should accept the verification code input
7. After clicking "Verify":
   - If the code is correct: User should be successfully authenticated and logged in
   - If the code is incorrect: Error message should be displayed (e.g., "Invalid verification code")
   - If the code is expired: Appropriate error message should be shown
8. Upon successful authentication, user should see:
   - User dashboard or homepage
   - User profile information or welcome message
   - Confirmation that 2FA authentication was successful
9. User should remain on the authenticated page with full access to authorized features

---

## Additional Test Scenarios

### Negative Scenarios:
- Invalid verification code: System should reject and display error message
- Expired verification code: System should prompt to resend code
- Maximum attempts exceeded: Account should be temporarily locked
- Network interruption during verification: Appropriate error handling should occur

### Edge Cases:
- Code resend functionality: Verify user can request a new code
- Different 2FA methods: Test switching between SMS, Email, and Authenticator App
- Remember device option: If available, test "Don't ask again on this device" functionality
- Backup codes: Test authentication using backup recovery codes

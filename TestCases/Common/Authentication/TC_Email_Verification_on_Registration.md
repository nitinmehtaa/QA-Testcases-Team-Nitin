# Test Case: Email Verification on Registration

## Test Case Information

| Field | Details |
|-------|--------|
| **Story ID** | US-2024-102 |
| **Feature** | User Registration - Email Verification |
| **Priority** | High |
| **Owner** | QA Team |
| **State** | Ready for Review |
| **Sprint** | Sprint 25 |
| **Regression** | Yes |

---

## Preconditions

1. The application registration page is accessible
2. Email service is configured and operational
3. Database is accessible for user account verification
4. Test email account is available for verification testing
5. Network connectivity is stable
6. SMTP server is properly configured
7. No existing user account with the test email address

---

## Test Steps

### Scenario 1: Successful Email Verification Flow

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Navigate to the registration page | Registration form is displayed with all required fields |
| 2 | Enter valid user details (username, email, password, confirm password) | All fields accept input without validation errors |
| 3 | Click on "Register" or "Sign Up" button | Registration success message is displayed; user is informed to check email for verification link |
| 4 | Check the registered email inbox | Verification email is received within 2 minutes |
| 5 | Open the verification email | Email contains verification link, clear instructions, and sender information |
| 6 | Click on the verification link in the email | User is redirected to the application with email verification success message |
| 7 | Attempt to log in with the registered credentials | Login is successful and user gains full access to the application |
| 8 | Verify user account status in database | User account status is marked as "email_verified" or "active" |

### Scenario 2: Email Receipt Verification

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Complete registration with a valid email | Verification email is sent |
| 2 | Verify email subject line | Subject clearly indicates "Email Verification" or "Verify Your Account" |
| 3 | Check email sender information | Email is from official domain (e.g., noreply@company.com) |
| 4 | Review email body content | Email contains personalized greeting, clear call-to-action, verification link, and expiration information |
| 5 | Verify email formatting | Email is properly formatted with HTML/plain text fallback |
| 6 | Check for alternative verification method | Email includes verification code or alternative method if link doesn't work |

### Scenario 3: Invalid Verification Link

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Complete registration process | Verification email is received |
| 2 | Modify the verification link (change token/parameters) | Modified link is created |
| 3 | Click on the modified/invalid verification link | User is redirected to error page with message "Invalid verification link" |
| 4 | Attempt to log in without verification | Login is blocked with message "Please verify your email first" |
| 5 | Check if resend verification option is available | Option to resend verification email is displayed |

### Scenario 4: Expired Verification Link

| Step | Action | Expected Result |
|------|--------|----------------|
| 1 | Complete registration process | Verification email is received |
| 2 | Wait for the verification link to expire (based on configured expiration time) | Link expiration period passes |
| 3 | Click on the expired verification link | User is redirected to page with message "Verification link has expired" |
| 4 | Verify resend verification link option | "Resend verification email" option is prominently displayed |
| 5 | Click on "Resend verification email" | New verification email is sent successfully |
| 6 | Click on the new verification link | Email verification is completed successfully |

---

## Expected Results

### Positive Test Results:

1. **Registration Success:**
   - User account is created in pending/unverified state
   - Confirmation message displayed on registration page
   - User is redirected appropriately

2. **Email Delivery:**
   - Verification email arrives within acceptable timeframe (< 2 minutes)
   - Email is not flagged as spam
   - Email contains all necessary information and branding

3. **Verification Link Functionality:**
   - Link is clickable and properly formatted
   - Link redirects to correct verification endpoint
   - Token is validated correctly
   - Single-use token prevents reuse

4. **Account Activation:**
   - User account status changes from unverified to verified
   - User gains access to all features
   - Welcome message or onboarding flow initiates
   - Database records are updated correctly

5. **User Experience:**
   - Clear instructions at each step
   - Appropriate success/error messages
   - Smooth navigation flow
   - Responsive design on all devices

### Negative Test Results:

1. **Invalid Link Handling:**
   - Tampered links are rejected
   - Clear error message displayed
   - No system errors or crashes
   - User is directed to resend option

2. **Expired Link Handling:**
   - Expired links are properly detected
   - Appropriate expiration message shown
   - Resend functionality works correctly
   - New token is generated successfully

3. **Security Measures:**
   - Tokens are cryptographically secure
   - One-time use is enforced
   - Rate limiting prevents abuse
   - No sensitive data exposed in URLs

---

## Additional Test Scenarios

### Negative Scenarios:

1. **Duplicate Registration:**
   - Attempt to register with already registered email
   - Expected: Error message "Email already registered" displayed

2. **Invalid Email Format:**
   - Enter invalid email format during registration
   - Expected: Validation error prevents submission

3. **Missing Email Configuration:**
   - Simulate email service outage
   - Expected: Graceful error handling with retry mechanism or notification

4. **Multiple Verification Attempts:**
   - Click verification link multiple times
   - Expected: First click succeeds, subsequent attempts show "Already verified" message

5. **Unregistered Verification Token:**
   - Use a random/non-existent token
   - Expected: Invalid token error message

6. **Email Service Timeout:**
   - Simulate slow email delivery
   - Expected: System continues functioning; email queues properly

### Edge Cases:

1. **Special Characters in Email:**
   - Register with email containing special characters (e.g., user+test@domain.com)
   - Expected: Email is sent and received correctly

2. **Concurrent Registrations:**
   - Register multiple accounts simultaneously
   - Expected: Each receives unique verification email

3. **Browser Session Closure:**
   - Close browser after registration, before email verification
   - Expected: Verification link still works when accessed later

4. **Different Device/Browser:**
   - Register on one device, verify email on another
   - Expected: Verification succeeds regardless of device

5. **Network Interruption:**
   - Simulate network interruption during verification click
   - Expected: Appropriate error message; link remains valid

6. **Multiple Email Resend Requests:**
   - Request resend verification email multiple times
   - Expected: Rate limiting prevents spam; only latest token is valid

7. **Long Delay Before Verification:**
   - Wait extended period before clicking verification link
   - Expected: Link expires appropriately; clear resend option available

8. **Email Client Issues:**
   - Test with various email clients (Gmail, Outlook, Yahoo, etc.)
   - Expected: Email renders correctly in all major clients

---

## Test Data

| Field | Valid Data | Invalid Data |
|-------|-----------|-------------|
| Email | test@example.com | test@, @example.com, test.example.com |
| Username | testuser123 | (empty), special@#$%, < script > |
| Password | Pass@1234 | 123, password (weak) |
| Verification Link | Valid token from email | Modified token, expired token, null |

---

## Notes

- Verification link expiration time should be configurable (recommended: 24-48 hours)
- Email templates should be internationalized for multi-language support
- Monitor email delivery rates and bounce rates
- Implement proper logging for debugging verification issues
- Consider implementing backup verification methods (SMS, code-based)
- Ensure GDPR compliance for email data handling
- Test with various email providers to ensure deliverability

---

## References

- Related Test Cases: TC_User_Login.md, TC_Password_Reset.md
- Security Standards: OWASP Authentication Guidelines
- Email Best Practices: RFC 5321, RFC 5322
- Previous Coverage: Initial implementation tested in Sprint 15, regression added in Sprint 20

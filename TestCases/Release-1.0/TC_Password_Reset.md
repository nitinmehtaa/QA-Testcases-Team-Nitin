# Test Case: TC_Password_Reset
**Story ID:** 1237
**Feature:** User Authentication
**Priority:** High
**Linked Bug IDs:** 5681
**Owner:** Nitin Mehta
**State:** Manual
**Sprint:** S-01
**Regression:** Yes

**Preconditions:**
1. Valid user account exists in the system
2. Password reset functionality is enabled
3. Email service is configured and working
4. User has access to registered email account

**Test Steps:**
1. Navigate to login page
2. Click on "Forgot Password" link
3. Enter registered email address
4. Submit password reset request
5. Check email for reset link
6. Click on password reset link
7. Enter new password meeting requirements:
   a. Minimum 8 characters
   b. At least one uppercase letter
   c. At least one number
   d. At least one special character
8. Confirm new password
9. Submit password change
10. Attempt to login with new password

**Expected Results:**
1. Password reset email is sent within 5 minutes
2. Reset link in email is valid and working
3. Password requirements are clearly displayed
4. System accepts valid new password
5. Confirmation message shown after successful reset
6. Old password no longer works
7. Login with new password is successful

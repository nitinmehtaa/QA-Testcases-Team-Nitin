# Test Case: TC_Password_Reset  
**Story ID:** 3546  
**Feature:** Password Reset via Email  
**Priority:** High  
**Linked Bug IDs:** 5666  
**Owner:** Nitin Mehta  
**State:** Manual  
**Sprint:** S-01  
**Regression:** Yes

**Preconditions:**
1. The user must have a registered email address.
2. Password reset functionality is accessible.

**Test Steps:**  
1. Navigate to the login page.
2. Click on the "Forgot Password" link.
3. Enter a valid registered email address.
4. Click on the "Reset Password" button.
5. Check the email inbox for a password reset link.
6. Click on the reset link provided in the email.
7. Enter a new valid password and confirm the password.
8. Click "Submit." 

**Expected Result:**  
The user receives a password reset email. Upon clicking the reset link, they can successfully reset their password and are redirected to a confirmation page.
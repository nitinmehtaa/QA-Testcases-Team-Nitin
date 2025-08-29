# Test Case: TC_Account_Lockout  
**Story ID:** 1236  
**Feature:** Account Security  
**Priority:** High  
**Linked Bug IDs:** 5680  
**Owner:** Nitin Mehta  
**State:** Manual  
**Sprint:** S-01  
**Regression:** Yes

**Preconditions:**
1. Valid user account exists in the system.
2. Login URL is accessible.
3. Account lockout threshold is configured (e.g., 5 attempts).

**Test Steps:**  
1. Navigate to the login page.  
2. Enter valid email address.
3. Enter incorrect password.
4. Click "Login."
5. Repeat steps 2-4 until account lockout threshold is reached.
6. Attempt to login with correct credentials.
7. Wait for lockout period to expire.
8. Try logging in with correct credentials.

**Expected Result:**  
1. System shows "Invalid credentials" for each failed attempt.
2. After reaching threshold, system displays "Account locked" message.
3. Login attempts are blocked during lockout period.
4. After lockout period expires, user can successfully login with correct credentials.
5. Security notification email is sent to user about account lockout.
# Test Case: TC_Login_Invalid  
**Story ID:** 1235  
**Feature:** Login Functionality  
**Priority:** High  
**Linked Bug IDs:** 5679  
**Owner:** Nitin Mehta  
**State:** Manual  
**Sprint:** S-01  
**Regression:** Yes

**Preconditions:**
1. Login URL is accessible.
2. Test account with invalid credentials available.

**Test Steps:**  
1. Navigate to the login page.  
2. Enter invalid email format.  
3. Enter any password.  
4. Click "Login."  
5. Clear fields and enter valid email format with wrong password.
6. Click "Login."
7. Clear fields and enter non-existent email with any password.
8. Click "Login."

**Expected Result:**  
1. For invalid email format: System shows "Invalid email format" error message.
2. For wrong password: System shows "Invalid credentials" error message.
3. For non-existent email: System shows "Invalid credentials" error message.
4. User remains on login page for all attempts.
5. No user session is created.
# Test Case: TC_User_Login
**Story ID:** 1239
**Feature:** User Authentication
**Priority:** High
**Owner:** Nitin Mehta
**State:** Manual
**Sprint:** S-01
**Regression:** Yes

**Preconditions:**
1. User account exists in the system
2. Application is accessible
3. Database is up and running

**Test Steps:**
1. Navigate to login page
2. Test valid login:
   a. Enter valid username/email
   b. Enter correct password
   c. Click login button
3. Test invalid login:
   a. Enter invalid username
   b. Enter invalid password
   c. Click login button
4. Test empty fields:
   a. Leave username empty
   b. Leave password empty
   c. Click login button
5. Test password masking
6. Test remember me functionality
7. Test logout functionality

**Expected Results:**
1. Valid login:
   - User successfully logs in
   - Redirected to dashboard
   - Session is created
2. Invalid login:
   - Error message displayed
   - User remains on login page
   - Appropriate error messaging
3. Empty fields:
   - Validation messages shown
   - Submit button disabled
4. Password field shows masked characters
5. Remember me keeps user logged in
6. Logout clears session properly

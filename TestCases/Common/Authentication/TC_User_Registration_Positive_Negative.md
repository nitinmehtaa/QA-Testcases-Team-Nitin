# Test Case: TC_User_Registration_Positive_Negative

**Story ID:** 1240 **Feature:** User Registration **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-01 **Regression:** Yes

**Preconditions:**
1. Application is accessible
2. Database is up and running
3. Email service is configured

**Test Steps:**

### Positive Scenarios:
1. Navigate to Registration page
2. Enter valid first name, last name, email, and password
3. Confirm password correctly
4. Accept terms and conditions
5. Click Register button
6. Verify confirmation email is sent
7. Verify user is redirected to success/login page

### Negative Scenarios:
1. Register with already existing email
2. Register with invalid email format (e.g., test@)
3. Register with mismatched passwords
4. Register with empty required fields
5. Register with password below minimum length
6. Register with special characters in name fields

**Expected Results:**

**Positive:**
- User account is created successfully
- Confirmation email sent to registered email
- User redirected to appropriate page
- User can log in with new credentials

**Negative:**
- Duplicate email: Error message "Email already registered"
- Invalid email format: Validation error shown inline
- Mismatched passwords: Error "Passwords do not match"
- Empty fields: Validation messages for all required fields
- Weak password: Password strength indicator and error message
- Special characters in name: Validation error or sanitized input

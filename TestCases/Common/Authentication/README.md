# Authentication Test Cases

This folder contains all test cases related to **User Authentication** features.

## Test Cases

| TC ID | File | Description | Priority | Regression |
|---|---|---|---|---|
| TC_User_Login | [TC_User_Login.md](./TC_User_Login.md) | Valid/invalid login, empty fields, password masking | High | Yes |
| TC_User_Registration | [TC_User_Registration_Positive_Negative.md](./TC_User_Registration_Positive_Negative.md) | Registration with valid/invalid data, duplicate accounts | High | Yes |
| TC_Email_Verification | [TC_Email_Verification_on_Registration.md](./TC_Email_Verification_on_Registration.md) | Email verification flow post-registration | Medium | Yes |
| TC_Two_Factor_Authentication | [TC_Two_Factor_Authentication.md](./TC_Two_Factor_Authentication.md) | 2FA enable, disable, OTP validation | High | Yes |
| TC_Password_Complexity_Expiry | [TC_Password_Complexity_Expiry.md](./TC_Password_Complexity_Expiry.md) | Password rules, strength validation, expiry policy | High | Yes |
| TC_Role_Based_Access_Control | [TC_Role_Based_Access_Control.md](./TC_Role_Based_Access_Control.md) | Role assignment, permission enforcement, access boundaries | High | Yes |

## Coverage Areas
- Login (valid, invalid, empty, masked password)
- Registration (positive and negative scenarios)
- Email verification on sign-up
- Two-factor authentication (TOTP/OTP)
- Password complexity rules and expiry
- Role-based access control (Admin, User, Viewer)

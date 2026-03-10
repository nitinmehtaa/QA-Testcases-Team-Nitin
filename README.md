# QA Test Cases Repository

## Repository Structure

```
QA-Testcases-Team-Nitin/
|
├── README.md
|
├── TestCases/
|   ├── Common/                          # Cross-release, reusable test cases
|   |   ├── Authentication/              # Login, Registration, 2FA, Password, Email Verification
|   |   |   ├── TC_User_Login.md
|   |   |   ├── TC_User_Registration_Positive_Negative.md
|   |   |   ├── TC_Two_Factor_Authentication.md
|   |   |   ├── TC_Password_Complexity_Expiry.md
|   |   |   ├── TC_Email_Verification_on_Registration.md
|   |   |   └── README.md
|   |   |
|   |   ├── Session_Management/          # Session timeout, Logout, Concurrent sessions
|   |   |   ├── TC_Session_Timeout_Auto_Logout.md
|   |   |   ├── TC_Logout_Explicit.md
|   |   |   ├── TC_Concurrent_Session_Handling.md
|   |   |   └── README.md
|   |   |
|   |   ├── Security/                    # SQL Injection, XSS, CSRF, Authorization, RBAC
|   |   |   ├── TC_SQL_Injection.md
|   |   |   ├── TC_XSS_Cross_Site_Scripting.md
|   |   |   ├── TC_CSRF_Protection.md
|   |   |   ├── TC_Authorization_Bypass.md
|   |   |   ├── TC_Role_Based_Access_Control.md
|   |   |   └── README.md
|   |   |
|   |   ├── API/                         # API auth tokens, CRUD operations, HTTP error codes
|   |   |   ├── TC_API_Authentication_Token.md
|   |   |   ├── TC_API_CRUD_Operations.md
|   |   |   ├── TC_API_HTTP_Error_Codes.md
|   |   |   └── README.md
|   |   |
|   |   └── General/                     # General UI / functional test cases
|   |       ├── TC_Search_Functionality.md
|   |       └── README.md
|   |
|   └── Release-1.0/                     # Release-specific test cases
|       ├── TC_Login_Valid.md
|       ├── TC_Login_Invalid.md
|       ├── TC_Password_Reset.md
|       ├── TC_Account_Lockout.md
|       └── TC_User_Profile_Update.md
|
├── Attachments/
|   ├── execution_logs/
|   |   └── Release-1.0/
|   └── screenshots/
|       └── Release-1.0/
|
└── ExecutionResults/
    └── Release-1.0/
```

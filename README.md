# QA Test Cases Repository

## Overview

This repository contains a comprehensive collection of test cases for QA validation, organised by release versions and common test categories. The test cases are structured to facilitate easy access, execution tracking, and result documentation.

Maintained by **Team Nitin** | Framework: **Selenium / Playwright** | Language: **Java**

---

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

---

## Test Case Categories

### Common Test Cases (Cross-Release)

| Category | # TCs | Description |
|---|---|---|
| **Authentication** | 5 | User login, registration, 2FA, password policy, email verification |
| **Session Management** | 3 | Session timeout, explicit logout, concurrent session handling |
| **Security** | 5 | SQL injection, XSS, CSRF protection, authorization bypass, RBAC |
| **API** | 3 | API token auth, CRUD operations, HTTP error code validation |
| **General** | 1 | Search functionality |

### Release-1.0 Test Cases

| File | Description |
|---|---|
| TC_Login_Valid.md | Valid login with correct credentials |
| TC_Login_Invalid.md | Invalid login scenarios (wrong password, locked user, etc.) |
| TC_Password_Reset.md | Password reset flow via email |
| TC_Account_Lockout.md | Account lockout after repeated failed login attempts |
| TC_User_Profile_Update.md | User profile update and save functionality |

---

## Test Case Structure

Each test case file includes the following metadata:

| Field | Description |
|---|---|
| **Test Case ID** | Unique identifier (e.g., `TC_User_Login`) |
| **Story ID** | Linked user story / requirement |
| **Feature** | Functionality being tested |
| **Priority** | High / Medium / Low |
| **Owner** | Person responsible for the test case |
| **State** | Manual / Automated |
| **Sprint** | Sprint in which TC is planned |
| **Regression** | Yes / No — whether it is part of regression suite |
| **Preconditions** | Setup required before executing the test |
| **Test Steps** | Step-by-step execution instructions |
| **Expected Results** | Expected outcome for each scenario |
| **Linked Bugs** | Any associated bugs or issues |

---

## Folder Descriptions

- **TestCases/Common/**: Reusable test cases applicable across all releases, organised into category subfolders (`Authentication`, `Session_Management`, `Security`, `API`, `General`).
- **TestCases/Release-1.0/**: Release-specific test cases tied to the first production release.
- **Attachments/execution_logs/**: Execution log files for each test run, organised by release.
- **Attachments/screenshots/**: Screenshots captured during test execution for evidence, organised by release.
- **ExecutionResults/**: Test execution result summaries (Pass/Fail status, remarks) organised by release.

---

## How to Contribute

1. **Fork the Repository**: Fork this repo to your GitHub account.
2. **Create a branch**: Use naming convention `feature/<description>` or `fix/<description>`.
3. **Add / Update Test Cases**: Place new TCs in the appropriate category subfolder under `TestCases/Common/` or the relevant release folder.
4. **Update Attachments**: After execution, add logs to `Attachments/execution_logs/` and screenshots to `Attachments/screenshots/`.
5. **Submit a Pull Request**: Raise a PR for review and integration into `main`.

---

## Getting Started

### Prerequisites

- Git installed on your local machine
- Clone the repository:
  ```bash
  git clone https://github.com/nitinmehtaa/QA-Testcases-Team-Nitin.git
  ```
- Set up your test execution environment (Selenium / Playwright / JUnit / TestNG as applicable).

### Running Tests

Test execution depends on your team's framework setup. Refer to the relevant test runner configuration in your project. Execution results should be saved under `ExecutionResults/` and logs/screenshots under `Attachments/`.

---

## License

This project is maintained by **Team Nitin** for internal QA purposes.

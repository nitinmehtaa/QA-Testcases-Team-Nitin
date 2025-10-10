# QA Test Cases Repository

## Overview
This repository contains a collection of test cases for QA validation, grouped by release versions and common test scenarios. The test cases are organized to facilitate easy access, execution tracking, and result documentation.

## Key Test Cases
- **API Authentication Token Validation**: Tests API authentication mechanism to ensure only valid tokens grant access.
- **API Response Time**: Measures and validates API response times meet performance requirements.
- **Logging**: Verifies that application events and transactions are properly logged for audit and debugging purposes.
- **Database Connection**: Tests database connectivity, connection pooling, and error handling for database operations.

## Folder Structure
The repository follows a clean folder hierarchy to ensure all testing assets are organized for easy navigation and access. Below is the structure of the repository:

```plaintext
QA-Testcases-Team-Nitin
│
├── README.md
│
├── TestCases
│   ├── Common
│   │   ├── TC_API_Authentication.md
│   │   ├── TC_API_Response_Time.md
│   │   ├── TC_Database_Connection.md
│   │   └── TC_Logging.md
│   │
│   └── Release-1.0
│       ├── TC_Login_Invalid.md
│       ├── TC_Login_Valid.md
│       └── TC_Password_Reset.md
│
├── Attachments
│   ├── execution_logs
│   │   └── Release-1.0
│   │       ├── TC_Login_Valid_Execution_Log
│   │       └── TC_Password_Reset_Execution_Log
│   │
│   └── screenshots
│       └── Release-1.0
│           ├── TC_Login_Valid_Screenshot.png
│           └── TC_Password_Reset_Screenshot.png
│
└── ExecutionResults
    └── Release-1.0
        ├── TC_Login_Invalid_Result.md
        ├── TC_Login_Valid_Result.md
        └── TC_Password_Reset_Result.md
```

### Folder Descriptions

- **TestCases**: This folder contains the test case scripts and definitions. Each test case is saved in its own file under either the `Common` folder (for test cases applicable across releases) or versioned folder (e.g., `Release-1.0`), and the test cases can be referenced and run by the test execution framework.

- **Attachments**: Contains supporting files for test execution:
  - `execution_logs`: Contains logs for each test case execution.
  - `screenshots`: Stores screenshots captured for validation purposes during the test case execution.

- **ExecutionResults**: This folder contains the results of the executed test cases. It holds the execution details and status for each test case after it has been run.

- **README**: The repository's documentation file, which provides an overview, usage instructions, and other relevant details.

## Test Case Structure
Each test case is represented by a unique file in the **TestCases** folder and is linked to the following attributes:

- **Test Case ID**: A unique identifier for the test case (e.g., `TC_Login_Valid`).
- **Story ID**: The user story that the test case corresponds to, usually tied to the project's requirements.
- **Feature**: The functionality or feature that the test case is validating.
- **Priority**: The importance of the test case (e.g., High, Medium, Low).
- **Owner**: The person responsible for the test case.
- **Linked Bugs**: Any bugs or issues associated with the test case (if applicable).
- **State**: Indicates whether the test case is automated or manual.
- **Sprint**: The sprint in which the test case is planned to be executed.
- **Regression**: Whether the test case is part of the regression suite.
- **Result**: The final result of the test case after execution (e.g., Pass, Fail).

## How to Contribute
1. **Fork the Repository**: Fork this repository to your own GitHub account.
2. **Add New Test Cases**: Create new test case files or modify existing ones under the appropriate folder (e.g., `TestCases/Release-1.0`).
3. **Execute Test Cases**: Run the tests and record the results (execution logs and screenshots) in the corresponding subfolders under `Attachments`.
4. **Submit Pull Requests**: Once you've completed your work, submit a pull request for review and integration into the main repository.

## Getting Started

### Prerequisites
- Ensure you have **Git** installed.
- Clone the repository to your local machine using the following command:
  ```bash
  git clone https://github.com/nitinmehtaa/QA-Testcases-Team-Nitin.git
  ```
- Set up your test execution environment based on the testing tools and frameworks used by your team.

### Running the Tests
The execution of test cases depends on the tools and frameworks being used. Please refer to the specific instructions provided for running tests in your team's testing environment (e.g., Selenium, Appium, JUnit, TestNG).

## License
This project is maintained by Team Nitin for QA purposes.

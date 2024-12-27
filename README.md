# QA Test Cases Repository

## Overview

This repository serves as a comprehensive and organized repository for QA test cases, enabling efficient testing, execution tracking, and smooth collaboration across teams. It consolidates all test-related artifacts—such as test case details, execution logs, screenshots, and results—into a structured format that streamlines the testing process and provides full traceability.

## Features

- **Organized Test Case Index**: A structured table of test cases that includes details like Test Case ID, Story ID, feature, priority, and results.
- **Execution Tracking**: A clear record of automated and manual test executions, linked to execution logs and screenshots.
- **Cross-Team Collaboration**: A well-organized structure that supports easy collaboration between different team members (owners, testers, developers).
- **Regression Suite**: A collection of regression tests that ensures continued product quality after code changes.

## Test Case Index

The following table outlines the current test cases for Release-1.0, with important details such as priority, execution status, and associated bugs:

| Test Case ID           | Story ID | Feature                    | Priority | Owner         | Linked Bugs | State       | Sprint | Regression | Result  |  
|------------------------|----------|----------------------------|----------|---------------|-------------|-------------|--------|------------|---------|  
| TC_Login_Valid         | 1234     | Login Functionality        | High     | John Doe      | 5678, 5680  | Automated   | S-01   | Yes        | ✅ Passed  |  
| TC_Password_Reset      | 1235     | Password Functionality     | Medium   | Jane Smith    | None        | Manual      | S-01   | Yes        | ✅ Passed  |  
| TC_Add_To_Cart_Valid   | 2345     | Cart Functionality         | High     | Alice Brown   | 6789        | Automated   | S-01   | Yes        | ✅ Passed  |  
| TC_Checkout_Payment    | 2346     | Payment Gateway Integration| High     | Bob Carter    | None        | Manual      | S-01   | Yes        | ✅ Passed  |  
| TC_Product_Search      | 2347     | Search Functionality       | Medium   | Alice Brown   | 6791, 6792  | Automated   | S-01   | Yes        | ✅ Passed  |  
| TC_Product_Filter      | 2348     | Filter Functionality       | Medium   | John Doe      | None        | Manual      | S-01   | Yes        | ✅ Passed  |  
| TC_Order_History_View  | 2349     | Order History              | Low      | Jane Smith    | 6793        | Automated   | S-01   | Yes        | ✅ Passed  |  

## Folder Structure

The repository follows a clean folder hierarchy to ensure all testing assets are organized for easy navigation and access. Below is the structure of the repository:

```plaintext
QA-Testcases-Team-Nitin
│
├── Attachments
│   ├── execution_logs
│   │   └── Release-1.0
│   │       ├── TC_Login_Valid_Execution_Log
│   │       └── TC_Password_Reset_Execution_Log
│   │   
│   └── screenshots
│       └── Release-1.0
│           ├── TC_login_page.png
│           └── TC_password_reset_page.png
│
├── ExecutionResults
│   └── Release-1.0
│       ├── TC_Login_Valid_Execution
│       └── TC_Password_Reset_Valid_Execution
│
├── TestCases
│   └── Release-1.0
│       ├── TC_Login_Valid
│       └── TC_Password_Reset
│
└── README
```

### Breakdown of Folders

- **Attachments**: This folder contains all supporting files such as execution logs and screenshots captured during test executions. It is subdivided into:
  - `execution_logs`: Contains logs for each test case execution.
  - `screenshots`: Stores screenshots captured for validation purposes during the test case execution.
  
- **ExecutionResults**: This folder contains the results of the executed test cases. It holds the execution details and status for each test case after it has been run.

- **TestCases**: This folder contains the test case scripts and definitions. Each test case is saved in its own file under a versioned folder (e.g., `Release-1.0`), and the test cases can be referenced and run by the test execution framework.

- **README**: The repository's documentation file, which provides an overview, usage instructions, and other relevant details.

## Test Case Structure

Each test case is represented by a unique file in the **TestCases** folder and is linked to the following attributes:

- **Test Case ID**: A unique identifier for the test case (e.g., `TC_Login_Valid`).
- **Story ID**: The user story that the test case corresponds to, usually tied to the project’s requirements.
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
4. **Submit Pull Requests**: Once you’ve completed your work, submit a pull request for review and integration into the main repository.

## Getting Started

### Prerequisites

- Ensure you have **Git** installed.
- Clone the repository to your local machine using the following command:

  ```bash
  git clone https://github.com/yourusername/QA-Testcases-Team-Nitin.git
  ```

- Set up your test execution environment based on the testing tools and frameworks used by your team.

### Running the Tests

The execution of test cases depends on the tools and frameworks being used. Please refer to the specific instructions provided for running tests in your team's testing environment (e.g., Selenium, Appium, JUnit, TestNG).



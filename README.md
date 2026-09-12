# COSC2759 Assignment 1
## Notes App - CI Pipeline
- Full Name/Names: Tony Chi
- Student ID/IDs: s3872779

## 1. Introduction - Analysis & Solution
### 1.1 Problem Analysis
The Notes Application was built and deployed directly from the lead developer's computer. This had created a dependency on a singular developer for important parts of software builds and deployment processes. This dependency had created several risks for the development and release cycle.

Problems identified:
- Dependent on a singular developer for building and deployment process
- Software release delays when the developer responsible for deplyoment was unavailable
- Manual building process that could be forgotten or performed incorrectly
- Lack of automated tests before changes were released
- Bugs and defects reaching production environment
- Increased workload for support caused by defects
- Additional development and QA work required to resolve issues post release

The main problem was the reliance on a manual process and a singular developer rather than having an automated and repeatable validation of changes.

### 1.2 Proposed Solution
The proposed solution is to implement a Continuous Integration (CI) pipeline through GitHub Actions.

The pipeline automatically validates changes whenever code is pushed to a branch or when a pull request is created or updated. This provides developers with quick feedback about their changes whether it passes the project's quality and testing requirements prior to being merged into the `main` branch.

The CI pipeline utilises existing npm scripts provided through the Notes application and automates their execution through GitHub Actions.

The completed pipeline includes:
- Installing project dependencies using `npm ci`
- Performing static code analysis using ESLint
- Running Jest unit tests
- Generating & uploading unit test coverage information
- Running MongoDB integration tests
- Generating a deployable application artifact on the `main` branch
- Installing Playwright and the required browsers
- Starting the Notes Application for E2E testing
- Running automated Playwright E2E tests against the application's user interface
- Uploading Playwright reports and test results when E2E tests fail

This will remove much of the manual validation which were previously required and provides a repeatable process for checking changes before they are integrated into the main codebase.

## 2. CI Pipeline
### 2.1 Pipeline Triggers 
The CI Pipeline is configured to run whenever changes are pushed to any branch in the repository. It also runs when a pull request is created or updated.

This ensures that any changes made on feature branches are automatically validated before they are merged into the `main` branch.

The triggers for the pipeline are:
- Pushes to a branch
- Pull Requests to the repository
- New commits pushed to an existing pull request

The workflow therefore provides CI validation throughout development rather than only after changes have been merged into `main`.

### 2.2 Dependency Installation
The pipeline installs the application Node.js dependencies using:

`npm ci`

The command is executed from the `src` directory as this is where the application's `package.json` and `package-lock.json` files are located.

`npm ci` provides a clean and repeatable dependency installation which is based on the project's lock file.

### 2.3 Static Code Analysis
The pipeline performs static code analysis using ESLint.

The command used by the pipeline is:

`npm run test:lint`

This executes the existing ESLint configuration provided by the application and checks the source code for coding errors and other problems.

If ESLint detects an error, the GitHub Actions job fails and the later stages of the pipeline are not considered successful.

This allows code quality issues to be identified automatically before changes are merged.

### 2.4 Unit Testing
The pipeline executes the application's Jest unit tests using:

`npm run test:unit`

The existing npm script runs Jest against the unit test directory and enables code coverage

Unit testing provides automated verification that individual parts of the application behave as expected.

If a unit test fails, Jest returns a failure status and the GitHub Actions job fails. The failure details are then displayed in the GitHub Actions workflow logs.

### 2.5 Code Coverage 
Code coverage is generated as part of the Jest unit testing stage.

The coverage results are generated in the application's `src/coverage` directory

The pipeline uploads the generated coverage information as a GitHub Actions artifact named:

`unit-test-coverage`

The artifact allows the coverage results from the CI run to be downloaded and reviewed after the workflow has completed.

Code coverage provides additional information about which parts of the application are exercised by the automated unit tests.

### 2.6 Integration Testing
The pipeline runs the application's existing Jest integration tests using:

`npm run test:integration`

The MongoDB service is provided within the GitHub Actions environment so that the application can be tested against a database during the CI run.

Integration testing verifies that the application's components can work together correctly, including communication between the application and MongoDB.

If an integration test fails, the GitHub Actions job fails and the test failure is displayed in the workflow logs.

### 2.7 Deployable Artifact
After the validation stages have successfully completed, the pipeline creates a deployable application package when the workflow is running on the `main` branch.

The application files are packaged into a ZIP file named:

`notes-app.zip`

The package is uploaded to GitHub Actions as the:

`notes-application` artifact

The deployable artifact is only generated when the workflow is running on `main`. Feature branches can therefore be validated by the CI pipeline without producing a deployment artifact.

This ensures that deployment packages are only produced from code that has been integrated into the main branch.

### 2.8 End-to-End Testing
The pipeline performs automated end-to-end testing using Playwright.

The Playwright browsers and their required dependencies are installed before the tests are executed.

The Notes Application is then started in the GitHub Actions environment so that it is available at:

`http://localhost:3000`

The existing E2E test suite is executed using:

`npm run test:e2e`

The automated test validates the application from a user's perspective by:

1. Opens the Notes application
2. Selects **New Note**
3. Verifies navigation to the new note page
4. Enters a note title
5. Enters a note description
6. Saves the note
7. Verifies navigation back to the main page
8. Deletes the newly created note
9. Verifies the application remains on the main page

The Playwright tests are executed against:
- Chromium
- Firefox
- WebKit

This provides automated UI validation across multiple browsers.

If an E2E test fails, the GitHub Actions job fails, Playwright's HTML report and test results are uploaded as GitHub Actions artifacts so that the failure can be investigated after the workflow has concluded.

## 3. GitHub Flow
### 3.1 Feature Branches
Development changes are made using feature branches rather than directly modifying the `main` branch.

Feature branches allow for individual changes to be developed and tested independently.

The CI pipeline runs against changes made to feature branches, allowing problems to be identified before the changes are merged.

Examples of feature branches used during development include:
`feature/ci-pipeline`
`feature/documentation`
`feature/integration-testing`
`feature/artifact-generation`
`feature/e2e-testing`

### 3.2 Pull Requests
Once work on a feature branch is complete, a pull request is created to merge the changes into `main`.

Github Actions automatically runs the CI pipeline against the changes made, providing automated feedback before the pull request is merged.

Before merging a feature branch into `main`, the pull request and CI results can be reviewed to ensure that the changes have passed the automated checks.

The feature branch is only merged into `main` after the required changes have been reviewed and the CI checks have successfully completed.

This GitHub Flow approach reduces the risk of introducing untested changes directly into the main branch.

## 4. Pipeline Results
Successful pipeline runs are displayed in the GitHub Actions section of the repository.

The workflow provides separate steps showing the progress of each stage of the CI process:

- Dependency installation
- Static code analysis
- Unit testing
- Code coverage
- Integration testing
- Deployable artifact generation
- Playwright browser installation
- Application startup
- End-to-end testing

A successful pipeline should show the validation stages as completed successfully.

If a test or quality check fails, the corresponding GitHub Actions step is marked as failed and the workflow does not complete successfully.

For E2E failures, the Playwright report and test results can be downloaded from the workflow's artifacts to provide additional information about the failure.

**Screenshot 1 - Successful CI Pipeline**
<img src="/img/successful-cipipeline.png" style="height: 250px;"/>

## 5. Pipeline Artifacts
The CI pipeline produces the following artifacts:

**Unit Test Coverage**

`unit-test-coverage`

Contains the coverage information generated by the Jest unit tests.

**Deployable Application**

`notes-application`

Contains the deployable `notes-app.zip` package.

**Playwright Report**

`playwright-report`

Contains the Playwright HTML report when E2E testing fails.

**Playwright Test Results**

`playwright-test-results`

Contains additional Playwright test result files generated during E2E testing failures.

These artifacts allow test and build results to be retained and investigated after the GitHub Actions workflow has completed.

**Screenshot 2 - Main Branch artifacts**
<img src="/img/cover-notesapp.png" style="height: 250px;"/>

**Screenshot 3 - Playwright Failure Evidence**
<img src="/img/report-testresults.png" style="height: 250px;"/>

## 6. Pipeline Flow
The CI pipeline follows this sequence:

1. Code is pushed to a branch or a pull request is created or updated.
2. GitHub Actions checks out the repository
3. Node.js dependencies are installed using `npm ci`
4. ESLint performs static code analysis
5. Jest unit tests are executed
6. Unit test code coverage is generated and uploaded as an artifact.
7. MongoDB integration tests are executed
8. When the workflow is running on the `main` branch, a deployable application package is created and uploaded as an artifact
9. Playwright browsers are installed
10. The Notes Application is started and made available at `http://localhost:3000`
11. Playwright executes the end-to-end tests against the application's user interface
12. The CI pipeline produces a final result:
- **Success:** All validation and testing stages complete successfully.
- **Failure:** If a linting check, unit test, integration test, or end-to-end test fails, the GitHub Actions workflow fails and the relevant error information is displayed in the workflow logs.
- **E2E Failure:** When an end-to-end test fails, the Playwright HTML report and test results are uploaded as artifacts to help with investigating the failure.

The pipeline runs validation on all branches, while the deployable application artifact is generated only from `main`.
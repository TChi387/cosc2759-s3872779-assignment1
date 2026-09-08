# COSC2759 Assignment 1
## Notes App - CI Pipeline
- Full Name/Names: Tony Chi
- Student ID/IDs: s3872779

### Guidance (remove this section before final submission)

1. Refer for assignment specification `Marking Guide` for details of what should appear in this README.

2. If you do not see an `Actions` tab in your GitHub, email craig.anslow@rmit.edu.au with URL to your repository, so that it can be enabled.

3. Implement your CI pipeline in the directory `.github/workflows`.

4. Refer to [src/README.md](/src/README.md) for important details on building and testing the application.

5. Commit images to the `img` directory and add them like 
    ```html
    <img src="/img/md.png" style="height: 70px;"/>
    ```
    <img src="/img/md.png" style="height: 70px;"/>

6. Only edit THIS README.md - not the src/README.md
## 1. Analysis & Justification
### 1.1 Problem Analysis
The Notes Application was built and deployed directly from the lead developer's computer and in doing so had created a dependency on a singular developer which meant that software releases could have been delayed whenever that dev was unavailable.

Problems identified:
- Dependent on a singular developer for building and deployment process
- Delays when the developer responsible for deplyoment is unavailable
- Manual process that could be forgotten or performed incorrectly
- Lack of automated tests before changes were released
- Bugs reaching production environment
- Increased workload for support caused by defects

### 1.2 Proposed Solution

The proposed solution is to implement a Continuous Integration (CI) pipeline through GitHub Actions.

The pipeline automatically performs software quality checks whenever changes are made and pushed to a branch, or when a pull request is created which provides developers quick feedback about their changes and if it passes the project's quality and testing requirements.

The CI pipeline utilises existing npm scripts which were provided through the Notes application to which is automated through GitHub Actions.

So far the pipeline currently includes:
- Installing project dependencies
- Performing static code analysis using ESLint
- Running Jest unit tests
- Generating code coverage information
- Uploading the generated coverage as a GitHub Actions artifact

*More CI stages will be added on as implementation progresses

## 2. Heading
### 2.1 Subheading 
### 2.2 Subheading 

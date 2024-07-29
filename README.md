# BOM 2 UAT
## Introduction
The Back Office Management 2 User Acceptance Tests (BOM 2 UATs) act as a repository for automated end-to-end tests of Smobilpay products within BOM 2, focusing on QA-level regression and smoke testing. These tests are executed daily through a Jenkins pipeline and can be accessed in the Allure report on the Docker server.

## Installing
{collapsible="true" default-state="collapsed"}

### Prerequisites
To run our uats we need to install several tools depending on our OS:
1)   **Node.js**: as a runtime environemment for running cypress localy
2)  **VS code**: as our code editor.
3) **Git**: for code versionning and git operations.
4) **Web browser**: sucha as chrome or firefox (optionnal, cypress own is own  lite weight web browser Electron).

### Steps
1) Clone the repository from Bitbucket (in the develop branch) [Here](https://bitbucket.org/maviance-development/bom2_uat/src/develop/)
2) Checkout a new branch with `git checkout -b <branch name>`
3) Pull the latest version of the code with `git pull origin develop`
4) Install all the dependencies with in the bom2_uat directory with `npm install`
5) Verify that everything is functioning correctly by opening Cypress with the command `npx cypress open`. 
6) Ensure that windows similar to the one shown in the image below appear.

   <img alt="cypress" height="520" src="Desktop_screenshot.png" width="520"/>

## Actions
{collapsible="true" default-state="collapsed"}

To simplify the test automation phase, we have established common actions to assist testers in performing repetitive tasks without needing to write the entire Cypress code. We have three main JavaScript files for our common actions: \
![actions](Desktop_screenshot_(5).png) 
1) `CommonActions.js` is a JavaScript class that provides a wide range of reusable functions for interacting with web elements and performing common actions in our E2E testing, along with utility functions for data manipulation.
2) `CommonServices.js` is a JavaScript class that includes a method for generating an access token using the provided username, password, and grant type to authenticate properly on the BOM2 platform.
3) `HttpActions.js` this class provides methods for sending HTTP requests (GET, POST, PUT, PATCH, DELETE) and asserting their responses in tests. Each method requires the endpoint URL, optional headers, and request data for certain request types. It utilizes Cypress's cy.request() to send requests and asserts the response status code, returning the response object for further processing. These methods are essential for making HTTP requests and verifying responses in end-to-end tests, ensuring data integrity.


## Test
{collapsible="true" default-state="collapsed"}

Here, we will explore how to set up test automation code with Cypress for end-to-end (E2E) testing in BOM2.
![setup](Desktop_screenshot_(11).png) \
In the picture above, we see that to create a test suite, we need to create a new directory in the `e2e/tests` directory with the name of the test suite. Inside this directory, we can create a test file that should always end with the `.cy.js` extension. 
To write test here we have several steps to follow:
### Describe
is a function in the testing framework that is used to group
and organize test cases related to the feature of the application that we are testing. Inside this function, you
can define multiple test cases using `it` blocks to test different scenarios related to the
functionality that you are testing. This helps in structuring and categorizing the tests for better readability and
organization.
![describe](Desktop_screenshot_(12).png)
### Before
This block is used to run setup code before any of the test
cases within the `describe` block are executed. In our case, we use it to load our test data and user data.
![before](Desktop_screenshot_(13).png)
### `it` Block
The `it()` function is used to define an individual test case and is typically placed inside a `describe()` block, which groups related tests together. Each `it()` block runs in isolation, ensuring that the application state is reset between tests, so they do not affect one another. Using multiple `it()` blocks enhances readability and maintainability by focusing on one functionality or scenario at a time. \
In our case, one or more it() blocks can refer to the acceptance criteria of an user story.
![it](Desktop_screenshot_(14).png)

### `it.skip()`
In Cypress, `it.skip()` is used to temporarily skip a specific test case without removing it from the codebase. This is particularly useful during development or debugging when you want to focus on other tests without executing a particular one.This feature helps maintain test organization and can be beneficial when dealing with flaky tests or when a feature is under development.

### `it.only()`
In Cypress, `it.only()` is used to run a specific test case while skipping all other tests in the suite. This is particularly helpful during development or debugging when you want to focus on a single test without executing the entire test suite.This feature is useful for ensuring that you can quickly iterate on a single test without the distraction of other tests running simultaneously.

### `beforeEach()` and `afterEach()`
In Cypress, `beforeEach()` and `afterEach()` are used to set up and tear down conditions for tests within a `describe()` block. The `beforeEach()` function runs before each individual test case defined by `it()`, allowing you to set up common prerequisites, such as navigating to a specific page or resetting application state. Conversely, `afterEach()` runs after each test case, which is useful for cleaning up or resetting any changes made during the test.

### Tags
In Cypress, test tags are used to categorize and organize tests based on their purpose and scope. in our project we have three main tags: `<smoke>`, `<sanity>`, and `<regression>`:
1) `<smoke>` tests are a subset of test cases that verify the most critical functionalities of an application to ensure that the basic features work correctly after a new build or deployment. They act as a quick check to confirm that the application is stable enough for further testing.

2) `<sanity>` tests are focused on verifying specific functionalities after changes have been made, such as bug fixes or new feature implementations. They ensure that the particular areas of the application impacted by the changes are functioning as expected without performing exhaustive testing.

3) `<regression>` tests are designed to confirm that previously developed and tested features still work after changes, such as enhancements or bug fixes, have been made to the codebase. They help identify any unintended side effects caused by recent updates.

<details>
<summary>Fixtures</summary>

## Fixtures
{collapsible="true" default-state="collapsed"}

fixtures are used to manage and load external data for testing purposes, typically stored in JSON or Javascript files. By utilizing the `cy.fixture()` command, we can easily load this data into our tests, allowing for better organization and reusability of test data. This is particularly useful for specific data such as expectation data, templates (e.g., Bulk Payment XLS files), external data (media, documents, proofs), mocked API responses, user data for different test environments, and more, without requiring live interactions (e.g., database access or human actions).
![fixtures](https://github.com/user-attachments/assets/87e98d95-0b21-4065-9a72-595cfea5141f)
</details>


## Pages
{collapsible="true" default-state="collapsed"}

This folder contains classes for the web pages being tested. Each class includes web element identifiers and action methods for interacting with the elements on the page, based on common actions.
![pages](Desktop_screenshot_(7).png)

1) **in Red** we have different pages (classes) along with their identifiers and methods
2) **in Yellow** we have the structure of a << page >> with identifiers organized by page section.
3) Below is the structure of an action method for a page, which clicks on the profile icon on the home page.
   ![action](Desktop_screenshot_(9).png)

## Support
{collapsible="true" default-state="collapsed"}

It provides a variety of useful modules for common test actions, such as adjusting the viewport, changing the test environment, and logging into BOM 2 based on the user role, among others.
<img alt="support" height="240" src="Desktop_screenshot_(10).png" width="240"/>
Refering to the caption below, we have in our project  four support modules:

1) `command.js` is defining custom Cypress commands for a test automation script, such as: 
   * Read the JSON file to pass test data in a test;
   * Login to the application based on the user role
   * Logout user from the application
2) `e2e.js` is setting up configurations and behaviors for our test,  such as 
   * Set the default command timeout
   * Set the default viewport size
   * Disable screenshots during test runs
   * Before running any tests, read users data and store in Cypress env variable userData for later use.
   * Before running any tests, read service endpoints data and store in Cypress env variable endpoints for later use.
   * Before running any tests, store environment config info in Cypress env variable env_config for later use.
   * Before each test, group tests by matching test type value
3) `envconfig.js` contains all the URLs for each service available in BOM2, depending on the test environment.
4) `excelUtility.js` It is a module used to read Excel files and return their content.

<details>
<summary>Running</summary>
   
## Running

Here we could run the tests in two different ways: in a **_console_** or in a **_browser interface_**

### Console
To run a suite of e2e tests in console just run `npx cypress run --spec cypress/e2e/tests/suite_name`

   <img alt="console_test" height="520" src="Desktop_screenshot_(1).png" width="520"/> \
to run a specific test in a suite, run `npx cypress run --spec cypress/e2e/tests/suite_name/test_file_name.cy.js`    

Running tests in the console will perform end-to-end (E2E) testing of the specified tests in a browser and display results in screenshots in the event of failing tests, without opening any specific windows.

### Browser interface
To run our test in browser interface, we should first of all open cypress interface by running `npx cypress open` \
then on the interface select **E2E Testing**, after that select the browser in wich you want to perform your e2e tests like in the picture below. \
<img alt="broswer" height="520" src="Desktop_screenshot_(2).png" width="520"/> \
Then, begin the end-to-end (E2E) testing by selecting the specifications in which you want to conduct your tests.

<img alt="spec" height="520" src="Desktop_screenshot_(3).png" width="520"/> \
We have the test runner interface with annotations displayed here.

![annotate](Desktop_screenshot_(4).png)

1) **Test Specs:** we can see the test suites and their corresponding test files here.
2) **Tests cases:** here, we have the different test cases along with their statuses after running the automated tests.
3) **Realtime testing interface:** it displays in real-time how the test is progressing.
4) **Spec file:** we have the test specification file that is currently running, along with the remaining time for the test to complete.
5) **Passed Tests:** this section contains the number of tests that have passed.
6) **Failed Tests:** this section contains the number of tests that have failed.
7) **Skipped Tests:** this section displays the number of tests that have been skipped.
8) **Run/Stop button:** used to run tests or stop them while they are in progress.
9) **Identifier targeter:** used to directly target identifiers on the web page with Cypress
10) **Screen size:** displays the current size of the test interface and is primarily used for responsive test cases.
</details>

<details>
<summary>Reporting</summary>
   
## Reporting

In Cypress, reporting is an essential feature that enhances test visibility and debugging capabilities. Cypress automatically captures screenshots of failed tests, which can be invaluable for diagnosing issues. By default, a screenshot is taken whenever a test fails, allowing developers to see the application state at the time of the failure. This feature can be customized through the configuration file to adjust when screenshots are taken.

Additionally, Cypress can be integrated with Allure Reports, a popular reporting tool that provides a more detailed and visually appealing representation of test results. To use Allure with Cypress, we typically need to install specific plugins and configure them in our project [more here](#internal-docs). Once set up, Allure generates comprehensive reports that include test status, execution times, and detailed logs, along with screenshots and videos. This integration helps us analyze test outcomes more effectively and track regression over time, making it easier to maintain high-quality software. In our case, we use an [Allure server](https://allure-ui.dev.maviance.info/allure-docker-service-ui/projects/bom2-uat) linked to the Jenkins pipeline, allowing tests to run daily at a specific hour. To execute these tests, Allure's base test configuration relies on test tags (e.g., `<smoke>`, `<regression>`) and the test environment (Integration, Acceptance).
![allure](https://github.com/user-attachments/assets/edd04d93-900c-4eff-bbec-3c98dfc7c866)
</details>

<details>
<summary>Documentation</summary>
   
## Documentation

### Internal docs
[Automated E2E Tests for BOM2](https://maviance.atlassian.net/wiki/spaces/MD/pages/3072098320/Automated+E2E+Tests+for+BOM+2) \
[Local test reporting with Cypress and Allure](https://maviance.atlassian.net/wiki/spaces/MD/pages/3072229408/05-Reporting+and+Setting+up+reporting+plugin) \
[Allure Reporting Tool](https://maviance.atlassian.net/wiki/spaces/MD/pages/2650538352/Allure+Reporting+Tool)
### External docs
[Cypress Documentation](https://docs.cypress.io/guides/overview/why-cypress) \
[Cypress test writing and organizing](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests)
</details>


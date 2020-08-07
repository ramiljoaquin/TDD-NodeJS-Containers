# Currency Exchange Microservice - Proof of Concept of test-driven development

This application was created using test-driven development(TDD) methodologies, in particular the Red-Green-Refactor or test-first approach. No code was written without **_first_** writing an associated unit test.

Code that has unit tests is regarded as more complete and accurate. The unit tests function as a means to clearly understand the application. Requirements for the application translate into tests, so examining the tests gives you an idea about what the application does, and it also shows how to use the code. For our unit tests we use [Jest](https://jestjs.io/), a JavaScript unit-test framework testing library that works well with TDD.

## What is test-driven development?

Test-driven development reverses traditional development and testing. So, instead of writing your code first and then retroactively fitting a test to validate the piece of code you just wrote, test-driven development dictates that you write the test first and then implement code changes until your code passes the test you already wrote.

In TDD, you write your unit test first, watch it fail, and then implement code changes until the test passes. Sounds backwards, right? But the code you produce when you use this testing methodology is cleaner and less prone to breaking in the long run.

A unit test is simply a test that covers a small portion of logic, like an algorithm, for example. Unit tests should be deterministic. When I say “deterministic” I mean that unit tests should never have side-effects like calls to external APIs that deliver random or changing data. Instead, you’d use mock data in place of data that could potentially change over time.

Five steps of test-driven development
There are 5 steps in the TDD flow:

1. Read, understand, and process the feature or bug request.
2. Translate the requirement by writing a unit test. If you have hot reloading set up, the unit test will run and fail as no code is implemented yet.
3. Write and implement the code that fulfills the requirement. Run all tests and they should pass, if not repeat this step.
4. Clean up your code by refactoring.
5. Rinse, lather and repeat.

Figure 1 shows these steps and their agile, cyclical, and iterative nature:

![Red green refactoring in TDD](doc/source/images/tdd-red-green-refactoring.png)

This workflow is sometimes called Red-Green-Refactoring, which comes from the status of the tests within the cycle.

- The red phase indicates that code does not work.
- The green phase indicates that everything is working, but not necessary in the most optimal way.
- The blue phase indicates that the tester is refactoring the code, but is confident their code is covered with tests which gives the tester confidence to change and improve our code.

## Test-driven development and CI/CD

Continuous integration(CI) is a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build, allowing teams to detect problems early. By integrating regularly, you can detect errors quickly, and locate them more easily.

The unit tests that come out of TDD are also an integral part of the continuous integration/continuous delivery (CI/CD) process. TDD relates specifically to unit tests and continuous integration/continuous delivery pipelines like CircleCI, GoCD, or Travis CI which run all the unit tests at commit time.

The tests are run in the deployment pipeline. If all tests pass, integration and deployment will happen. On the other hand, if any tests fail, the process is halted, thus ensuring the build is not broken.

## Set up your tools, toolchain, and IDE first

In order to do test-driven development, you need to setup your tools, toolchain, and IDE first. In our [code pattern], we are developing a Node.js example, so here are the key tools we set up:

- nvm (Node Version Manager) for Node.js and NPM: NVM allows you to run the Node.js version you want and change it without affecting the system node.
- npm libraries for development:

1. Jest for unit tests
2. ESLint for linting
3. Prettier for formatting
4. Husky and lint-staged for precommit Git hooks

## How to write unit tests that fail

There are a couple different ways to write unit tests that fail.

1. Write a test that references a function in the code that doesn’t exist yet. This will cause the test to fail with a non-found error (for instance, a 404 error).

2. Alter the assert statement to make it fail. An assert statement says what value the code being tested is expected to return; this kind of statement is a key aspect of a unit test. The assert statement should reflect the feature or bug fix request.

So, to make it fail, you would write an asset statement that returns an unexpected value in, say, a data structure you want to enrich. For example, your JSON returns a person’s name, but your new requirement says to include the person’s cellphone number. You would first write the assert statement to only include the person’s name, which would cause it to fail. Then you would add the code to include the person phone number as well.

Or, in real life coding: Your assert statement could be: assert actualResult == {‘track’:‘foo fighters’}. Once the code (function) is hooked up, the 404 goes away, but the actual result could be an empty object like {}. You then hard code the result in the function to be {‘track’:‘foo fighters’}.

The test will now pass (Green!). The code is obviously just a sub for now, but you can get the basic understanding. The test is wired up to a point in the code correctly. From there you can implement actual business logic, for example, read a file/db/call an external API.

## Architecture

This flow is for the runtime of the currency conversion microservice.

![run time flow](doc/source/images/architecture.png)

**_Figure 1. Production flow_**

1. Client API Consumer calls the microservice over the internet (http/s request).
1. ExpressJS `web server` accepts the REST request (e.g. GET /convertCurrency/ZAR/USD/600.66).
1. Code routing in Express passes the request to a service module which in turn calls the External European Currency Exchange API (http://api.exchangeratesapi.io).
1. An exchange rate for ZAR is retrieved and stored. The value of 600.66 South African Rands (ZAR) is converted to US Dollars(USD).
1. The ExpressJS `web server` sends a response to the calling consumer with the dollar amount (in this case, \$40.59 ).

## Included components

- [Swagger](https://swagger.io/): A framework of API developer tools for the OpenAPI Specification that enables development across the entire API lifecycle.

## Featured technologies

- [Node.js](https://nodejs.org/): Node.js is a JavaScript framework which has an awesome package manager called `npm` that lets you build applications with components built and supported by an active open source community.
- [Express](https://expressjs.com/): Fast, unopinionated, minimalist web framework for Node.js.
- [Axios](https://www.npmjs.com/package/axios): Promise based HTTP client for the browser and Node.js.
- [csvtojson](https://www.npmjs.com/package/csvtojson): A node module is a comprehensive nodejs csv parser to convert csv to json or column arrays
- [esm](https://www.npmjs.com/package/esm): The brilliantly simple, babel-less, bundle-less ECMAScript module loader.
  <details><summary><strong>Why JavaScript Modules?</strong></summary>

> Modules in JavaScript are really great feature in the latest version of the language. Unfortunately it's not supported in earlier versions of Node.js ( < ver 13.2.x )

> So in order to use these new features you'll need a transpiler to generate plain old JavaScript for now.

> we recomment `esm` which is the world’s most advanced ECMAScript module loader. This fast, production ready, zero dependency loader is all you need to support ECMAScript modules in Node 6+.

> See the release [post](https://medium.com/web-on-the-edge/tomorrows-es-modules-today-c53d29ac448c) for details!

</details>

# Prerequisites

You need to have the following installed to complete the steps in this code pattern:

For running these services locally without Docker containers, you need:

- [Node.js v10 or later](https://nodejs.org/en/download/)
  <details><summary><strong>Tip: Use Node Version Manager (nvm)</strong></summary>

> nvm is a simple bash script to manage multiple active Node.js versions.

> We recommend using `Node Version Manager (NVM)` to control the version of Node.js that you use.

> Why? The system or operating system installed Node.js version is fixed. You may need different versions of Node for other projects.

> NVM allows you to choose and switch to the version of Node.js that suits your needs.

> Install via command line:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

[Learn more about NVM](https://github.com/nvm-sh/nvm) and find the latest installation instructions.

</details>

- [Relevant Node.js packages](package.json): Use `npm install`

### This code pattern was built 100% TDD and has 100% test coverage.

We use Jest as our unit test framework. Jest uses the popular `describe`, `it`, and `expect` syntax

Remember to use mock data so that your tests don't fail because of changing data.

This pattern includes neat developer productivity tools:

1. linting and formatting NPM scripts

You can call it by running `npm run lint`. You can use the `prettier` formatter which can be run with `npm run format`.

The unit tests we run in this pattern are run in the deployment pipeline

# Steps

Follow these steps to set up and run this code pattern locally. The steps are described in detail below.

### 2. Run the application locally

1. Install packages with NPM by running `npm install`.
2. Start the app by running `npm start`.
3. Browse the API from your browser `localhost:4001`.

> Note: The server host can be changed as required in the server.js file, and `PORT` can be set in the `.env` file.

## Anatomy of this application

These are the key components of this microservice.

- **Jest for unit testing**

  - Use Jest `mocks` to run unit tests locally without side effects. Examples of side effects include:
    _ Calling external services (like other Web APIs) that are changed or offline; for example, the World Bank currency exchange API that our microservice wraps.
    _ Calling external databases that are in-flux or down \* Using time stamps and random ID generation that are non-deterministic, so they're not good for test data that may be generated on the fly (Mocks really shine here and provide expected reliable values that tests your business logic).
  - Hot code reloading (aka On page save hooks) run tests automatically on save by running `Jest -watch`.

- **Pino for logging**

  - A best practice is to have a logging framework to extract good errors from your application, as console.log is not always sufficient.

  - [Pino](https://github.com/pinojs/pino) is a great simple tool to use logging framework.

- **Code formatting**

  - [Prettier](https://prettier.io/) for code formatting.

- **JavaScript syntax checking**

  - [ESLint](https://eslint.org/) helps you find and fix problems in your JavaScript code.

- **Git pre-commit hooks**
  Every time you run `git commit ...` both the linter and formatter will run. If, for example, you have extra spaces in your code like `const planet = " Saturn ";`, the formatter automatically cleans up the code and formats it correctly to be `const planet = "Saturn";`. This newly formatted code is then committed and can be pushed. However, say you have a syntax error, for example `cnst planet = "Saturn";`, the commit will fail as the symbol `cnst` is invalid. You will see informative output in your console as Figure 3 shows. Once you have manually corrected the syntax error, you can recommit it until the syntax is correct and the linter passes.

![Git pre-commit hooks](./doc/source/images/pre-commit-hook-syntax-error-csnt-const.jpg)

**_Figure 3. Syntax error caught by Git pre-commit hooks with both linter 1 (ESLint) and formatter 2 (Prettier)_**

This is achieved with the two `npm` libraries `lint-staged` and `husky`, which are installed by running `npx` as such:

    ``` sh
        npx mrm lint-staged
    ```

You will see the following automatically appended to the `package.json` file:

`json "husky": { "hooks": { "pre-commit": "lint-staged" } }, "lint-staged": { "*.js": "eslint --cache --fix", "*.+(js|json)": "prettier --write" }`

- [`esm`](https://www.npmjs.com/package/esm)

  - The brilliantly simple, babel-less, bundle-less ECMAScript module loader.
  - A lightweight **JavaScript Transpiler** alternative to `babel`
  - Allows you to use modern JavaScript for example: `modules` with `import` and `export` rather than the older `requires()` methods to link packages

- [`rimraf`](https://www.npmjs.com/package/rimraf)

  - Cleanup previous builds and distributions
    - rimraf is The UNIX command `rm -rf` for **_node_**

- **`swagger`**
  - Installing the npm package `swagger-ui-express` lets you create a REST API with a well-documented test harness with almost no effort at all, giving your microservice that professional and polished look as well as a useful way to manually test the API from a swagger html test harness.

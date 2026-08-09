# Newman and CI/CD

While Postman's GUI is great for development, automated pipelines (like GitHub Actions, GitLab CI, or Jenkins) don't have a GUI. This is where Newman comes in.

## What is Newman?

Newman is Postman's command-line Collection Runner. It allows you to run and test a Postman Collection directly from the command line. It's built on Node.js.

## Basic Usage

1. **Export** your Collection and Environment from Postman as JSON files.
2. **Install** Newman via npm: `npm install -g newman`
3. **Run** the collection:
   `newman run my-collection.json -e my-environment.json`

## Reporting

Newman can generate various reports (CLI, JSON, HTML, JUnit). For CI/CD pipelines, JUnit or XML reporting is often used so the CI server can parse the test results.

`newman run my-collection.json -r cli,junit --reporter-junit-export results.xml`

## CI/CD Integration

**Why is this important for an SDE?**
Interviews often focus on the entire software lifecycle. Knowing how to integrate tests into a pipeline shows you care about continuous quality.

*   You can set up a GitHub Action to run Newman every time code is pushed or a Pull Request is created.
*   If Newman encounters a failing test (e.g., a status code was 500 instead of 200), it returns a non-zero exit code. The CI pipeline detects this exit code and marks the build/PR as "Failed," preventing broken code from being deployed.

**GitHub Actions Example (`.github/workflows/api-tests.yml`):**
```yaml
name: API Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  api-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Install Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Newman
        run: npm install -g newman

      - name: Run API Tests
        run: newman run postman/my-collection.json -e postman/dev-environment.json -r cli,junit --reporter-junit-export results.xml

      - name: Upload Test Results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: api-test-results
          path: results.xml
```

## Data-Driven Testing

Newman supports running a collection multiple times with different data sets. You provide a CSV or JSON file, and Newman iterates through each row, injecting the values as variables.

**Example CSV file (`test-data.csv`):**
```csv
username,email,expectedStatus
john_doe,john@example.com,201
,missing@example.com,400
jane_doe,,400
```

**Running with data:**
```bash
newman run my-collection.json -d test-data.csv -n 3
```
*   `-d test-data.csv`: The data file.
*   `-n 3`: Number of iterations (matches the 3 rows in the CSV).
*   Inside your requests, use `{{username}}`, `{{email}}`, and `{{expectedStatus}}` to reference the data.

*Interview Tip:* Mentioning data-driven testing shows that you understand how to test boundary conditions (empty fields, missing values) systematically rather than one request at a time.

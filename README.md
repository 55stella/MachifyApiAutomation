# Machify Backend API Test Automation

This repository contains automated API tests for the Machify Backend, written using Postman and executed via Newman in a CI/CD pipeline using GitHub Actions.

## API Endpoints Tested

### Profile Creation

- **Endpoint:** `POST https://matchify-backend-theta.vercel.app/api/profile`
- **Tests:**
  - Successful profile creation with valid inputs.
  - Validation to ensure the `age` field is a number only.
  - Error response when required fields are missing.

### Login

- **Endpoint:** `POST https://matchify-backend-theta.vercel.app/api/login`
- **Tests:**
  - Successful login with valid credentials.
  - Error response for empty values.
  - Error response for incorrect credentials.

## Running Tests Locally

To run the tests locally, you need to have Node.js and Newman installed.

### Steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/55stella/MachifyApiAutomation.git
   ```
2. Navigate to the project directory:
   ```bash
   cd MachifyApiAutomation
   ```
3. Install Newman globally if not already installed:
   ```bash
   npm install -g newman
   ```
4. Run the Postman collection:
   ```bash
   newman run postman/MachifyBackendAutomation.postman_collection.json --reporters cli
   ```

## CI/CD Pipeline

The API tests are integrated into a GitHub Actions pipeline, which runs on every push and pull request to the `main` branch.

### GitHub Actions Configuration:

```yaml
name: Postman API Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  run-postman-tests:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Install Newman
      run: npm install -g newman

    - name: Run Postman Collection
      run: newman run postman/MachifyBackendAutomation.postman_collection.json --reporters cli
```

## Test Output Summary

### Profile Creation

- **Successful profile creation:**

  - Status code: 201
  - Message: Successful profile creation
  - Validation: Age must be a number only

- **Negative Test:**

  - Status code: 400
  - Message: User is prevented from proceeding if required fields are not filled.

### Login

- **Negative Test (Empty value):**

  - Status code: 400

- **Negative Test (Incorrect credentials):**

  - Status code: 401

- **Successful login:**

  - Status code: 200
  - Message: Successful login with user information returned

## Summary Report

```
┌─────────────────────────┬────────────────────┬────────────────────┐
│                         │           executed │             failed │
├─────────────────────────┼────────────────────┼────────────────────┤
│              iterations │                  1 │                  0 │
├─────────────────────────┼────────────────────┼────────────────────┤
│                requests │                  5 │                  0 │
├─────────────────────────┼────────────────────┼────────────────────┤
│            test-scripts │                  5 │                  0 │
├─────────────────────────┼────────────────────┼────────────────────┤
│      prerequest-scripts │                  0 │                  0 │
├─────────────────────────┼────────────────────┼────────────────────┤
│              assertions │                 12 │                  0 │
├─────────────────────────┴────────────────────┴────────────────────┤
│ total run duration: 1237ms                                        │
├───────────────────────────────────────────────────────────────────┤
│ total data received: 608B (approx)                                │
├───────────────────────────────────────────────────────────────────┤
│ average response time: 226ms [min: 85ms, max: 773ms, s.d.: 273ms] │
└───────────────────────────────────────────────────────────────────┘
```


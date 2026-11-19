# API Testing Project – QA Portfolio

A hands-on API testing project focused on validating REST API functionality, authentication, authorization, security, data integrity, and performance using **Postman** and **Supabase**.

The project demonstrates practical QA skills through documented test cases, automated API assertions, negative testing scenarios, and Row Level Security (RLS) validation.

---

## Project Overview

| Detail         | Description                    |
| -------------- | ------------------------------ |
| Project        | API Testing Project            |
| Type           | Manual & Automated API Testing |
| API Platform   | Supabase REST API              |
| Testing Tool   | Postman                        |
| Database       | PostgreSQL                     |
| Authentication | Supabase Auth / JWT            |
| Security       | Row Level Security (RLS)       |
| Documentation  | GitHub Markdown                |
| Test Cases     | 45                             |
| Categories     | 9                              |

## Project Objectives

- Validate REST API endpoints and HTTP methods.
- Test positive and negative API scenarios.
- Verify request and response validation.
- Test authentication and JWT token lifecycle.
- Validate authorization and user-level data isolation.
- Identify potential security issues.
- Verify API response structure and headers.
- Evaluate basic API response time and reliability.
- Document test cases and execution results.

## Test Coverage

The project contains 45 test cases organized into 9 categories.

| Folder | Category                         | Test Cases |
| ------ | -------------------------------- | ---------- |
| 01     | HTTP Methods                     | TC01–TC04  |
| 02     | User Registration                | TC05–TC07  |
| 03     | Security Testing                 | TC08–TC11  |
| 04     | API Validation & Error Handling  | TC12–TC16  |
| 05     | Filtering, Sorting & Pagination  | TC17–TC21  |
| 06     | Authentication & Token Lifecycle | TC22–TC30  |
| 07     | Authorization & Data Isolation   | TC31–TC39  |
| 08     | API Contract & Schema Validation | TC40–TC42  |
| 09     | Performance & Reliability        | TC43–TC45  |

### Key Testing Scenarios

**Functional Testing**

- HTTP request methods
- User registration
- Filtering and sorting
- Pagination
- API response validation

**Security Testing**

- Missing and invalid API keys
- SQL injection filter payload
- Unauthorized database operations
- Authentication and authorization
- JWT expiration and refresh
- Logout behavior
- User-level data isolation
- RLS policy enforcement
- User ID spoofing attempts

**API Contract Testing**

- JSON Schema validation
- Authentication response validation
- HTTP response header validation

**Performance & Reliability**

- API response time validation
- Repeated sequential requests
- API error handling and recovery

## Tools & Technologies

| Tool        | Purpose                                                 |
| ----------- | ------------------------------------------------------- |
| Postman     | API requests and automated assertions                   |
| Supabase    | Backend, REST API and authentication                    |
| PostgreSQL  | Database and SQL validation                             |
| JavaScript  | Postman test scripts                                    |
| GitHub      | Version control and test documentation                  |
| Postman CLI | Command-line test execution (next implementation stage) |

## Project Structure

```text
api-testing-project/
│
├── test-cases/
│   ├── 01 - HTTP Methods/
│   ├── 02 - User Registration/
│   ├── 03 - Security Testing/
│   ├── 04 - API Validation & Error Handling/
│   ├── 05 - Filtering, Sorting & Pagination/
│   ├── 06 - Authentication & Token Lifecycle/
│   ├── 07 - Authorization & Data Isolation/
│   ├── 08 - API Contract & Schema Validation/
│   └── 09 - Performance & Reliability/
│
└── README.md
```

## Test Environment

The project uses a Supabase backend with dedicated PostgreSQL tables created for API testing.

Main test tables:

- `qa_sql_injection_test`
- `qa_rls_protected_test`
- `qa_user_data_test`

Row Level Security (RLS) policies are used to validate access restrictions and user-level data isolation.

The `qa_user_data_test` table contains separate test records associated with different authenticated users. This allows verification that users can access and modify only their own records.

## How to Run the Tests

### 1. Requirements

- Postman
- Access to a Supabase project
- A valid Supabase API key
- Test user credentials

### 2. Configure Environment Variables

Create a Postman Environment and configure the following variables:

| Variable        | Description                  |
| --------------- | ---------------------------- |
| `baseUrl`       | Supabase project URL         |
| `apiKey`        | Supabase public API key      |
| `loginEmail`    | Test user email              |
| `loginPassword` | Test user password           |
| `accessToken`   | Authentication access token  |
| `refreshToken`  | Authentication refresh token |

Additional variables are used by specific test cases for token lifecycle and test execution.

### 3. Execute API Requests

1. Select the configured Postman Environment.
2. Open the required test request.
3. Execute the request using Send.
4. Review the HTTP status code and response body.
5. Review automated assertions in the Postman Test Results tab.
6. Compare actual results with expected results documented in the corresponding test case.

**Important:** Never commit real passwords, access tokens, refresh tokens, or secret API keys to GitHub.

## Notable Testing Findings

The project includes practical observations about Supabase API behavior:

- RLS can return HTTP 200 with an empty array when a user attempts to access rows that are not visible to them.
- Unauthorized INSERT operations violating an RLS `WITH CHECK` policy can return HTTP 403.
- JWT expiration and logout behavior are different scenarios and must be tested separately.
- An access JWT may remain accepted by the REST API after logout until it expires, depending on token verification behavior.
- API response time can vary due to network conditions and server load.

These findings demonstrate the importance of validating actual API behavior instead of relying only on assumptions.

##

## Skills Demonstrated

- API Testing
- REST API
- HTTP Methods and Status Codes
- Postman
- JavaScript Test Scripts
- SQL and PostgreSQL
- Supabase
- Authentication and JWT
- Authorization
- Row Level Security (RLS)
- Negative Testing
- Security Testing
- JSON Schema Validation
- API Documentation
- Git and GitHub

## Author

**QA Engineer – API Testing Portfolio**

This repository is a practical demonstration of API testing knowledge, test design, security validation, and quality assurance practices.


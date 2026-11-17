# User Registration – Test Cases

## TC01 – Successful Registration

**Test Case ID:** REG-001

**Title:** Successful registration with valid email and password

**Priority:** High

**Preconditions:**
- Supabase project is available.
- Email registration provider is enabled.
- A new email address is used for registration.

**Test Data:**
- Email: `api.testing.qa1311@yopmail.com`
- Password: Test password (not stored in this document)
- Name: `Anna`

**Steps:**
1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to `POST`.
4. Send a request to `{{baseUrl}}/auth/v1/signup`.
5. Provide a valid email and password in the JSON request body.
6. Check the API response.

**Expected Result:**
- API returns status code `200 OK`.
- Response contains a non-empty user ID.
- Response contains the correct email address.
- User role is `authenticated`.
- Email confirmation status is `false` because email verification is pending.

**Actual Result:**
- API returned `200 OK`.
- Response contained a valid user ID.
- Response email matched the test email.
- User role was `authenticated`.
- `user_metadata.email_verified` was `false`.
- User metadata contained the name `Anna`.

**Postman Test Results:** 5/5 Passed

**Status:** Passed

**Notes:** Supabase accepted the registration request and returned the created user data. Email confirmation was pending at the time of the test.

---

## TC02 – Password Less Than Minimum Length

**Test Case ID:** REG-002

**Title:** Registration with password shorter than the minimum allowed length

**Priority:** High

**Test Type:** Negative

**Preconditions:**
- Supabase project is available.
- Email registration provider is enabled.
- Minimum password length is configured to 6 characters.
- A new test email address is available.

**Test Data:**
- Email: `api.testing.qa1711@yopmail.com`
- Password: `Qa1!a` (5 characters)

**Steps:**
1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to POST.
4. Send a request to `{{baseUrl}}/auth/v1/signup`.
5. Enter a valid email and a 5-character password.
6. Check the API response.

**Expected Result:**
- API rejects the registration request.
- API returns status code `422 Unprocessable Entity`.
- Response contains an error message.
- User is not created.

**Actual Result:**
- API returned `422 Unprocessable Entity`.
- Response contained an error message.
- Registration was rejected.

**Postman Test Results:** 3/3 Passed

**Status:** Passed

**Notes:** Supabase correctly rejected a password shorter than the configured minimum length of 6 characters.

## TC03 – Password Exactly Minimum Length

**Test Case ID:** REG-003

**Title:** Registration with password matching the minimum allowed length

**Priority:** High

**Test Type:** Positive / Boundary Value

**Preconditions:**

- Supabase project is available.
- Email registration provider is enabled.
- Minimum password length is configured to 6 characters.
- A new test email address is available.

**Test Data:**

- Email: `api.testing.qa1711@yopmail.com`
- Password: `Qa1!aB` (6 characters)

**Steps:**

1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to POST.
4. Send a request to `{{baseUrl}}/auth/v1/signup`.
5. Enter a valid email and a password containing exactly 6 characters.
6. Check the API response.

**Expected Result:**

- API accepts the registration request.
- API returns status code `200 OK`.
- Response contains a user ID.
- Response contains the registered email address.
- Response contains the user role `authenticated`.
- Registration is successful.

**Actual Result:**

- API returned `200 OK`.
- Response contained a user ID.
- Response contained the registered email address.
- Response contained the user role `authenticated`.
- Registration was successful.

**Postman Test Results:** 4/4 Passed

**Status:** Passed

**Notes:** Supabase accepted a password matching the configured minimum password length of 6 characters. All Postman assertions passed.

## TC04 – Password Greater Than Minimum Length

**Test Case ID:** REG-004

**Title:** Registration with password longer than the minimum allowed length

**Priority:** High

**Test Type:** Positive

**Preconditions:**

- Supabase project is available.
- Email registration provider is enabled.
- Minimum password length is configured to 6 characters.
- A unique test email address is available.
- The Supabase Auth rate limit allows the registration request.

**Test Data:**

- Email: `api.testing.qa17211@yopmail.com` 
- Password: `Qa1!aBcd` (8 characters)

**Steps:**

1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to POST.
4. Enter the endpoint: `{{baseUrl}}/auth/v1/signup`.
5. Add the required headers:
   - `apikey: {{apiKey}}`
   - `Content-Type: application/json`
6. Select Body → raw → JSON.
7. Enter a valid email address and an 8-character password.
8. Send the request.
9. Verify the API response and Postman test results.

Expected Result

API accepts the password because it exceeds the configured minimum length.

Password validation is successful.

API processes the registration request successfully.

No weak password validation error is returned.

User registration is successful according to the configured email confirmation behavior.

Actual Result

Initial execution was temporarily affected by Supabase Auth rate limiting (HTTP 429 Too Many Requests).

After the rate limit condition cleared, the request was executed again.

The test passed successfully.

Password validation was accepted.

HTTP Status Code: Record the status code from the successful Postman execution.

Postman Test Results

Response status is not a server error – PASSED

Response body is valid JSON – PASSED

Response time is recorded – PASSED

Password exceeds minimum required length – PASSED

No weak password validation error – PASSED

Status

PASS

## TC05 – Registration with Already Registered Email

**Test Case ID:** REG-005

**Title:** Registration using an email address that is already registered

**Priority:** High

**Test Type:** Negative

**Preconditions:**

- Supabase project is available.
- Email registration provider is enabled.
- A user account with the test email address already exists.
- The test account has been registered successfully.
- Supabase Auth rate limits allow the test request.

**Test Data:**

- Email: `api.testing.qa1711@yopmail.com` (already registered email)
- Password: `Qa1!aBcd` (valid password)

**Steps:**

1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to POST.
4. Enter the endpoint: `{{baseUrl}}/auth/v1/signup`.
5. Add the required headers:
   - `apikey: {{apiKey}}`
   - `Content-Type: application/json`
6. Select Body → raw → JSON.
7. Enter the already registered email address and a valid password.
8. Send the request.
9. Check the API response, status code, and response body.

### Actual Result

- HTTP Status Code: 200 OK
- API returned a valid JSON response.
- Response time was successfully recorded.
- Existing email was used for the registration attempt.
- Supabase Authentication Users dashboard confirmed that only one user account exists for the test email.
- No duplicate user account was created.

### Postman Test Results

- Response status is not a server error – PASSED
- Response body is valid JSON – PASSED
- Response time is recorded – PASSED
- Duplicate account prevention – PASSED (manually verified in Supabase Dashboard → Authentication → Users)

### Status

**PASS**

### Notes

Supabase applied anti-enumeration behavior and returned HTTP 200 OK instead of explicitly reporting that the email address was already registered.

The absence of a duplicate account was verified manually in the Supabase Authentication Users dashboard.

# TC06 – Registration with Invalid Email Format

**Test Case ID:** TC06  
**Priority:** High  
**Test Type:** Negative Testing / API Validation

### Preconditions
- Supabase API is available.
- Valid `baseUrl` and `apiKey` are configured in Postman environment.
- Registration endpoint is accessible.

### Test Data

| Parameter | Value |
|---|---|
| Email | `test@` |
| Password | Valid test password (stored in Postman environment) |

### Request
**Method:** POST  
**Endpoint:** `{{baseUrl}}/auth/v1/signup`

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Body:**
```json
{
  "email": "test@",
  "password": "{{testPassword}}"
}
```

### Test Steps
1. Open the TC06 request in Postman.
2. Select the Supabase - API Testing environment.
3. Enter an invalid email format (`test@`).
4. Send the POST request.
5. Verify the HTTP status code.
6. Validate the response body and error message.
7. Verify that registration is unsuccessful.

### Expected Result
- API rejects the invalid email address.
- HTTP status code is 400 Bad Request.
- Response body contains a validation error.
- Error code is `validation_failed`.
- Error message indicates an invalid email format.
- User registration is unsuccessful.

### Actual Result
HTTP Status: **400 Bad Request**

Response:
```json
{
  "code": 400,
  "error_code": "validation_failed",
  "msg": "Unable to validate email address: invalid format"
}
```

### Postman Test Results
- API rejects invalid email format – PASSED
- Response body is valid JSON – PASSED
- Response contains a validation error – PASSED
- Registration is not successful – PASSED

### Status
**PASS** – Manual API negative test.

### Notes
The API correctly rejected the invalid email format and returned the expected validation error.

# TC07 – Registration with Empty Password

**Test Case ID:** REG-007

**Title:** Registration using an empty password

**Priority:** High

**Test Type:** Negative / Validation

### Preconditions

- Supabase project is available.
- Email registration provider is enabled.
- Valid `baseUrl` and `apiKey` are configured in Postman environment.
- A valid test email address is available.
- Supabase Auth rate limits allow the test request.

### Test Data

- Email: Unique test email address.
- Password: Empty string (`""`).

### Request Details

**Method:** POST

**Endpoint:** `{{baseUrl}}/auth/v1/signup`

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Request Body:**

```json
{
  "email": "new.unique.test@example.com",
  "password": ""
}
```

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to POST.
4. Enter the endpoint: `{{baseUrl}}/auth/v1/signup`.
5. Add the required request headers.
6. Select Body → raw → JSON.
7. Enter a valid test email address.
8. Leave the password field empty.
9. Send the POST request.
10. Verify the HTTP response status code.
11. Validate the response body and error message.
12. Verify that registration is rejected.

### Expected Result

- API rejects registration with an empty password.
- HTTP status code is 400 Bad Request.
- Response body contains a validation error.
- Error code is `validation_failed`.
- Error message indicates that a valid password is required.
- User registration is unsuccessful.

### Actual Result

- HTTP Status Code: 400 Bad Request
- Error Code: `validation_failed`
- Error Message: `Signup requires a valid password`
- API rejected registration with an empty password.

### Postman Test Results

- Status code is 400 – PASSED
- Response body is valid JSON – PASSED
- API rejects empty password – PASSED
- Registration is not successful – PASSED
- Response time is recorded – PASSED

### Status

**PASS**

### Notes

The API correctly rejected the registration request because the password field was empty and returned a validation error.

No successful registration occurred.

# TC08 – Registration Without API Key

**Test Case ID:** REG-008

**Title:** Registration request without API Key

**Priority:** High

**Test Type:** Negative / API Security

### Preconditions

- Supabase project is available.
- Email registration provider is enabled.
- Supabase API endpoint is accessible.
- Valid `baseUrl` is configured in Postman environment.
- API key is intentionally omitted from the request.

### Test Data

- Email: Unique test email address.
- Password: Valid test password.
- API Key: Not provided.

### Request Details

**Method:** POST

**Endpoint:** `{{baseUrl}}/auth/v1/signup`

**Headers:**
- `Content-Type: application/json`
- `apikey`: intentionally omitted.

### Request Body

```json
{
  "email": "{{email}}",
  "password": "{{testPassword}}"
}
```

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to POST.
4. Enter the registration endpoint.
5. Add the Content-Type header.
6. Disable or remove the `apikey` header.
7. Select Body → raw → JSON.
8. Enter valid registration data.
9. Send the request.
10. Verify the HTTP status code.
11. Validate the response body and error message.
12. Verify that the API rejects the request.

### Expected Result

- API Gateway rejects the request because the API key is missing.
- HTTP status code is 401 Unauthorized.
- Response body contains an appropriate error message.
- Response indicates that no API key was found.
- Registration request is not processed.

### Actual Result

**HTTP Status Code:** 401 Unauthorized

**Response Body:**

```json
{
  "message": "No API key found in request",
  "hint": "No `apikey` request header or url param was found."
}
```

The API correctly rejected the request without an API key.

### Postman Test Results

- Status code is 401 Unauthorized – PASSED
- Response body is valid JSON – PASSED
- API rejects request when API key is missing – PASSED
- Response contains missing API key hint – PASSED
- Response time is recorded – PASSED

### Status

**PASS**

### Notes

The Supabase API Gateway successfully prevented access to the registration endpoint when the required API key was not provided.

This test verifies API authentication at the gateway level.

# TC09 – SQL Injection Testing – Filter Payload

**Test Case ID:** TC09

**Title:** SQL Injection Testing through REST API Filter Parameter

**Priority:** High

**Test Type:** Security Testing / Negative

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- API key is configured in Postman environment.
- Row Level Security (RLS) is enabled.
- Test table `qa_sql_injection_test` exists.
- Three dummy records are available in the test table.
- Baseline GET request successfully returns all three records.

### Test Data

**Test table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

**SQL-like filter payload:**

`' OR '1'='1`

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test
```

**Query Parameters:**

- `select=id,product_name,description`
- `product_name=eq.' OR '1'='1` (URL-encoded in the actual request)

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Verify that the baseline GET request returns three test records.
4. Create a separate GET request for SQL Injection testing.
5. Enter the test table endpoint.
6. Add the required request headers.
7. Add a SQL-like payload to the `product_name` filter parameter.
8. Send the GET request.
9. Verify the HTTP response status code.
10. Validate the response body.
11. Verify that the SQL-like input does not bypass the filter and return all records.
12. Check the response time.

### Expected Result

- API processes the request without executing the SQL-like input as an SQL command.
- The filter does not return all records.
- No unexpected records are exposed.
- Response body is valid JSON.
- The API does not reveal or expose additional test records through the supplied payload.

### Actual Result

**HTTP Status Code:** 200 OK

**Response Body:**

```json
[]
```

The API returned an empty JSON array.

The SQL-like filter payload did not cause the API to return all three existing test records.

### Postman Test Results

- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- SQL injection payload does not return all records – PASSED
- No unexpected test records are returned – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 6/6 PASSED

### Status

**PASS**

### Notes

The tested SQL-like input did not bypass the REST API filtering behavior.

The API returned an empty array instead of exposing all available test records.

This test covers one SQL Injection-like payload against a specific REST API filter parameter. It does not constitute a comprehensive SQL Injection security assessment.

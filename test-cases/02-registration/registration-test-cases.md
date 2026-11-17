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

**Expected Result:**

- API accepts the registration request.
- API returns a successful response (expected `200 OK`, subject to the configured Supabase Auth behavior).
- Response contains the registered user's information.
- Returned email matches the submitted email.
- User is assigned the appropriate authenticated role.
- Registration is processed successfully.

**Actual Result:**

- API returned `429 Too Many Requests`.
- Registration could not be verified because the request was rate-limited.
- Successful registration was not confirmed.

**Postman Test Results:** Not Passed / Not Verified — rate limit response (`429`).

**Status:** Blocked

**Notes:** The test execution was blocked by an API rate limit. The `429 Too Many Requests` response does not indicate that password validation failed. The test must be re-executed when the applicable rate limit allows requests. No successful registration result has been confirmed yet.

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

**Expected Result:**

- API processes the registration attempt according to the configured Supabase Auth behavior.
- The API must not create a second account for the same email address.
- The response must follow the configured duplicate-email and anti-enumeration behavior.
- No duplicate user account should be created.

**Actual Result:**

Not executed yet.

**Postman Test Results:**

Not executed yet.

**Status:** Not Run

**Notes:** Supabase may use anti-enumeration behavior, so the response for an already registered email can depend on email confirmation settings and Auth configuration. Verify the actual response before documenting a specific status code or error message.

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



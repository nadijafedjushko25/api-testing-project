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


# TC10 – Unauthorized INSERT – Insufficient Table Privileges

**Test Case ID:** TC10

**Title:** Verify that the anon role cannot insert records into the protected test table

**Priority:** High

**Test Type:** Negative / Security Testing / Access Control

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- A valid Supabase `anon` API key is configured in Postman.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Row Level Security (RLS) is enabled on the table.
- The `anon` role has SELECT permission only.
- INSERT, UPDATE and DELETE privileges are not granted to the `anon` role.
- Three dummy test records exist in the table.

### Test Data

**Table:** `public.qa_sql_injection_test`

**API Role:** `anon`

**Operation:** INSERT

**Test Record:**

```json
{
  "product_name": "TC10 Unauthorized Insert",
  "description": "This record should not be inserted"
}
```

### Request Details

**Method:** POST

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Request Body:**

```json
{
  "product_name": "TC10 Unauthorized Insert",
  "description": "This record should not be inserted"
}
```

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to POST.
4. Enter the test table REST API endpoint.
5. Add the required request headers using the public anon API key.
6. Select Body → raw → JSON.
7. Enter the test record data.
8. Send the POST request.
9. Verify the HTTP response status code.
10. Validate the response body.
11. Verify the PostgreSQL error code and error message.
12. Verify that the INSERT operation is rejected due to insufficient table privileges.

### Expected Result

- API rejects the INSERT request.
- The `anon` role cannot insert records into the protected test table.
- HTTP response indicates an access restriction.
- PostgreSQL returns an insufficient privileges error.
- No new record should be inserted into the table.

### Actual Result

**HTTP Status Code:** 401 Unauthorized

**PostgreSQL Error Code:** `42501`

**Response Body:**

```json
{
  "code": "42501",
  "details": null,
  "hint": "Grant the required privileges to the current role with: GRANT INSERT ON public.qa_sql_injection_test TO anon;",
  "message": "permission denied for table qa_sql_injection_test"
}
```

The API rejected the INSERT operation because the `anon` role does not have the required INSERT privilege on the table.

### Postman Test Results

- Status code is 401 Unauthorized – PASSED
- Response body is valid JSON – PASSED
- API rejects unauthorized INSERT operation – PASSED
- INSERT permission is not granted to anon role – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Status

**PASS**

### Notes

The test confirms that PostgreSQL table-level privileges prevent the `anon` role from inserting records into the test table.

RLS is enabled, and the configured policy allows read access only. However, this specific INSERT attempt was rejected at the table privilege level before an INSERT RLS policy could be evaluated.

This test demonstrates the principle of least privilege and helps verify access restrictions for the Supabase REST API.

A separate verification of the table contents can be used to confirm that the original three records remain unchanged.

# TC11 – RLS – Unauthorized Data Access

**Test Case ID:** TC11

**Title:** Verify that Row Level Security prevents anonymous access to protected records

**Priority:** High

**Test Type:** Security Testing / Authorization / RLS

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Public `anon` API key is configured in Postman.
- Table `qa_rls_protected_test` exists in the `public` schema.
- Row Level Security (RLS) is enabled on the table.
- Three dummy test records exist in the table.
- The `anon` role has SELECT table privilege.
- No SELECT policy is configured for the `anon` role.
- Postman request uses the `anon` API key without a user access token.

### Test Data

**Table:** `public.qa_rls_protected_test`

**API Role:** `anon`

**Expected accessible records:** 0

**Existing test records:**
- QA Record 1
- QA Record 2
- QA Record 3

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_rls_protected_test?select=*
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the protected table REST API endpoint.
5. Add the required request headers using the public anon API key.
6. Ensure that no user access token is provided.
7. Send the GET request.
8. Verify the HTTP response status code.
9. Validate the response body.
10. Verify that the response is an empty array.
11. Confirm that no restricted test records are exposed.
12. Check the response time.

### Expected Result

- API returns HTTP 200 OK.
- Response body is valid JSON.
- Response is an empty array.
- Anonymous API client cannot access protected table records.
- No restricted records are exposed.
- RLS policies prevent unauthorized row access.

### Actual Result

**HTTP Status Code:** 200 OK

**Response Body:**

```json
[]
```

The API returned an empty JSON array.

Although three records exist in the table, the anonymous API client could not retrieve them because no SELECT policy was configured for the `anon` role.

### Postman Test Results

- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- RLS prevents anon access to protected records – PASSED
- No restricted records are exposed – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 6/6 PASSED

### Status

**PASS**

### Notes

This test verifies Row Level Security (RLS) enforcement for anonymous API access.

The `anon` role has table-level SELECT permission, but no applicable SELECT policy exists. Therefore, PostgreSQL RLS prevents the role from accessing table rows.

The API returns an empty array rather than exposing restricted data.

This test demonstrates the difference between table-level privileges and row-level access control.

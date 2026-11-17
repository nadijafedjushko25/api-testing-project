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

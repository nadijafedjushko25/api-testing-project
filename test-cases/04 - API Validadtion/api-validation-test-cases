# TC12 – Invalid Query Filter Syntax

**Test Case ID:** TC12

**Title:** Verify API handling of invalid query filter syntax

**Priority:** Medium

**Test Type:** Negative Testing / API Validation / Error Handling

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Test records are available in the table.

### Test Data

**Table:** `qa_sql_injection_test`

**Invalid filter:**

```text
id=badoperator.1
```

**Expected valid operator:** `eq`

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&id=badoperator.1
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the REST API endpoint.
5. Add the required request headers.
6. Add an invalid operator (`badoperator`) to the `id` query filter.
7. Send the GET request.
8. Verify the HTTP response status code.
9. Validate the response body.
10. Verify the API error code.
11. Check the error message and parsing details.
12. Verify that the API handles the invalid query parameter correctly.

### Expected Result

- API rejects the request with an invalid filter operator.
- HTTP status code is 400 Bad Request.
- Response body is valid JSON.
- Error code is `PGRST100`.
- Response contains details about the invalid filter syntax.
- API does not return regular table records.

### Actual Result

**HTTP Status Code:** 400 Bad Request

**Response Body:**

```json
{
  "code": "PGRST100",
  "details": "unexpected \"b\" expecting \"not\" or operator (eq, gt, ...)",
  "hint": null,
  "message": "\"failed to parse filter (badoperator.1)\" (line 1, column 1)"
}
```

The API correctly rejected the request because the supplied filter operator was invalid.

### Postman Test Results

- Status code is 400 – Expected
- Response body is valid JSON – Expected
- API returns invalid filter syntax error (`PGRST100`) – Expected
- Response contains filter parsing details – Expected
- Response time is recorded – Pending confirmation of assertion execution

### Status

**PASS – Manual response validation**

### Notes

The test verifies API input validation and error handling for an incorrectly formatted query filter.

The invalid operator `badoperator` was rejected by PostgREST.

The valid equality operator is `eq`, for example:

`id=eq.1`

The API returned HTTP 400 with error code `PGRST100`, indicating that the filter expression could not be parsed.

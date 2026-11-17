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

# TC13 – Invalid Query Parameter Data Type

**Test Case ID:** TC13

**Title:** Verify API handling of invalid data type in query parameter

**Priority:** Medium

**Test Type:** Negative Testing / API Validation / Error Handling

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- The `id` column has the BIGINT data type.
- Test records are available in the table.

### Test Data

**Table:** `qa_sql_injection_test`

**Column:** `id`

**Expected data type:** BIGINT

**Invalid filter value:** `not-a-number`

**Query parameter:**

```text
id=eq.not-a-number
```

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&id=eq.not-a-number
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
6. Set the `id` query parameter to `eq.not-a-number`.
7. Send the GET request.
8. Verify the HTTP response status code.
9. Validate the response body.
10. Verify the PostgreSQL error code.
11. Check the error message for the invalid data type.
12. Verify that the API correctly rejects the invalid query parameter.

### Expected Result

- API rejects the request because the `id` column expects a numeric value.
- HTTP status code is 400 Bad Request.
- Response body is valid JSON.
- PostgreSQL error code is `22P02`.
- Error message indicates invalid input syntax for type BIGINT.
- API does not return regular table records.

### Actual Result

**HTTP Status Code:** 400 Bad Request

**Response Body:**

```json
{
  "code": "22P02",
  "details": null,
  "hint": null,
  "message": "invalid input syntax for type bigint: \"not-a-number\""
}
```

The API correctly rejected the request because the supplied text value could not be converted to BIGINT.

### Postman Test Results

- Status code is 400 – PASSED
- Response body is valid JSON – PASSED
- API rejects invalid data type – PASSED
- Response contains invalid BIGINT error message – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Status

**PASS**

### Notes

The test verifies API validation and error handling when a query parameter contains a value that does not match the database column data type.

The `id` column expects a BIGINT value, but the request supplied the text value `not-a-number`.

PostgreSQL returned error code `22P02` (invalid text representation), and the API correctly rejected the request with HTTP 400 Bad Request.

# TC14 – Unsupported Content-Type Header

**Test Case ID:** TC14

**Title:** Verify API rejects requests with an unsupported Content-Type header

**Priority:** Medium

**Test Type:** Negative Testing / API Validation / Error Handling

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- The `anon` role is used for the API request.
- The request body contains JSON-formatted data.
- The Content-Type header is intentionally set to an unsupported media type.

### Test Data

| Parameter | Value |
|---|---|
| HTTP Method | POST |
| Content-Type | `text/plain` |
| Expected Content-Type | `application/json` |
| Table | `qa_sql_injection_test` |
| API Role | `anon` |

### Request Details

**Method:** POST

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test
```

**Headers:**

- `apikey: {{apiKey}}`
- `Content-Type: text/plain`

**Authorization:** No Auth

### Request Body

Body → raw → Text

```text
{
  "product_name": "TC14 Unsupported Content Type",
  "description": "Testing unsupported media type"
}
```

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to POST.
4. Enter the REST API endpoint.
5. Add the public API key header.
6. Set the Content-Type header to `text/plain`.
7. Select Body → raw → Text.
8. Enter the JSON-formatted request body.
9. Send the POST request.
10. Verify the HTTP response status code.
11. Validate the response body.
12. Verify the PostgREST error code.
13. Check the error message.
14. Verify that the API rejects the unsupported media type.

### Expected Result

- API rejects the request because `text/plain` is not an accepted Content-Type for this table operation.
- HTTP status code is 400 Bad Request.
- Response body is valid JSON.
- Error code is `PGRST102`.
- Response message indicates that the Content-Type is not acceptable.
- The INSERT operation is not processed.

### Actual Result

**HTTP Status Code:** 400 Bad Request

**Response Body:**

```json
{
  "code": "PGRST102",
  "details": null,
  "hint": null,
  "message": "Content-Type not acceptable: text/plain"
}
```

The API rejected the request because the supplied Content-Type (`text/plain`) was not acceptable.

### Postman Test Results

- Status code is 400 – Pending confirmation after corrected assertion
- Response body is valid JSON – PASSED
- API rejects unsupported Content-Type (`PGRST102`) – Pending confirmation after corrected assertion
- Response contains error message – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5

### Status

**PASS – Manual response validation**

Automated assertions for the corrected HTTP status and error code require a final re-run.

### Notes

This test verifies API request content-type validation.

The request contains JSON-formatted data but declares `text/plain` as its Content-Type.

The API rejected the request with HTTP 400 Bad Request and error code `PGRST102`.

This confirms that the API does not accept the supplied media type for this table operation.

The test uses the public anon API key and does not require elevated database privileges.

# TC15 – Malformed JSON Request Body

**Test Case ID:** TC15

**Title:** Verify API handling of malformed JSON request body

**Priority:** Medium

**Test Type:** Negative Testing / API Validation / Error Handling

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- The request uses `Content-Type: application/json`.
- The request body intentionally contains invalid JSON syntax.

### Test Data

**HTTP Method:** POST

**Content-Type:** `application/json`

**Invalid JSON:** Missing comma between object properties.

### Request Details

**Method:** POST

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Request Body

Body → raw → Text

```json
{
  "product_name": "TC15 Malformed JSON"
  "description": "Missing comma between fields"
}
```

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to POST.
4. Enter the REST API endpoint.
5. Add the required request headers.
6. Set Content-Type to `application/json`.
7. Select Body → raw → Text.
8. Enter a JSON-formatted body with a missing comma between properties.
9. Send the POST request.
10. Verify the HTTP response status code.
11. Validate the response body.
12. Verify the PostgREST error code.
13. Check the JSON parsing error details.
14. Verify that the API rejects the malformed request.

### Expected Result

- API rejects the malformed JSON request body.
- HTTP status code is 400 Bad Request.
- Response body is valid JSON.
- PostgREST error code is `PGRST102`.
- Response contains information about the malformed request body.
- The INSERT operation is not processed.

### Actual Result

**HTTP Status Code:** 400 Bad Request

The API rejected the malformed JSON request because a comma was missing between object properties.

The response contained a JSON error description. PostgREST returned the expected error code `PGRST102`.

### Postman Test Results

- Status code is 400 – PASSED
- Response body is valid JSON – PASSED
- API rejects malformed JSON body – PASSED
- Response contains JSON parsing error details – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Status

**PASS**

### Notes

This test verifies API request body validation.

The request uses the correct Content-Type (`application/json`) but contains invalid JSON syntax because a comma is missing between two properties.

PostgREST correctly detected the malformed JSON and returned HTTP 400 Bad Request with error code `PGRST102`.

The test confirms that the API rejects malformed request bodies instead of processing the database operation.

# TC16 – Non-existent API Endpoint

**Test Case ID:** TC16

**Title:** Verify API handling of requests to a non-existent resource

**Priority:** Medium

**Test Type:** Negative Testing / API Validation / Error Handling

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- The table `qa_nonexistent_endpoint` does not exist in the `public` schema.
- The request uses the public anon API key.

### Test Data

**HTTP Method:** GET

**Non-existent resource:** `qa_nonexistent_endpoint`

**Schema:** `public`

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_nonexistent_endpoint?select=*
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the REST API endpoint pointing to a non-existent table.
5. Add the required request headers.
6. Send the GET request.
7. Verify the HTTP response status code.
8. Validate the response body.
9. Verify the PostgREST error code.
10. Check the error message.
11. Confirm that the API identifies the requested table as unavailable.

### Expected Result

- API rejects the request to the non-existent resource.
- HTTP status code is 404 Not Found.
- Response body is valid JSON.
- PostgREST returns error code `PGRST205`.
- Error message indicates that the requested table cannot be found in the schema cache.
- No regular table records are returned.

### Actual Result

**HTTP Status Code:** 404 Not Found

**Response Body:**

```json
{
  "code": "PGRST205",
  "details": null,
  "hint": null,
  "message": "Could not find the table 'public.qa_nonexistent_endpoint' in the schema cache"
}
```

The API correctly returned a resource-not-found response for the non-existent table.

### Postman Test Results

- Status code is 404 – PASSED
- Response body is valid JSON – PASSED
- API returns PGRST205 error – PASSED
- Response identifies non-existent table – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED after automated test execution.

### Status

**PASS**

### Notes

This test verifies API error handling when a client requests a resource that does not exist in the Supabase public schema.

PostgREST returned HTTP 404 Not Found with error code `PGRST205`.

The response clearly identifies the missing table in the schema cache.

# TC17 – Filter by Exact Value

**Test Case ID:** TC17

**Title:** Verify API filtering by exact field value

**Priority:** Medium

**Test Type:** Functional Testing / API Filtering / Positive

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Test records are available in the table.
- Row Level Security allows the anon role to read the test records.

### Test Data

**Table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

**Filter Parameter:**

`product_name=eq.Laptop`

**Expected Matching Record:** Laptop

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&product_name=eq.Laptop
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
6. Set the query parameter `product_name` to `eq.Laptop`.
7. Send the GET request.
8. Verify the HTTP response status code.
9. Validate the response body.
10. Verify that the response is an array.
11. Check that only the matching record is returned.
12. Verify the returned record ID and product name.
13. Check the response time.

### Expected Result

- API successfully processes the GET request.
- HTTP status code is 200 OK.
- Response body is valid JSON.
- Response contains exactly one record.
- The returned record has `id = 1`.
- The returned product name is `Laptop`.
- No non-matching records are returned.

### Actual Result

**HTTP Status Code:** 200 OK

**Response Body:**

```json
[
  {
    "id": 1,
    "product_name": "Laptop"
  }
]
```

The API successfully filtered the records by the exact product name and returned only the matching record.

### Postman Test Results

- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- API returns only the matching record – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Status

**PASS**

### Notes

The test verifies exact value filtering using the PostgREST `eq` operator.

The API correctly returned only the record where `product_name` equals `Laptop`.

The test confirms that exact value filtering works as expected.

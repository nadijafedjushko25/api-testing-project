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

# TC18 – Filter by Comparison Operator

**Test Case ID:** TC18

**Title:** Verify API filtering using a greater-than comparison operator

**Priority:** Medium

**Test Type:** Functional Testing / API Filtering / Positive

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Test records are available in the table.
- Row Level Security allows the `anon` role to read test records.

### Test Data

**Table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

**Filter Parameter:**

`id=gt.1`

**Comparison Operator:** `gt` (Greater Than)

**Expected Matching Records:**
- ID 2 – Keyboard
- ID 3 – Monitor

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&id=gt.1
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

**Body:** Not required.

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the REST API endpoint.
5. Add the required request headers.
6. Set the `id` query parameter to `gt.1`.
7. Send the GET request.
8. Verify the HTTP response status code.
9. Validate the response body.
10. Verify that the response is an array.
11. Check that only records with IDs greater than 1 are returned.
12. Verify the returned record IDs and product names.
13. Check the response time.

### Expected Result

- API successfully processes the GET request.
- HTTP status code is 200 OK.
- Response body is valid JSON.
- Response contains exactly two records.
- The returned IDs are 2 and 3.
- The returned product names are Keyboard and Monitor.
- No record with ID 1 is returned.
- All returned IDs are greater than 1.

### Actual Result

**HTTP Status Code:** 200 OK

**Response Body:**

```json
[
  {
    "id": 2,
    "product_name": "Keyboard"
  },
  {
    "id": 3,
    "product_name": "Monitor"
  }
]
```

The API successfully applied the greater-than filter and returned only records with IDs greater than 1.

### Postman Test Results

- Status code is 200 – Pending automated assertion confirmation
- Response body is valid JSON – Pending automated assertion confirmation
- Response is an array – Pending automated assertion confirmation
- API returns only records with ID greater than 1 – Pending automated assertion confirmation
- Response time is recorded – Pending automated assertion confirmation

**Total Assertions:** 5

### Status

**PASS – Manual response validation**

# TC19 – Sort Records (Ascending and Descending)

**Test Case ID:** TC19

**Title:** Verify API sorting in ascending and descending order

**Priority:** Medium

**Test Type:** Functional Testing / API Sorting / Positive

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Three test records are available in the table.
- Row Level Security allows the anon role to read test records.

### Test Data

**Table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

### Request Details

**Method:** GET

**Base Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Test Scenario 1 – Ascending Order (ASC)

**Request URL:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&order=product_name.asc
```

**Test Steps:**

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the endpoint with `order=product_name.asc`.
5. Add the required request headers.
6. Send the GET request.
7. Verify the HTTP response status code.
8. Validate the response body.
9. Verify that records are sorted alphabetically from A to Z.
10. Check the response time.

**Expected Result:**

- HTTP status code is 200 OK.
- Response body is valid JSON.
- All three records are returned.
- Records are sorted alphabetically in ascending order:
  1. Keyboard
  2. Laptop
  3. Monitor

**Actual Result:**

HTTP Status Code: 200 OK

Response:

```json
[
  {
    "id": 2,
    "product_name": "Keyboard"
  },
  {
    "id": 1,
    "product_name": "Laptop"
  },
  {
    "id": 3,
    "product_name": "Monitor"
  }
]
```

Ascending sorting was verified successfully.

**Postman Test Results:**
- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- Records are sorted alphabetically ASC – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Test Scenario 2 – Descending Order (DESC)

**Request URL:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&order=product_name.desc
```

**Test Steps:**

1. Open the TC19 DESC request in Postman.
2. Select the `Supabase - API Testing` environment.
3. Verify the request method is GET.
4. Enter the endpoint with `order=product_name.desc`.
5. Add the required request headers.
6. Send the GET request.
7. Verify the HTTP response status code.
8. Validate the response body.
9. Verify that records are sorted alphabetically from Z to A.
10. Check the response time.

**Expected Result:**

- HTTP status code is 200 OK.
- Response body is valid JSON.
- All three records are returned.
- Records are sorted alphabetically in descending order:
  1. Monitor
  2. Laptop
  3. Keyboard

**Actual Result:**

HTTP Status Code: 200 OK

Response:

```json
[
  {
    "id": 3,
    "product_name": "Monitor"
  },
  {
    "id": 1,
    "product_name": "Laptop"
  },
  {
    "id": 2,
    "product_name": "Keyboard"
  }
]
```

Descending sorting was verified successfully.

**Postman Test Results:**
- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- Records are sorted alphabetically DESC – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Overall Status

**PASS**

**Total Requests:** 2

**Total Assertions:** 10/10 PASSED

### Notes

This test verifies sorting functionality using the PostgREST `order` query parameter.

The `asc` direction sorts records in ascending alphabetical order, while `desc` sorts records in descending alphabetical order.

Both sorting directions were successfully verified using the same test dataset.

# TC20 – Limit Number of Records

**Test Case ID:** TC20

**Title:** Verify API limits the number of returned records

**Priority:** Medium

**Test Type:** Functional Testing / API Filtering / Positive

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Three test records are available in the table.
- Row Level Security allows the anon role to read test records.

### Test Data

**Table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

**Query Parameters:**
- `order=id.asc`
- `limit=2`

**Expected Number of Records:** 2

### Request Details

**Method:** GET

**Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&order=id.asc&limit=2
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

**Body:** Not required.

### Test Steps

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the REST API endpoint.
5. Add the required request headers.
6. Set the `order` parameter to `id.asc`.
7. Set the `limit` parameter to `2`.
8. Send the GET request.
9. Verify the HTTP response status code.
10. Validate the response body.
11. Verify that the response is an array.
12. Check that exactly two records are returned.
13. Verify that the returned record IDs are 1 and 2.
14. Verify that the returned product names are Laptop and Keyboard.
15. Confirm that the third record is excluded.
16. Check the response time.

### Expected Result

- API successfully processes the GET request.
- HTTP status code is 200 OK.
- Response body is valid JSON.
- Response is an array.
- API returns exactly two records.
- Returned record IDs are 1 and 2.
- Returned product names are Laptop and Keyboard.
- The record with ID 3 is not included in the response.
- Response time is recorded.

### Actual Result

**HTTP Status Code:** 200 OK

The API successfully processed the GET request with `limit=2`.

The response contained exactly two matching records, sorted by ID in ascending order:

```json
[
  {
    "id": 1,
    "product_name": "Laptop"
  },
  {
    "id": 2,
    "product_name": "Keyboard"
  }
]
```

The third record (Monitor, ID 3) was excluded from the response.

### Postman Test Results

- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- API returns exactly 2 records – PASSED
- API returns the first 2 records in ascending ID order – PASSED
- Response time is recorded – PASSED

**Total Assertions:** All executed assertions PASSED.

### Status

**PASS**

### Notes

This test verifies record limitation functionality using the PostgREST `limit` query parameter.

The `limit=2` parameter restricts the response to two records.

The `order=id.asc` parameter ensures predictable record selection by sorting IDs in ascending order.

The API correctly returned the first two records and excluded the third record from the response.

# TC21 – Pagination with Offset

**Test Case ID:** TC21

**Title:** Verify API pagination using limit and offset parameters

**Priority:** Medium

**Test Type:** Functional Testing / API Pagination / Positive

### Preconditions

- Supabase project is available.
- REST API endpoint is accessible.
- Valid `baseUrl` and public `apiKey` are configured in Postman environment.
- Table `qa_sql_injection_test` exists in the `public` schema.
- Three test records are available in the table.
- Row Level Security allows the anon role to read test records.

### Test Data

**Table:** `qa_sql_injection_test`

| ID | Product Name |
|---|---|
| 1 | Laptop |
| 2 | Keyboard |
| 3 | Monitor |

**Pagination Configuration:**
- Page size: 2 records
- Page 1 offset: 0
- Page 2 offset: 2
- Sorting: `id.asc`

### Request Details

**Method:** GET

**Base Endpoint:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test
```

**Headers:**
- `apikey: {{apiKey}}`
- `Content-Type: application/json`

**Authorization:** No Auth

### Test Scenario 1 – Page 1

**Request URL:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&order=id.asc&limit=2&offset=0
```

**Test Steps:**

1. Open Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the endpoint with `limit=2` and `offset=0`.
5. Add the required request headers.
6. Send the GET request.
7. Verify the HTTP status code.
8. Validate the response body.
9. Check that exactly two records are returned.
10. Verify the returned record IDs and product names.
11. Check the response time.

**Expected Result:**

- HTTP status code is 200 OK.
- Response body is valid JSON.
- Exactly two records are returned.
- Record IDs are 1 and 2.
- Product names are Laptop and Keyboard.

**Actual Result:**

HTTP Status Code: 200 OK

The API returned the first two records:

- ID 1 – Laptop
- ID 2 – Keyboard

**Postman Test Results:**
- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- Page 1 returns records with IDs 1 and 2 – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Test Scenario 2 – Page 2

**Request URL:**

```text
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&order=id.asc&limit=2&offset=2
```

**Test Steps:**

1. Open the TC21 Page 2 request in Postman.
2. Select the `Supabase - API Testing` environment.
3. Set the request method to GET.
4. Enter the endpoint with `limit=2` and `offset=2`.
5. Add the required request headers.
6. Send the GET request.
7. Verify the HTTP status code.
8. Validate the response body.
9. Check that exactly one remaining record is returned.
10. Verify the returned record ID and product name.
11. Check the response time.

**Expected Result:**

- HTTP status code is 200 OK.
- Response body is valid JSON.
- Exactly one record is returned.
- Record ID is 3.
- Product name is Monitor.

**Actual Result:**

HTTP Status Code: 200 OK

The API returned the remaining record:

- ID 3 – Monitor

**Postman Test Results:**
- Status code is 200 – PASSED
- Response body is valid JSON – PASSED
- Response is an array – PASSED
- Page 2 returns record with ID 3 – PASSED
- Response time is recorded – PASSED

**Total Assertions:** 5/5 PASSED

### Overall Status

**PASS**

**Total Requests:** 2

**Total Assertions:** 10/10 PASSED

### Notes

This test verifies pagination functionality using the PostgREST `limit` and `offset` query parameters.

The `limit=2` parameter defines the maximum number of records per page.

The `offset=0` parameter retrieves the first page, while `offset=2` skips the first two records and retrieves the next page.

The `order=id.asc` parameter ensures a consistent record sequence.

Both pages were verified to contain the expected records without overlap. Together, they returned all three test records.

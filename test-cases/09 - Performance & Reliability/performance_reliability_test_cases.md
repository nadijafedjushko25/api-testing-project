# TC43 – API Response Time Validation

## Test Case ID
TC43

## Test Case Title
API Response Time Validation

## Category
Performance & Reliability

## Priority
Medium

## Objective
Verify that the API responds successfully within the defined response time threshold.

## Preconditions
- A valid Supabase project is available.
- A valid API key is configured in Postman.
- A valid authenticated access token is available.
- The `qa_user_data_test` table contains at least one record accessible to the authenticated user.

## Test Data
- API endpoint: `/rest/v1/qa_user_data_test`
- Expected response time: ≤ 1000 ms
- HTTP method: GET

## Request Details

**Method:** GET

**URL:**
```http
{{baseUrl}}/rest/v1/qa_user_data_test?select=id,user_id,test_data
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |
| Authorization | `Bearer {{accessToken}}` |

## Test Steps
1. Open the TC43 request in Postman.
2. Ensure that a valid access token is available.
3. Send a GET request to the API endpoint.
4. Verify that the response status code is 200 OK.
5. Check that the response time does not exceed 1000 ms.
6. Verify that the response body contains valid JSON.
7. Confirm that the response contains at least one record.

## Expected Results
- The API returns HTTP status code `200 OK`.
- The response time is less than or equal to 1000 ms.
- The response body contains valid JSON.
- The response body is an array.
- At least one user record is returned.

## Actual Results
- The API returned HTTP status code `200 OK`.
- The response time met the defined threshold of 1000 ms.
- The response body contained valid JSON.
- The response contained user data.

## Postman Test Results
- All automated assertions passed.
- Response time validation passed.
- API response structure validation passed.

## Status
**PASS**

## Notes
The 1000 ms response time threshold is a test benchmark defined for this portfolio project. It is not an official Supabase performance SLA.

Response time may vary depending on network conditions, server load, and other external factors. A single successful request does not establish overall API performance.

# TC44 – Repeated API Requests Stability

## Test Case ID
TC44

## Test Case Title
Repeated API Requests Stability

## Category
Performance & Reliability

## Priority
Medium

## Objective
Verify that the API consistently handles multiple consecutive requests without unexpected errors or response structure changes.

## Preconditions
- A valid Supabase project is available.
- A valid API key is configured in Postman.
- A valid authenticated access token is available.
- The `qa_user_data_test` table contains at least one record accessible to the authenticated user.

## Test Data
- API endpoint: `/rest/v1/qa_user_data_test`
- HTTP method: GET
- Number of consecutive requests: 5
- Expected HTTP status: 200 OK
- Response time threshold: ≤ 1000 ms per request

## Request Details

**Method:** GET

**URL:**
```http
{{baseUrl}}/rest/v1/qa_user_data_test?select=id,user_id,test_data&order=id.asc
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |
| Authorization | `Bearer {{accessToken}}` |

## Test Steps
1. Open the TC44 request in Postman.
2. Configure the Pre-request and Post-response scripts.
3. Send the same GET request five consecutive times.
4. Verify that each request returns HTTP status code `200 OK`.
5. Verify that each response time is within the defined threshold.
6. Check that every response contains a valid JSON array.
7. Verify that the expected record fields are present.
8. Review the final execution summary in Postman Console.

## Expected Results
- All five requests return HTTP status code `200 OK`.
- All five requests complete within the 1000 ms threshold.
- No unexpected request failures occur.
- Every response contains a valid JSON array.
- The expected record structure remains consistent across requests.

## Actual Results
- All 5 consecutive requests completed successfully.
- All requests returned the expected successful response.
- No failed requests were recorded.
- Response structure validation was successful.

## Postman Test Results
- Successful requests: 5
- Failed requests: 0
- Total requests: 5

## Status
**PASS**

## Notes
This test evaluates basic API stability under repeated sequential requests.

The test uses five manually triggered consecutive requests. It does not represent a concurrent load test or a comprehensive performance benchmark.

The 1000 ms response time threshold is a portfolio-defined benchmark, not an official Supabase SLA.

# TC45 – API Error Recovery

## Test Case ID
TC45

## Test Case Title
API Error Recovery After Invalid Request

## Category
Performance & Reliability

## Priority
Medium

## Objective
Verify that the API correctly handles an invalid request and continues to process subsequent valid requests without unexpected errors.

## Preconditions
- A valid Supabase project is available.
- A valid API key is configured in Postman.
- The `qa_sql_injection_test` table exists.
- The table contains a record with ID 1 and product name `Laptop`.

## Test Data

| Scenario | Input | Expected Status |
|---|---|---|
| Invalid request | `id=eq.not-a-number` | 400 Bad Request |
| Valid request | `id=eq.1` | 200 OK |

## Test Steps

### Step 1 – Send an Invalid Request

**Method:** GET

**URL:**
```http
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&id=eq.not-a-number
```

**Expected Results:**
- HTTP status code: `400 Bad Request`.
- PostgreSQL error code: `22P02`.
- Error message identifies invalid input syntax.
- Error response contains valid JSON.

### Step 2 – Send a Valid Request After the Error

**Method:** GET

**URL:**
```http
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=id,product_name&id=eq.1
```

**Expected Results:**
- HTTP status code: `200 OK`.
- Response contains valid JSON.
- Exactly one record is returned.
- Returned record has ID `1` and product name `Laptop`.

## Actual Results
- The invalid request returned HTTP `400 Bad Request` with PostgreSQL error code `22P02`.
- The subsequent valid request returned HTTP `200 OK`.
- The API successfully returned the expected Laptop record.
- No persistent request failure was observed.

## Postman Test Results

**Step 1 – Invalid Request**
- Error status validation: PASS
- PostgreSQL error code validation: PASS
- Error message validation: PASS
- JSON response validation: PASS

**Step 2 – Recovery Request**
- Successful status validation: PASS
- JSON response validation: PASS
- Expected record validation: PASS

## Status
**PASS**

## Notes
This test verifies API error handling and successful processing of a subsequent valid request.

It does not represent server restart, infrastructure recovery, or recovery from an actual service outage.

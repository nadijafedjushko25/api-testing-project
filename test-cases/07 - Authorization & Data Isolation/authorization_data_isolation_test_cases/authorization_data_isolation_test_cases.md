# TC31 – Invalid API Key

**Test Case ID:** AUTHZ-001  
**Category:** Authorization & Data Isolation  
**Priority:** High  
**Type:** Negative Testing  
**Status:** PASS

## Objective
Verify that the API rejects requests containing an invalid Supabase API Key and does not return protected resource data.

## Preconditions
- Supabase project is active.
- The `qa_sql_injection_test` table exists.
- A Postman environment is configured with a valid `baseUrl`.
- No user access token is required for this test.

## Request Details

**Method:** GET

**Endpoint:**
```http
{{baseUrl}}/rest/v1/qa_sql_injection_test?select=*
```

**Headers:**

| Key | Value |
|---|---|
| apikey | invalid_api_key_12345 |

**Authorization:** No Auth

**Body:** None

## Test Steps
1. Create a GET request to the test table endpoint.
2. Set the `apikey` header to an invalid value.
3. Do not include an Authorization Bearer token.
4. Send the request.
5. Verify the response status, format, error message, and absence of resource data.

## Expected Result
- API returns HTTP `401 Unauthorized`.
- Response is in JSON format.
- An error message indicates that the API Key is invalid.
- No table records are returned.

## Actual Result

**HTTP Status:** `401 Unauthorized`

**Response Body:**
```json
{
  "message": "Invalid API key",
  "hint": "Double check your Supabase `anon` or `service_role` API key."
}
```

## Assertions

| # | Assertion | Result |
|---|---|---|
| 1 | Status code is 401 | PASS |
| 2 | Response is JSON | PASS |
| 3 | Error message is returned | PASS |
| 4 | Protected data is not returned | PASS |

**Test Execution:** 4/4 assertions passed.

## Final Status
**PASS**

The API correctly rejects requests with an invalid API Key and prevents access to the requested resource.

## Notes
This test verifies API Key validation at the API gateway level. It does not test user-level authorization or Row Level Security (RLS) policies.

# TC32 – Anonymous vs Authenticated Access

**Test Case ID:** AUTHZ-002  
**Category:** Authorization & Data Isolation  
**Priority:** High  
**Type:** Positive / Negative Testing  
**Status:** PASS

## Objective
Verify that anonymous and authenticated users have different access permissions when requesting records from an RLS-protected Supabase table.

## Preconditions
- Supabase project is active.
- The `qa_rls_protected_test` table exists.
- Row Level Security (RLS) is enabled.
- Anonymous role has SELECT table privilege but no SELECT RLS policy.
- Authenticated role has SELECT table privilege and an applicable SELECT RLS policy.
- Three test records exist in the table.
- A valid access token is available in the Postman environment.

## Test Scenario 1 – Anonymous Access

**Method:** GET

**Endpoint:**
```http
{{baseUrl}}/rest/v1/qa_rls_protected_test?select=*
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |

**Authorization:** No Bearer token

### Expected Result
- HTTP status: `200 OK`
- Response is an empty JSON array.
- No protected records are returned.

### Actual Result
- HTTP status: `200 OK`
- Response: `[]`

### Assertions

| Assertion | Result |
|---|---|
| Status code is 200 | PASS |
| Response is an array | PASS |
| Anonymous user receives no records | PASS |

**Execution:** 3/3 PASSED

## Test Scenario 2 – Authenticated Access

**Method:** GET

**Endpoint:**
```http
{{baseUrl}}/rest/v1/qa_rls_protected_test?select=*
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |
| Authorization | `Bearer {{accessToken}}` |

### Expected Result
- HTTP status: `200 OK`
- Response is a JSON array.
- Authenticated user can access the test records.
- Each record contains the expected `id` field.

### Actual Result
- HTTP status: `200 OK`
- Three test records returned successfully.

### Assertions

| Assertion | Result |
|---|---|
| Status code is 200 | PASS |
| Response is an array | PASS |
| Authenticated user can access records | PASS |
| Records contain expected fields | PASS |

**Execution:** 4/4 PASSED

## Final Result

**PASS – 7/7 assertions passed.**

The API correctly distinguishes anonymous and authenticated access based on the configured Supabase RLS policies.

## Security Notes
This test verifies role-based access differences. The authenticated SELECT policy uses `USING (true)` on a dedicated test table, allowing all authenticated users to read its records.

This setup demonstrates authenticated access but does not verify record-level isolation between different users (BOLA/IDOR testing).

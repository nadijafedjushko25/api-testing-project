# TC40 – Full JSON Schema Validation

**Test Case ID:** TC40  
**Module:** API Contract & Schema Validation  
**Category:** API Schema Validation  
**Priority:** High  
**Test Type:** Positive Testing  
**Status:** PASS

## Objective
Verify that the API response conforms to the expected JSON Schema, including data structure, required fields, data types, UUID format, and absence of unexpected properties.

## Preconditions
- Supabase project is configured.
- RLS is enabled on the `qa_user_data_test` table.
- User 1 has a valid access token.
- User 1 has permission to read their own records.

## Request Details

**Method:** `GET`

**Endpoint:**
```http
{{baseUrl}}/rest/v1/qa_user_data_test?select=id,user_id,test_data
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |
| Authorization | `Bearer {{accessToken}}` |

## Test Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | Authenticate as User 1. | A valid access token is obtained. |
| 2 | Send a GET request to the `qa_user_data_test` endpoint. | API returns `200 OK`. |
| 3 | Validate the response Content-Type. | Response Content-Type is JSON. |
| 4 | Validate the response against the JSON Schema. | Response matches the defined schema. |
| 5 | Verify required fields and data types. | All required fields and data types are valid. |

## JSON Schema Requirements

| Field | Expected Type | Validation |
|---|---|---|
| id | Integer | Must be a number |
| user_id | String | Must match UUID format |
| test_data | String | Must be a string |

Additional validation rules:
- Response must be a JSON array.
- Every array item must be an object.
- All three fields are required.
- Additional properties are not allowed.

## Actual Result

**Status Code:** `200 OK`

**Response Body:**
```json
[
  {
    "id": 1,
    "user_id": "2d8f9dc6-68bc-4a18-8db1-9bead4420417",
    "test_data": "TC33 - User 1 private test record"
  }
]
```

## Postman Test Results

- Status code validation – PASS
- Response Content-Type validation – PASS
- Full JSON Schema validation – PASS

**Result: All Postman tests passed.**

## Conclusion

The API response successfully matched the defined JSON Schema.

All required fields were present, data types were correct, the `user_id` value followed the UUID format, and no unexpected properties were returned.

The API response contract was validated successfully.

## Tools Used
- Postman
- Supabase
- JSON Schema
- JavaScript (Postman Tests)
  
# TC41 – Validate API Response Headers

**Test Case ID:** TC41  
**Module:** API Contract & Schema Validation  
**Category:** HTTP Response Headers Validation  
**Priority:** Medium  
**Test Type:** Positive / Security Testing  
**Status:** PASS

## Objective

Verify that the Supabase REST API returns the expected HTTP response headers, including content type, schema profile, security headers, and content range information.

## Preconditions

- Supabase project is configured.
- RLS is enabled on the `qa_user_data_test` table.
- User 1 has a valid access token.
- User 1 has permission to read their own records.

## Request Details

**Method:** `GET`

**Endpoint:**
```http
{{baseUrl}}/rest/v1/qa_user_data_test?select=id,user_id,test_data
```

**Headers:**

| Key | Value |
|---|---|
| apikey | `{{apiKey}}` |
| Authorization | `Bearer {{accessToken}}` |

## Test Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | Send a GET request to the API endpoint. | API returns `200 OK`. |
| 2 | Check the Content-Type header. | Contains `application/json`. |
| 3 | Check the Content-Profile header. | Equals `public`. |
| 4 | Check X-Content-Type-Options. | Equals `nosniff`. |
| 5 | Check Strict-Transport-Security. | Contains the expected HSTS directives. |
| 6 | Check Content-Range. | Header is present in the response. |

## Expected Response Headers

| Header | Expected Value |
|---|---|
| Content-Type | `application/json; charset=utf-8` |
| Content-Profile | `public` |
| X-Content-Type-Options | `nosniff` |
| Strict-Transport-Security | Contains `max-age=31536000` and `includeSubDomains` |
| Content-Range | Present |

## Actual Result

**Status Code:** `200 OK`

The API returned all expected response headers.

| Header | Actual Value |
|---|---|
| Content-Type | `application/json; charset=utf-8` |
| Content-Profile | `public` |
| X-Content-Type-Options | `nosniff` |
| Strict-Transport-Security | `max-age=31536000; includeSubDomains; preload` |
| Content-Range | `0-0/*` |

## Postman Test Results

All 6 assertions passed:

- Status code validation – PASS
- Content-Type validation – PASS
- Content-Profile validation – PASS
- X-Content-Type-Options validation – PASS
- Strict-Transport-Security validation – PASS
- Content-Range validation – PASS

**Result: 6/6 assertions passed.**

## Conclusion

The Supabase REST API returned the expected HTTP response headers.

The response used JSON content, identified the `public` schema, included MIME type sniffing protection, and provided HTTP Strict Transport Security directives.

The API response header validation was completed successfully.

## Tools Used

- Postman
- Supabase REST API
- JavaScript (Postman Tests)

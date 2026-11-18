TC22 — Login with Valid Credentials

PASS
7/7 tests passed
POST

Test Case ID: AUTH-001

Priority: High

Module: Authentication

Objective:

Verify that a registered user with a confirmed email can successfully log in using valid credentials and receive valid access and refresh tokens.

Preconditions:
- User is registered in Supabase Authentication.
- User email is confirmed.
- Valid email and password are available.
- Postman Environment variables baseUrl, apiKey, loginEmail, and loginPassword are configured.
Request:
POST {{baseUrl}}/auth/v1/token?grant_type=password


Headers:
Key	Value
apikey	{{apiKey}}
Content-Type	application/json

Body:
{
  "email": "{{loginEmail}}",
  "password": "{{loginPassword}}"
}


Expected Result:
- Status code: 200 OK.
- Response contains a valid access_token.
- Response contains a valid refresh_token.
- token_type equals bearer.
- expires_in is a positive number.
- User email matches the login email.
- User information is returned successfully.
- Tokens are saved in Postman Environment variables.
Post-response Tests:
// Parse the JSON response
// Отримуємо JSON-відповідь сервера
let jsonData = pm.response.json();

// Verify successful authentication
// Перевіряємо успішний статус авторизації
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Verify that the response is valid JSON
// Перевіряємо формат відповіді
pm.test("Response is valid JSON", function () {
    pm.response.to.be.json;
});

// Verify that Access Token is present
// Перевіряємо наявність Access Token
pm.test("Access token is present", function () {
    pm.expect(jsonData.access_token).to.be.a("string").and.not.empty;
});

// Verify that Refresh Token is present
// Перевіряємо наявність Refresh Token
pm.test("Refresh token is present", function () {
    pm.expect(jsonData.refresh_token).to.be.a("string").and.not.empty;
});

// Verify the token type
// Перевіряємо, що тип токена — Bearer
pm.test("Token type is Bearer", function () {
    pm.expect(jsonData.token_type).to.eql("bearer");
});

// Verify that the returned email matches the login email
// Перевіряємо, що авторизувався саме потрібний користувач
pm.test("User email matches login email", function () {
    pm.expect(jsonData.user.email.toLowerCase())
        .to.eql(pm.environment.get("loginEmail").toLowerCase());
});

// Verify that token expiration is a positive number
// Перевіряємо коректність часу життя Access Token
pm.test("Access token expiration is valid", function () {
    pm.expect(jsonData.expires_in).to.be.a("number").and.above(0);
});

// Save tokens only after successful authentication
// Зберігаємо токени лише після успішної авторизації
if (pm.response.code === 200 &&
    jsonData.access_token &&
    jsonData.refresh_token) {

    pm.environment.set("accessToken", jsonData.access_token);
    pm.environment.set("refreshToken", jsonData.refresh_token);
}


Actual Result:

API returned 200 OK with valid authentication data, including Access Token, Refresh Token, token type, expiration information, and user details.

Test Execution Result: PASS — 7/7 assertions passed.

Notes: Email confirmation is required for the tested account. Tokens must not be included in public documentation or screenshots.

# TC23 — Login with Invalid Credentials

**Test Case ID:** AUTH-002  
**Priority:** High  
**Category:** Authentication  
**Status:** PASS

## Objective

Verify that the API rejects login attempts when a user provides invalid credentials and does not issue authentication tokens.

## Preconditions

- Supabase project is available.
- A registered user exists.
- Valid `loginEmail` is configured in Postman Environment.
- An intentionally incorrect `wrongPassword` is configured.
- Supabase API key is available.

## Request Details

**Method:** POST

**Endpoint:**
```text
{{baseUrl}}/auth/v1/token?grant_type=password
```

**Headers:**

| Key | Value |
|---|---|
| apikey | {{apiKey}} |
| Content-Type | application/json |

**Authorization:** No Auth

**Request Body:**
```json
{
  "email": "{{loginEmail}}",
  "password": "{{wrongPassword}}"
}
```

## Expected Result

- API returns HTTP 400 Bad Request.
- Error code is `invalid_credentials`.
- Error message is `Invalid login credentials`.
- No Access Token is returned.
- No Refresh Token is returned.
- Response is valid JSON.

## Post-response Tests

```javascript
// Parse the JSON response
// Отримуємо JSON-відповідь від API
let jsonData = pm.response.json();

// Verify that login was rejected
// Перевіряємо, що API відхилив авторизацію
pm.test("Status code is 400", function () {
    pm.response.to.have.status(400);
});

// Verify the error code
// Перевіряємо код помилки авторизації
pm.test("Error code is invalid_credentials", function () {
    pm.expect(jsonData.error_code).to.eql("invalid_credentials");
});

// Verify the error message
// Перевіряємо повідомлення про неправильні облікові дані
pm.test("Invalid credentials message is returned", function () {
    pm.expect(jsonData.msg).to.eql("Invalid login credentials");
});

// Verify that no access token is returned
// Перевіряємо, що Access Token не повертається
pm.test("Access token is not returned", function () {
    pm.expect(jsonData).to.not.have.property("access_token");
});

// Verify that no refresh token is returned
// Перевіряємо, що Refresh Token не повертається
pm.test("Refresh token is not returned", function () {
    pm.expect(jsonData).to.not.have.property("refresh_token");
});

// Verify that the response is JSON
// Перевіряємо, що відповідь має формат JSON
pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});
```

## Actual Result

**HTTP Status:** 400 Bad Request

**Response:**
```json
{
  "code": 400,
  "error_code": "invalid_credentials",
  "msg": "Invalid login credentials"
}
```

**Test Results:** 6/6 assertions passed.

**Final Status:** PASS

## Notes

The API correctly rejected the login attempt with invalid credentials. No authentication tokens were issued.

This test verifies negative authentication behavior and error response handling.

# TC24 — Get User with Valid Bearer Token

**Test Case ID:** AUTH-003  
**Priority:** High  
**Category:** Authentication  
**Status:** PASS

## Objective

Verify that an authenticated user can access the Supabase Auth endpoint using a valid Access Token and retrieve their own user information.

## Preconditions

- Supabase project is available.
- A registered and confirmed user exists.
- Valid login credentials are configured in Postman Environment.
- A successful login has been performed using TC22.
- A valid Access Token is stored in the Postman Environment as `accessToken`.
- Supabase API key is available.

## Request Details

**Method:** GET

**Endpoint:**
```text
{{baseUrl}}/auth/v1/user
```

**Headers:**

| Key | Value |
|---|---|
| apikey | {{apiKey}} |
| Authorization | Bearer {{accessToken}} |

**Authorization:** No Auth (Bearer Token is passed manually in Headers).

**Request Body:** None

## Expected Result

- API returns HTTP 200 OK.
- Response is valid JSON.
- User ID is present and not empty.
- Returned email matches the authenticated user's email.
- User role is `authenticated`.
- User is not anonymous (`is_anonymous` is `false`).

## Post-response Tests

```javascript
// Parse the JSON response
// Отримуємо JSON-відповідь від API
let jsonData = pm.response.json();

// Verify that the request was successful
// Перевіряємо, що запит виконано успішно
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Verify that the response is JSON
// Перевіряємо, що відповідь має формат JSON
pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

// Verify that the user ID is present
// Перевіряємо, що відповідь містить ID користувача
pm.test("User ID is present", function () {
    pm.expect(jsonData.id).to.be.a("string").and.not.empty;
});

// Verify that the email matches the logged-in user
// Перевіряємо, що email відповідає авторизованому користувачу
pm.test("User email matches login email", function () {
    pm.expect(jsonData.email).to.eql(
        pm.environment.get("loginEmail")
    );
});

// Verify that the user role is authenticated
// Перевіряємо, що роль користувача — authenticated
pm.test("User role is authenticated", function () {
    pm.expect(jsonData.role).to.eql("authenticated");
});

// Verify that the user is not anonymous
// Перевіряємо, що користувач не є анонімним
pm.test("User is not anonymous", function () {
    pm.expect(jsonData.is_anonymous).to.eql(false);
});
```

## Actual Result

**HTTP Status:** 200 OK

**Test Results:** 6/6 assertions passed.

**Final Status:** PASS

## Notes

The API successfully accepted the valid Bearer Access Token and returned the authenticated user's information.

This test verifies Bearer Token Authentication and confirms that the Access Token can be used to access a protected authentication endpoint.

TC22 verifies successful login and token generation, while TC24 verifies successful token usage.

# TC25 — Access Protected Resource Without Token

**Test Case ID:** AUTH-004  
**Priority:** High  
**Category:** Authentication / Security Testing  
**Status:** PASS

## Objective

Verify that the Supabase Auth API denies access to a protected endpoint when a request does not contain an Authorization Bearer Token.

## Preconditions

- Supabase project is available.
- Supabase API key is configured in Postman Environment.
- The endpoint `/auth/v1/user` is available.
- No Authorization header is included in the request.

## Request Details

**Method:** GET

**Endpoint:**
```text
{{baseUrl}}/auth/v1/user
```

**Headers:**

| Key | Value |
|---|---|
| apikey | {{apiKey}} |

**Authorization:** No Auth

**Request Body:** None

**Important:** The Authorization header must be completely omitted from the request.

## Expected Result

- API returns HTTP 401 Unauthorized.
- Response is valid JSON.
- Error code is `no_authorization`.
- Error message indicates that a valid Bearer Token is required.
- JSON response code is 401.

## Post-response Tests

```javascript
// Parse the JSON error response
// Отримуємо JSON-відповідь із помилкою
let jsonData = pm.response.json();

// Verify that access is denied
// Перевіряємо, що API заборонив доступ
pm.test("Status code is 401", function () {
    pm.response.to.have.status(401);
});

// Verify that the response is JSON
// Перевіряємо, що відповідь має формат JSON
pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

// Verify the authentication error code
// Перевіряємо код помилки автентифікації
pm.test("Error code is no_authorization", function () {
    pm.expect(jsonData.error_code).to.eql("no_authorization");
});

// Verify the error message
// Перевіряємо повідомлення про відсутність Bearer Token
pm.test("Missing Bearer Token message is returned", function () {
    pm.expect(jsonData.msg).to.eql(
        "This endpoint requires a valid Bearer token"
    );
});

// Verify the response code in the JSON body
// Перевіряємо код помилки безпосередньо в JSON Body
pm.test("Response body code is 401", function () {
    pm.expect(jsonData.code).to.eql(401);
});
```

## Actual Result

**HTTP Status:** 401 Unauthorized

**Response Body:**
```json
{
    "code": 401,
    "error_code": "no_authorization",
    "msg": "This endpoint requires a valid Bearer token"
}
```

**Test Results:** 5/5 assertions passed.

**Final Status:** PASS

## Notes

The API correctly rejected the request because no Authorization Bearer Token was provided.

This test verifies that the protected authentication endpoint does not allow unauthenticated access.

TC24 verifies access using a valid Bearer Token, while TC25 verifies that access is denied when the token is missing.

Together, these test cases cover both positive and negative Bearer Token authentication scenarios.

# TC26 – Access Resource with Invalid JWT

## Test Case ID
AUTH-005

## Title
Access Protected Resource with Invalid JWT

## Module
Authentication & Token Lifecycle

## Category
Authentication / Security Testing

## Priority
High

## Status
PASS

## Objective
Verify that the API rejects an invalid JWT and prevents unauthorized access to a protected resource.

## Preconditions
- A Supabase project is configured.
- A valid Supabase API key is available.
- The `/auth/v1/user` endpoint requires a valid Bearer Token.

## Request Details

**Method:** GET

**Endpoint:**
```text
{{baseUrl}}/auth/v1/user
```

**Authorization:** No Auth

### Headers

| Key | Value |
|---|---|
| apikey | {{apiKey}} |
| Authorization | Bearer invalid.jwt.token |

**Request Body:** None

## Test Steps
1. Open the Postman request for TC26.
2. Set the HTTP method to GET.
3. Enter the `/auth/v1/user` endpoint.
4. Add the `apikey` header with the Supabase API key.
5. Add the Authorization header with the invalid JWT: `Bearer invalid.jwt.token`.
6. Send the request.
7. Verify the response status and error details.
8. Execute the Post-response assertions.

## Expected Results
- API returns HTTP status code **401 Unauthorized**.
- Response is returned in JSON format.
- Authentication error code is present.
- User ID is not returned.
- An error message is returned.
- Protected user information is not accessible.

## Actual Results
- API rejected the invalid JWT.
- HTTP status code: **401 Unauthorized**.
- Response was returned in JSON format.
- Authentication error details were present.
- User ID was not returned.
- Access to the protected endpoint was denied.

## Post-response Tests
- Verify HTTP status code is 401.
- Verify response content type is JSON.
- Verify authentication error code is present.
- Verify user ID is not returned.
- Verify an error message is present.

## Test Result
**PASS** – The API correctly rejected the invalid JWT and prevented unauthorized access to the protected user endpoint.

## Security Notes
This test verifies that the API does not accept a malformed or invalid JWT as valid authentication credentials.

It complements:
- **TC24** – Get User with Valid Bearer Token
- **TC25** – Access Protected Resource Without Token

**Note:** This test checks an invalid JWT. An expired but otherwise valid JWT is covered separately in TC27.



TC30 — Logout and Session Revocation

PASS
204 + 403 verified
POST / GET

Test Case ID: AUTH-009

Priority: High

Module: Authentication / Session Management

Objective:

Verify that an authenticated user can successfully log out and that the previously issued Access Token can no longer access the authenticated user endpoint after session revocation.

Preconditions:
- User is successfully authenticated.
- Valid Access Token is available.
- Supabase API key is configured.
- Access Token is saved in Postman Environment as accessToken.
Part 1 — Logout Request

Request:
POST {{baseUrl}}/auth/v1/logout


Headers:
Key	Value
apikey	{{apiKey}}
Authorization	Bearer {{accessToken}}

Expected Result:
- Status code: 204 No Content.
- No response body is returned.
- Logout request is successfully processed.
Part 2 — Verify Session Revocation

Use the same previously issued Access Token. Do not perform another login between the two requests.

Request:
GET {{baseUrl}}/auth/v1/user


Headers:
Key	Value
apikey	{{apiKey}}
Authorization	Bearer {{accessToken}}

Expected Result:
- Status code: 403 Forbidden for the observed Supabase response.
- Response contains error_code: session_not_found.
- The old session is no longer available.
Post-response Tests — Logout Request:
// Verify that the logout request was successfully processed
// Перевіряємо успішну обробку запиту Logout
pm.test("Logout returns status code 204", function () {
    pm.response.to.have.status(204);
});

// Verify that the response has no body
// Перевіряємо, що відповідь 204 не містить Body
pm.test("Logout response body is empty", function () {
    pm.expect(pm.response.text()).to.eql("");
});


Post-response Tests — Old Token Verification:
// Parse the JSON error response
// Отримуємо JSON-відповідь із помилкою
let jsonData = pm.response.json();

// Verify that the old token is rejected
// Перевіряємо, що старий Access Token більше не приймається
pm.test("Status code is 403", function () {
    pm.response.to.have.status(403);
});

// Verify that the session no longer exists
// Перевіряємо, що сесію відкликано
pm.test("Session not found error is returned", function () {
    pm.expect(jsonData.error_code).to.eql("session_not_found");
});

// Verify that the error message refers to the missing session
// Перевіряємо повідомлення про відсутню сесію
pm.test("Session revocation message is returned", function () {
    pm.expect(jsonData.msg).to.include("Session");
});


Actual Result:
- Logout request returned 204 No Content.
- Subsequent GET /auth/v1/user request using the same old Access Token returned 403 Forbidden.
- Response contained error_code: session_not_found.
Test Execution Result: PASS — logout and subsequent session revocation behavior verified.

Notes: The exact observed error response is specific to the tested Supabase configuration. 
The test verifies that the old session is rejected, not that every API must return HTTP 403 after logout.

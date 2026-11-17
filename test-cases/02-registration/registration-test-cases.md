# User Registration – Test Cases

## TC01 – Successful Registration

**Test Case ID:** REG-001

**Title:** Successful registration with valid email and password

**Priority:** High

**Preconditions:**
- Supabase project is available.
- Email registration provider is enabled.
- A new email address is used for registration.

**Test Data:**
- Email: `api.testing.qa1311@yopmail.com`
- Password: Test password (not stored in this document)
- Name: `Anna`

**Steps:**
1. Open Postman.
2. Select the `Supabase – API Testing` environment.
3. Set the request method to `POST`.
4. Send a request to `{{baseUrl}}/auth/v1/signup`.
5. Provide a valid email and password in the JSON request body.
6. Check the API response.

**Expected Result:**
- API returns status code `200 OK`.
- Response contains a non-empty user ID.
- Response contains the correct email address.
- User role is `authenticated`.
- Email confirmation status is `false` because email verification is pending.

**Actual Result:**
- API returned `200 OK`.
- Response contained a valid user ID.
- Response email matched the test email.
- User role was `authenticated`.
- `user_metadata.email_verified` was `false`.
- User metadata contained the name `Anna`.

**Postman Test Results:** 5/5 Passed

**Status:** Passed

**Notes:** Supabase accepted the registration request and returned the created user data. Email confirmation was pending at the time of the test.

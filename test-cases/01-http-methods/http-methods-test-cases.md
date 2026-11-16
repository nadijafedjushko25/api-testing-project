# HTTP Methods – Test Cases

## HTTP-001 – Retrieve a Post Successfully

- **Priority:** High
- **Test Type:** Positive Testing
- **Method:** GET
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts/1`

### Preconditions
The API endpoint is available.

### Test Data
Post ID: `1`

### Steps
1. Open Postman.
2. Create a GET request.
3. Enter the endpoint URL.
4. Send the request.
5. Verify the HTTP status code and response body.

### Expected Result
- HTTP status code is `200 OK`.
- Response body is valid JSON.
- Response contains the `userId`, `id`, `title`, and `body` fields.
- The `id` value equals `1`.

### Actual Result

- HTTP status code: 200 OK.
- Response body is valid JSON.
- Response contains userId, id, title, and body fields.
- Post ID equals 1.
- All 4 Postman tests passed.

### Execution Status

Passed

## HTTP-002 – Retrieve a User Successfully

- **Priority:** High
- **Test Type:** Positive Testing
- **Method:** GET
- **Endpoint:** `https://jsonplaceholder.typicode.com/users/1`

### Preconditions

The API endpoint is available.

### Test Data

User ID: `1`

### Steps

1. Open Postman.
2. Create a GET request.
3. Enter the endpoint URL.
4. Send the request.
5. Verify the HTTP status code and response body.
6. Execute the Postman test scripts.

### Expected Result

- HTTP status code is `200 OK`.
- Response body is valid JSON.
- Response contains the `id`, `name`, `username`, and `email` fields.
- The `id` value equals `1`.

### Actual Result

- HTTP status code: `200 OK`.
- Response body: valid JSON.
- Required fields are present: `id`, `name`, `username`, and `email`.
- User ID equals `1`.
- All 4 Postman tests passed.

### Execution Status

Passed

## HTTP-003 – Create a Post Successfully

- **Priority:** High
- **Test Type:** Positive Testing
- **Method:** POST
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts`

### Preconditions

The API endpoint is available.

### Test Data

```json
{
  "title": "My first API test",
  "body": "Learning Postman",
  "userId": 1
}
```

### Steps

1. Open Postman.
2. Create a POST request.
3. Enter the endpoint URL.
4. Select Body → raw → JSON.
5. Enter the test data.
6. Send the request.
7. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `201 Created`.
- Response contains the correct `title`.
- Response contains the correct `userId`.

### Actual Result

- HTTP status code: `201 Created`.
- Title equals `My first API test`.
- User ID equals `1`.
- All 3 Postman tests passed.
- JSONPlaceholder returned a simulated created resource; persistent database storage was not verified.

### Execution Status

Passed

## HTTP-004 – Partially Update a Post Successfully

- **Priority:** High
- **Test Type:** Positive Testing
- **Method:** PATCH
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts/1`

### Preconditions

The API endpoint is available.

### Test Data

```json
{
  "title": "Updated title"
}
```

### Steps

1. Open Postman.
2. Create a PATCH request.
3. Enter the endpoint URL.
4. Select Body → raw → JSON.
5. Enter the test data.
6. Send the request.
7. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `200 OK`.
- Response body is valid JSON.
- The `title` field equals `Updated title`.
- The `id` value equals `1`.

### Actual Result

- HTTP status code: `200 OK`.
- Response body is valid JSON.
- Title equals `Updated title`.
- Post ID equals `1`.
- All 4 Postman tests passed.
- JSONPlaceholder simulated the update; persistent data storage was not verified.

### Execution Status

Passed

## HTTP-005 – Delete a Post Successfully

- **Priority:** High
- **Test Type:** Positive Testing
- **Method:** DELETE
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts/1`

### Preconditions

The API endpoint is available.

### Test Data

Post ID: `1`

### Steps

1. Open Postman.
2. Create a DELETE request.
3. Enter the endpoint URL.
4. Send the request.
5. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `200 OK`.
- Response body is a valid JSON object.
- Response body is empty (`{}`).

### Actual Result

- HTTP status code: `200 OK`.
- Response body: `{}`.
- All 3 Postman tests passed.
- JSONPlaceholder simulated the deletion; persistent data deletion was not independently verified.

### Execution Status

Passed

## HTTP-006 – Retrieve a Non-Existing Post

- **Priority:** Medium
- **Test Type:** Negative Testing
- **Method:** GET
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts/9999`

### Preconditions

The API endpoint is available.

### Test Data

Post ID: `9999` (non-existing post)

### Steps

1. Open Postman.
2. Create a GET request.
3. Enter the endpoint URL with a non-existing post ID.
4. Send the request.
5. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `404 Not Found`.
- Response body is a valid JSON object.
- Response body is empty (`{}`).

### Actual Result

- HTTP status code: `404 Not Found`.
- Response body: `{}`.
- All 3 Postman tests passed.

### Execution Status

Passed

## HTTP-007 – Create a Post with Missing Title

- **Priority:** High
- **Test Type:** Negative Testing
- **Method:** POST
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts`

### Preconditions

The API endpoint is available.

### Test Data

```json
{
  "body": "Testing a post without title",
  "userId": 1
}
```

### Steps

1. Open Postman.
2. Create a POST request.
3. Enter the endpoint URL.
4. Send the request without the required `title` field.
5. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `400 Bad Request`.
- Response contains a validation error indicating that the `title` field is required.
- The post is not created.

### Actual Result

- HTTP status code: `201 Created`.
- Response body contains `body`, `userId`, and generated `id`.
- Response does not contain `title`.
- Response is valid JSON.
- 2 Postman tests passed and 1 test failed.

### Execution Status

Failed

### Notes

JSONPlaceholder is a mock API and does not implement the required business validation for the `title` field. The actual result does not match the defined test requirement. This limitation does not confirm a defect in a real business application.and does not implement the required business validation. The result demonstrates a limitation of the test API and does not confirm a defect in a real business application.

## HTTP-008 – Retrieve a Post with Invalid ID

- **Priority:** Medium
- **Test Type:** Negative Testing
- **Method:** GET
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts/abc`

### Preconditions

The API endpoint is available.

### Test Data

Post ID: `abc` (invalid ID format)

### Steps

1. Open Postman.
2. Create a GET request.
3. Enter the endpoint URL with a non-numeric post ID.
4. Send the request.
5. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `404 Not Found`.
- Response body is a valid JSON object.
- Response body is empty (`{}`).

### Actual Result

- HTTP status code: `404 Not Found`.
- Response body: `{}`.
- All 3 Postman tests passed.

### Execution Status

Passed

## HTTP-009 – Create a Post with Empty Body

- **Priority:** High
- **Test Type:** Negative Testing
- **Method:** POST
- **Endpoint:** `https://jsonplaceholder.typicode.com/posts`

### Preconditions

The API endpoint is available.

### Test Data

```json
{}
```

### Steps

1. Open Postman.
2. Create a POST request.
3. Enter the endpoint URL.
4. Select Body → raw → JSON.
5. Send an empty JSON object.
6. Verify the HTTP status code and response body.

### Expected Result

- HTTP status code is `400 Bad Request`.
- Response contains a validation error.
- The post is not created.

### Actual Result

- HTTP status code: `201 Created`.
- Response body: `{"id": 101}`.
- Response is valid JSON.
- No validation error is returned.
- 1 Postman test passed and 2 tests failed.

### Execution Status

Failed

### Notes

JSONPlaceholder is a mock API and does not implement required business validation for an empty request body. The actual result does not match the defined test requirement. This limitation does not confirm a defect in a real business application.

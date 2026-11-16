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

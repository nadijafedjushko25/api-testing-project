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

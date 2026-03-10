# Test Case: TC_API_HTTP_Error_Codes

**Story ID:** 1309 **Feature:** API Error Handling **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that all API endpoints return the correct and appropriate HTTP status codes for various scenarios including successful operations, client errors, authentication failures, and server errors. Proper error codes are critical for API consumers to handle responses correctly.

## Preconditions:
1. Application API is accessible
2. Valid and invalid authentication tokens are available
3. Postman, curl, or REST client available
4. API endpoint documentation is available

## Test Steps:

### Scenario 1: 200 OK - Successful GET Request
1. Send a GET request to a valid endpoint with valid auth token:
   `GET /api/users/101`
2. Verify response status code is `200 OK`
3. Verify response body contains the expected resource data
4. Verify `Content-Type: application/json` is in response headers

### Scenario 2: 201 Created - Successful POST (Resource Creation)
1. Send a POST request with valid payload to a creation endpoint:
   `POST /api/users`
2. Verify response status code is `201 Created`
3. Verify response body contains the newly created resource with generated `id`
4. Verify `Location` header points to the new resource URL (e.g., `/api/users/102`)

### Scenario 3: 204 No Content - Successful DELETE
1. Send a DELETE request for an existing resource:
   `DELETE /api/users/101`
2. Verify response status code is `204 No Content` or `200 OK`
3. Verify the response body is empty (for 204)
4. Verify the resource no longer exists (GET returns 404)

### Scenario 4: 400 Bad Request - Invalid/Missing Parameters
1. Send a POST request with missing required fields or invalid data types:
   ```json
   { "email": "not-an-email", "age": "not-a-number" }
   ```
2. Verify response status code is `400 Bad Request`
3. Verify response body includes field-level validation error details
4. Verify error message clearly indicates which fields are invalid

### Scenario 5: 401 Unauthorized - Missing or Invalid Token
1. Send a GET request to a protected endpoint WITHOUT an Authorization header
2. Verify response status code is `401 Unauthorized`
3. Verify response body contains error message (e.g., `"error": "Authentication required"`)
4. Repeat with an invalid/expired token and verify same 401 response
5. Verify `WWW-Authenticate` header is present in the response

### Scenario 6: 403 Forbidden - Insufficient Permissions
1. Log in as a regular (non-admin) user
2. Attempt to access an admin-only endpoint:
   `GET /api/admin/users`
3. Verify response status code is `403 Forbidden`
4. Verify response body explains the user lacks permission
5. Verify the resource is NOT returned (no partial data leak)

### Scenario 7: 404 Not Found - Non-Existent Resource
1. Send a GET request for a resource that does not exist:
   `GET /api/users/99999`
2. Verify response status code is `404 Not Found`
3. Verify response body contains a descriptive error (e.g., `"error": "User not found"`)
4. Verify no stack trace or internal error details are exposed

### Scenario 8: 405 Method Not Allowed - Wrong HTTP Method
1. Send a DELETE request to a read-only endpoint:
   `DELETE /api/public/info`
2. Verify response status code is `405 Method Not Allowed`
3. Verify the `Allow` response header lists the permitted methods (e.g., `Allow: GET, HEAD`)

### Scenario 9: 409 Conflict - Duplicate Resource
1. Attempt to create a resource that already exists (e.g., duplicate email registration):
   `POST /api/users` with existing email
2. Verify response status code is `409 Conflict`
3. Verify response body explains the conflict (e.g., `"error": "Email already registered"`)

### Scenario 10: 422 Unprocessable Entity - Semantic Validation Error
1. Send a request with syntactically valid JSON but semantically invalid data:
   ```json
   { "start_date": "2025-12-31", "end_date": "2025-01-01" }
   ```
   (end_date is before start_date)
2. Verify response status code is `422 Unprocessable Entity`
3. Verify response body explains the semantic validation failure

### Scenario 11: 429 Too Many Requests - Rate Limiting
1. Send more than the allowed number of requests to an endpoint in a time window (e.g., 100 requests in 1 minute)
2. Verify response status code is `429 Too Many Requests` after the limit is exceeded
3. Verify `Retry-After` header is present indicating when to retry
4. Verify subsequent requests below the limit succeed with `200 OK`

### Scenario 12: 500 Internal Server Error - Server-Side Failure
1. Simulate a server-side failure (e.g., send a request that triggers a known bug in test environment, or database connection loss)
2. Verify response status code is `500 Internal Server Error`
3. Verify response body contains a generic error message (e.g., `"error": "Internal server error"`)
4. Verify NO stack traces, database queries, or internal details are exposed to the client
5. Verify the error is logged server-side for debugging

## Expected Results:

| HTTP Code | Scenario | Expected Behavior |
|---|---|---|
| 200 OK | Successful GET | Data returned with correct structure |
| 201 Created | Successful POST | Resource created; location header returned |
| 204 No Content | Successful DELETE | Empty body; resource removed |
| 400 Bad Request | Invalid/missing fields | Field-level validation errors returned |
| 401 Unauthorized | No/expired/invalid token | Authentication error; no data returned |
| 403 Forbidden | Insufficient permissions | Permission error; no data leaked |
| 404 Not Found | Non-existent resource | Not found error; no stack trace |
| 405 Method Not Allowed | Wrong HTTP method | Allow header lists valid methods |
| 409 Conflict | Duplicate resource | Conflict error; no duplicate created |
| 422 Unprocessable | Semantic validation fail | Descriptive semantic error returned |
| 429 Too Many Requests | Rate limit exceeded | Retry-After header; limit message |
| 500 Internal Server | Server-side failure | Generic error; no internals exposed |

## Test Data:
| Field | Value |
|---|---|
| Valid User ID | 101 |
| Non-existent ID | 99999 |
| Valid Token | `Bearer <valid_token>` |
| Invalid Token | `Bearer invalidtoken` |
| Admin Endpoint | /api/admin/users |
| Rate Limit Threshold | As per API config (e.g., 100 req/min) |

## Notes:
- HTTP status codes must follow RFC 7231 standards
- Never return `200 OK` with an error message in the body ("false positive 200")
- Error responses must follow a consistent JSON structure: `{ "error": "message", "code": "ERROR_CODE" }`
- All 4xx and 5xx errors should be logged in the application error log
- Do not expose internal server details (file paths, DB queries, stack traces) in any error response
- 500 errors should trigger an alert to the development/ops team

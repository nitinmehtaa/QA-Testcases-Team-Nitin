# Test Case: TC_API_CRUD_Operations

**Story ID:** 1308 **Feature:** API Functional Testing **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that all CRUD (Create, Read, Update, Delete) API operations work correctly with valid and invalid payloads, proper HTTP methods, and return expected responses and status codes.

## Preconditions:
1. Application API is accessible
2. Valid authentication token is available (refer to TC_API_Authentication_Token)
3. Postman, curl, or REST client available
4. API base URL and endpoint documentation is available
5. Test database is in a known clean state

## Test Steps:

### Scenario 1: CREATE - POST Request with Valid Payload
1. Send a POST request to the resource creation endpoint (e.g., `POST /api/users` or `POST /api/records`):
   ```json
   {
     "name": "Test User",
     "email": "newuser@example.com",
     "role": "viewer"
   }
   ```
2. Include valid Authorization header: `Bearer <token>`
3. Verify response status is `201 Created`
4. Verify response body contains the created resource with an `id` field
5. Verify the resource is persisted in the database

### Scenario 2: CREATE - POST Request with Missing Required Fields
1. Send a POST request with missing required fields:
   ```json
   { "name": "Test User" }
   ```
2. Verify response status is `400 Bad Request`
3. Verify response body contains field-level validation errors
4. Verify no partial record is created in the database

### Scenario 3: CREATE - POST Request with Duplicate/Conflicting Data
1. Send a POST request to create a resource that already exists (e.g., duplicate email)
2. Verify response status is `409 Conflict`
3. Verify response body contains a descriptive conflict error message

### Scenario 4: READ - GET Request for Existing Resource
1. Send a GET request to retrieve a specific resource (e.g., `GET /api/users/101`)
2. Include valid Authorization header
3. Verify response status is `200 OK`
4. Verify response body contains the correct resource data matching the ID

### Scenario 5: READ - GET Request for Non-Existent Resource
1. Send a GET request with a non-existent ID (e.g., `GET /api/users/99999`)
2. Verify response status is `404 Not Found`
3. Verify response body contains a descriptive not-found message

### Scenario 6: READ - GET List of Resources (Pagination)
1. Send a GET request to retrieve a list (e.g., `GET /api/users?page=1&limit=10`)
2. Verify response status is `200 OK`
3. Verify response contains an array of resources
4. Verify pagination metadata is returned (e.g., `total`, `page`, `limit`, `hasMore`)
5. Verify each item in the array contains expected fields

### Scenario 7: UPDATE - PUT Request with Valid Full Payload
1. Send a PUT request to update an existing resource (e.g., `PUT /api/users/101`):
   ```json
   {
     "name": "Updated Name",
     "email": "updated@example.com",
     "role": "editor"
   }
   ```
2. Verify response status is `200 OK`
3. Verify response body reflects the updated data
4. Verify changes are persisted in the database

### Scenario 8: UPDATE - PATCH Request with Partial Payload
1. Send a PATCH request with only the fields to update (e.g., `PATCH /api/users/101`):
   ```json
   { "name": "Partially Updated Name" }
   ```
2. Verify response status is `200 OK`
3. Verify only the specified field is updated; other fields remain unchanged

### Scenario 9: UPDATE - PUT/PATCH Request on Non-Existent Resource
1. Send a PUT/PATCH request to update a non-existent resource (e.g., `PUT /api/users/99999`)
2. Verify response status is `404 Not Found`
3. Verify no data is created or modified

### Scenario 10: DELETE - DELETE Request for Existing Resource
1. Send a DELETE request (e.g., `DELETE /api/users/101`)
2. Include valid Authorization header with appropriate permissions
3. Verify response status is `200 OK` or `204 No Content`
4. Verify the resource is removed from the database
5. Attempt to GET the deleted resource and verify `404 Not Found` is returned

### Scenario 11: DELETE - DELETE Request for Non-Existent Resource
1. Send a DELETE request for a non-existent resource (e.g., `DELETE /api/users/99999`)
2. Verify response status is `404 Not Found`
3. Verify no database error occurs

### Scenario 12: Wrong HTTP Method on Endpoint
1. Send a GET request to an endpoint that only accepts POST (e.g., `GET /api/users` when it should be POST)
2. Verify response status is `405 Method Not Allowed`
3. Verify the `Allow` header lists the permitted methods

## Expected Results:

| Scenario | Expected Status | Behavior |
|---|---|---|
| POST valid payload | 201 Created | Resource created; ID returned |
| POST missing fields | 400 Bad Request | Field validation errors returned |
| POST duplicate data | 409 Conflict | Conflict error; no duplicate created |
| GET existing resource | 200 OK | Correct resource data returned |
| GET non-existent | 404 Not Found | Not found error returned |
| GET list | 200 OK | Array with pagination metadata |
| PUT valid payload | 200 OK | Full update applied and persisted |
| PATCH partial payload | 200 OK | Only specified fields updated |
| PUT non-existent | 404 Not Found | No data created/modified |
| DELETE existing | 200/204 | Resource deleted; 404 on re-GET |
| DELETE non-existent | 404 Not Found | No error; clean response |
| Wrong HTTP Method | 405 Method Not Allowed | Allow header returned |

## Test Data:
| Field | Value |
|---|---|
| Base URL | https://api.example.com |
| Auth Token | `Bearer <valid_token>` |
| Valid User ID | 101 |
| Non-existent ID | 99999 |
| Duplicate Email | existing@example.com |

## Notes:
- All requests must include `Content-Type: application/json` header
- Verify response body structure matches the API contract/schema
- Test with authenticated AND unauthenticated requests to verify access control
- Verify that DELETE operations are soft-delete (flag) vs hard-delete based on requirements
- Use JSON Schema Validation to verify response payload structure is consistent

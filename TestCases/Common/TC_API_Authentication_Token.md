# Test Case: TC_API_Authentication_Token

**Story ID:** 1307 **Feature:** API Security - Authentication **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that the application's API endpoints correctly enforce token-based authentication. All protected endpoints must validate Bearer/JWT tokens and handle expired, invalid, missing, and malformed tokens appropriately.

## Preconditions:
1. Application API is accessible
2. Valid user credentials are available to generate a JWT/Bearer token
3. Postman, curl, or similar API testing tool is available
4. API base URL and endpoint documentation is available

## Test Steps:

### Scenario 1: Valid Token - Successful API Access
1. Send a POST request to `/api/auth/login` with valid credentials:
   ```json
   { "email": "testuser@example.com", "password": "Test@12345" }
   ```
2. Capture the returned Bearer/JWT token from the response
3. Send a GET request to a protected endpoint (e.g., `/api/user/profile`) with the token:
   ```
   Authorization: Bearer <valid_token>
   ```
4. Verify the response is `200 OK` with the expected data

### Scenario 2: Missing Token - Request Without Authorization Header
1. Send a GET request to a protected endpoint (e.g., `/api/user/profile`) WITHOUT the Authorization header
2. Verify the response is `401 Unauthorized`
3. Verify the response body contains an appropriate error message (e.g., `"error": "Authentication required"`)
4. Verify no user data is returned in the response

### Scenario 3: Invalid Token - Malformed/Random String
1. Send a GET request to a protected endpoint with a random invalid token:
   ```
   Authorization: Bearer invalidtoken12345
   ```
2. Verify the response is `401 Unauthorized`
3. Verify error message indicates invalid token (e.g., `"error": "Invalid token"`)
4. Verify no protected data is returned

### Scenario 4: Expired Token
1. Generate a valid JWT token
2. Wait for the token to expire (or manually set expiry to past date in a test environment)
3. Send a request to a protected endpoint using the expired token:
   ```
   Authorization: Bearer <expired_token>
   ```
4. Verify the response is `401 Unauthorized`
5. Verify the error message indicates token expiry (e.g., `"error": "Token expired"`)
6. Verify the client is prompted to re-authenticate or use refresh token

### Scenario 5: Tampered Token (Modified Payload)
1. Decode a valid JWT token (base64 decode the payload)
2. Modify the payload (e.g., change `"role": "user"` to `"role": "admin"` or change the user ID)
3. Re-encode and re-sign the token (without the secret key)
4. Send a request using the tampered token
5. Verify the response is `401 Unauthorized`
6. Verify the server detects signature mismatch and rejects the token

### Scenario 6: Token Used After Logout (Revocation)
1. Log in and obtain a valid JWT token
2. Log out via the UI or API (`POST /api/auth/logout`)
3. Attempt to use the old token to access a protected endpoint
4. Verify the response is `401 Unauthorized` (token revoked/blacklisted)
5. Verify no data is returned

### Scenario 7: Token Reuse Across Different Users
1. Log in as User A and obtain User A's JWT token
2. Log out as User A
3. Log in as User B
4. Attempt to access User A's resources using User A's token
5. Verify the token is only valid for User A's authorized resources
6. Verify User B cannot manipulate User A's data using User A's token

### Scenario 8: Refresh Token Flow
1. Log in and obtain both `access_token` and `refresh_token`
2. Wait for the access token to expire
3. Send a POST request to `/api/auth/refresh` with the refresh token
4. Verify a new valid access token is returned
5. Use the new access token to access a protected endpoint
6. Verify the new token works and returns `200 OK`
7. Attempt to use the old refresh token again after it's been used
8. Verify the used refresh token is invalidated (one-time use)

## Expected Results:

1. **Valid Token:** API returns `200 OK` with expected data
2. **Missing Token:** API returns `401 Unauthorized`; no data exposed
3. **Invalid Token:** API returns `401 Unauthorized` with clear error message
4. **Expired Token:** API returns `401 Unauthorized`; prompts re-auth or refresh
5. **Tampered Token:** API detects signature mismatch; returns `401 Unauthorized`
6. **Revoked Token:** Post-logout token returns `401 Unauthorized`
7. **Cross-User Token:** Token valid only for owner's resources; no cross-user data access
8. **Refresh Token:** New token issued; old refresh token invalidated after use

## Test Data:
| Field | Value |
|---|---|
| Valid Email | testuser@example.com |
| Valid Password | Test@12345 |
| Login Endpoint | POST /api/auth/login |
| Profile Endpoint | GET /api/user/profile |
| Refresh Endpoint | POST /api/auth/refresh |
| Logout Endpoint | POST /api/auth/logout |

## Notes:
- JWT tokens should use HS256 or RS256 algorithm minimum
- Token expiry (access): recommended 15-60 minutes
- Token expiry (refresh): recommended 7-30 days
- Store tokens in httpOnly cookies or memory; avoid localStorage for high-security apps
- Implement token blacklisting/revocation list for logout scenarios
- All token validation failures should be logged in security audit logs

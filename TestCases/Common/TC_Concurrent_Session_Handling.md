# Test Case: TC_Concurrent_Session_Handling

**Story ID:** 1302 **Feature:** Session Management **Priority:** High **Owner:** Nitin Mehta **State:** Manual **Sprint:** S-02 **Regression:** Yes

## Description
Verify that the application correctly handles simultaneous logins from multiple browsers, devices, or locations for the same user account — enforcing appropriate session policies.

## Preconditions:
1. Valid user account exists in the system
2. Application is accessible from at least two different browsers or devices
3. Session management policy is defined (e.g., single-session or multi-session allowed)

## Test Steps:

### Scenario 1: Concurrent Login - Same Browser, Different Tabs
1. Open Tab A in Chrome, log in with valid credentials
2. Open Tab B in Chrome (same session), navigate to the application
3. Verify both tabs show the authenticated state
4. Perform actions on Tab A (e.g., update profile)
5. Refresh Tab B and verify it reflects the updated state
6. Verify only one session token is active (not duplicated)

### Scenario 2: Concurrent Login - Two Different Browsers
1. Log in on Browser A (Chrome) with valid credentials
2. Log in on Browser B (Firefox) with the same credentials
3. Observe behavior:
   - If single-session policy: Browser A should be logged out with notification
   - If multi-session policy: Both sessions should remain active independently
4. Verify the system behavior matches the documented session policy

### Scenario 3: Concurrent Login - Two Different Devices (Desktop + Mobile)
1. Log in on a desktop browser with valid credentials
2. Log in on a mobile browser with the same credentials
3. Verify the session on each device works independently (if multi-session)
4. OR verify the older session is terminated (if single-session policy)
5. Verify the user receives a notification/email about new login from another device

### Scenario 4: Session Conflict Resolution
1. Log in on Device A
2. Log in on Device B with the same credentials
3. On Device A, perform a data update (e.g., change display name)
4. On Device B, attempt to save a conflicting update simultaneously
5. Verify the system handles the conflict gracefully (last-write-wins, conflict error, etc.)

### Scenario 5: Forced Logout via Admin
1. Log in as User A on two different browsers
2. As admin, force logout User A (invalidate all sessions)
3. Verify both active sessions are terminated immediately
4. Verify User A receives appropriate notification

### Scenario 6: Session Count Limit Enforcement
1. Log in with the same user on 5 different browsers/devices simultaneously
2. Verify that if the system has a session limit (e.g., max 3), the oldest session(s) are invalidated
3. Verify appropriate messaging is shown to the user on the invalidated session

## Expected Results:

1. **Same Browser Tabs:** Single session maintained; actions reflected across tabs
2. **Different Browsers:** Session policy enforced — either both active (multi) or only newest active (single)
3. **Different Devices:** Session isolation maintained; login notification sent to user
4. **Conflict Resolution:** No data corruption; graceful error handling or conflict resolution
5. **Admin Force Logout:** All sessions invalidated immediately across all devices
6. **Session Limit:** Oldest sessions dropped when limit exceeded; user notified

## Test Data:
| Field | Value |
|---|---|
| Valid Username | testuser@example.com |
| Valid Password | Test@12345 |
| Browser A | Chrome (Desktop) |
| Browser B | Firefox (Desktop) |
| Device | Mobile Browser (Safari/Chrome) |

## Notes:
- Check session management config (Redis/DB session store) to validate session count server-side
- Verify JWT refresh tokens are also invalidated correctly in single-session scenarios
- This TC is critical for high-security applications (banking, healthcare)
- Document expected behavior based on business requirements before execution

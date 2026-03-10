# TC_Role_Based_Access_Control

## Test Case ID
TC_RBAC_001

## Test Case Title
Role-Based Access Control Verification

## Description
This test case verifies that the system correctly implements role-based access control (RBAC) by ensuring users can only access features and data appropriate to their assigned roles. It validates permission enforcement, unauthorized access prevention, and proper role escalation/de-escalation mechanisms.

## Priority
High

## Severity
Critical

## Category
- Security Testing
- Access Control Testing
- Authorization Testing

## Preconditions
1. System is deployed and accessible
2. User authentication system is functional
3. Multiple user roles are configured in the system:
   - Admin (full system access)
   - Manager (department-level access)
   - User (basic access)
   - Guest (read-only access)
4. Test user accounts exist for each role:
   - admin@test.com (Admin role)
   - manager@test.com (Manager role)
   - user@test.com (User role)
   - guest@test.com (Guest role)
5. Test data and resources are available across different permission levels
6. Session management is configured
7. Audit logging is enabled

## Test Data
- **Admin User**: Username: admin@test.com, Password: Test@1234, Role: Admin
- **Manager User**: Username: manager@test.com, Password: Test@1234, Role: Manager
- **Standard User**: Username: user@test.com, Password: Test@1234, Role: User
- **Guest User**: Username: guest@test.com, Password: Test@1234, Role: Guest
- **Protected Resources**: 
  - Admin Dashboard (/admin/dashboard)
  - User Management (/admin/users)
  - Reports Section (/reports)
  - Settings (/settings)
  - Public Content (/public)

## Test Steps

### Phase 1: Admin Role Verification
1. Open the application login page
2. Enter admin@test.com credentials
3. Click "Login" button
4. Verify successful login and redirection to admin dashboard
5. Navigate to User Management section
6. Verify ability to:
   - View all user accounts
   - Create new user accounts
   - Edit existing user accounts
   - Delete user accounts
   - Assign/modify user roles
7. Navigate to System Settings
8. Verify ability to:
   - Modify system configurations
   - Access security settings
   - View audit logs
   - Manage backup and restore functions
9. Navigate to Reports section
10. Verify ability to:
    - Generate all types of reports
    - Export sensitive data
    - View financial reports
11. Logout from admin account

### Phase 2: Manager Role Verification
12. Login with manager@test.com credentials
13. Verify successful login and redirection to manager dashboard
14. Attempt to access Admin Dashboard
15. Verify "403 Forbidden" or "Access Denied" message is displayed
16. Navigate to Reports section
17. Verify ability to:
    - Generate department-specific reports
    - View team performance metrics
18. Verify inability to:
    - Access system-wide reports
    - Export sensitive financial data
19. Attempt to access User Management section
20. Verify limited access:
    - Can view users in their department only
    - Cannot create/delete users
    - Cannot modify user roles
21. Attempt to access System Settings
22. Verify "Access Denied" message
23. Logout from manager account

### Phase 3: Standard User Role Verification
24. Login with user@test.com credentials
25. Verify successful login and redirection to user dashboard
26. Verify access to:
    - Personal profile settings
    - Assigned tasks/projects
    - Public resources
27. Attempt to access Reports section
28. Verify limited access:
    - Can view personal reports only
    - Cannot generate department reports
29. Attempt to access User Management
30. Verify "Access Denied" message
31. Attempt to access Admin Dashboard
32. Verify "403 Forbidden" error
33. Attempt to modify another user's data
34. Verify operation is blocked with appropriate error
35. Logout from user account

### Phase 4: Guest Role Verification
36. Login with guest@test.com credentials
37. Verify successful login with read-only access
38. Verify ability to:
    - View public content only
    - Browse available resources
39. Verify inability to:
    - Create or modify any content
    - Access user profiles
    - View reports
    - Access any administrative functions
40. Attempt to navigate directly to /admin/users via URL
41. Verify automatic redirect to access denied page
42. Attempt to perform any write operations
43. Verify all write operations are blocked
44. Logout from guest account

### Phase 5: Role Escalation Testing
45. Login as user@test.com
46. Attempt to escalate privileges by:
    - Modifying role parameter in request headers
    - Tampering with session cookies
    - Using browser developer tools to modify role attributes
47. Verify that privilege escalation attempts are:
    - Detected and blocked
    - Logged in audit trail
    - Result in session termination
48. Attempt to access admin functions after manipulation
49. Verify access is still denied

### Phase 6: Role De-escalation Testing
50. Login as admin@test.com
51. Request system administrator to change role to "Manager"
52. Refresh the session or re-login
53. Verify:
    - Previous admin privileges are revoked
    - Only manager-level access is available
    - Previously accessible admin features are now restricted
54. Attempt to access User Management
55. Verify restricted access matching manager permissions

### Phase 7: Cross-Role Data Access
56. Login as manager@test.com
57. Note the user ID or session identifier
58. Attempt to access another manager's department data by:
    - Modifying URL parameters
    - Changing resource IDs in API calls
59. Verify:
    - Access is denied to other departments' data
    - Error message is displayed
    - Attempt is logged
60. Logout from manager account

## Expected Results
1. **Admin Role**: 
   - Full access to all system features and data
   - Can perform all CRUD operations
   - Can access all administrative functions
   
2. **Manager Role**:
   - Access to department-specific features
   - Cannot access admin-only functions
   - Receives "403 Forbidden" when attempting unauthorized access
   - Can view and manage only their department's data
   
3. **Standard User Role**:
   - Access to personal and assigned resources only
   - Cannot access administrative or management features
   - Cannot view or modify other users' data
   - Receives appropriate access denied messages
   
4. **Guest Role**:
   - Read-only access to public content
   - All write operations blocked
   - Cannot access any restricted areas
   - Direct URL navigation to restricted areas is blocked
   
5. **Security Measures**:
   - All privilege escalation attempts are blocked
   - Unauthorized access attempts are logged
   - Role changes take immediate effect
   - Cross-role data access is prevented
   - Session invalidation occurs on suspicious activity

## Acceptance Criteria
1. ✅ Users with Admin role can access all system features without restrictions
2. ✅ Users with Manager role can access only department-level features
3. ✅ Users with Standard User role can access only personal resources
4. ✅ Users with Guest role have read-only access to public content
5. ✅ All unauthorized access attempts return HTTP 403 or appropriate error
6. ✅ Error messages do not reveal system architecture or sensitive information
7. ✅ Privilege escalation attempts are detected and blocked
8. ✅ All access attempts (successful and failed) are logged in audit trail
9. ✅ Role changes are reflected immediately without requiring system restart
10. ✅ Direct URL manipulation does not bypass access controls
11. ✅ API endpoints enforce role-based permissions
12. ✅ Session tokens contain encrypted role information that cannot be tampered
13. ✅ Cross-role data access is prevented with proper validation
14. ✅ De-escalation of privileges works correctly and revokes previous access
15. ✅ System displays user-friendly error messages for access violations

## Test Environment
- **Application URL**: https://app.example.com
- **Browser**: Chrome (latest), Firefox (latest), Safari (latest)
- **Operating System**: Windows 10/11, macOS, Linux
- **Test Data Location**: /test-data/rbac/
- **Database**: Test database with role configuration

## Edge Cases and Boundary Scenarios

### Edge Case 1: Concurrent Role Modification
- **Scenario**: User's role is changed while they are actively logged in
- **Test**: 
  1. Login as user@test.com
  2. While logged in, admin changes role to Manager
  3. User attempts to access resources
- **Expected**: Session should refresh with new permissions on next action or force re-login

### Edge Case 2: Multiple Role Assignment
- **Scenario**: User is assigned multiple roles simultaneously
- **Test**:
  1. Assign both User and Manager roles to same account
  2. Login and verify access permissions
- **Expected**: System should either combine permissions or use highest privilege, based on design

### Edge Case 3: Orphaned Sessions
- **Scenario**: User account is disabled while session is active
- **Test**:
  1. Login as user@test.com
  2. Admin disables the account
  3. User attempts to perform actions
- **Expected**: Session should be invalidated immediately, forcing logout

### Edge Case 4: Role Inheritance
- **Scenario**: Testing hierarchical role permissions
- **Test**:
  1. Create custom role inheriting from Manager
  2. Verify inherited permissions work correctly
  3. Verify custom permissions are also enforced
- **Expected**: All permissions should be correctly inherited and enforced

### Edge Case 5: Time-Based Role Assignment
- **Scenario**: Temporary admin access expires
- **Test**:
  1. Grant temporary admin access to manager@test.com for 1 hour
  2. Verify admin access during valid period
  3. Wait for expiration
  4. Verify automatic reversion to original role
- **Expected**: Temporary permissions should expire and revert automatically

## Security Test Scenarios

### Security Test 1: Token Manipulation
- **Attack Vector**: Modifying JWT or session tokens to escalate privileges
- **Test**:
  1. Login as guest@test.com
  2. Capture authentication token
  3. Decode and modify role claims
  4. Resend requests with modified token
- **Expected**: Server rejects tampered tokens, logs security event

### Security Test 2: Insecure Direct Object Reference (IDOR)
- **Attack Vector**: Accessing other users' resources by changing IDs
- **Test**:
  1. Login as user@test.com (User ID: 123)
  2. Access profile: /api/users/123/profile
  3. Attempt to access: /api/users/456/profile
- **Expected**: 403 Forbidden, access denied to other user's data

### Security Test 3: Parameter Pollution
- **Attack Vector**: Sending multiple role parameters to confuse the system
- **Test**:
  1. Send request with: ?role=user&role=admin
  2. Verify which role is processed
- **Expected**: System uses first parameter or rejects request, no privilege escalation

### Security Test 4: Missing Authorization Headers
- **Attack Vector**: Accessing protected resources without authentication
- **Test**:
  1. Remove authentication headers
  2. Attempt to access /admin/dashboard
- **Expected**: 401 Unauthorized, redirect to login

### Security Test 5: SQL Injection in Role Queries
- **Attack Vector**: Injecting SQL to bypass role checks
- **Test**:
  1. Login with username: admin' OR '1'='1
  2. Attempt to access admin features
- **Expected**: Injection is blocked, query is parameterized, no unauthorized access

### Security Test 6: Session Fixation
- **Attack Vector**: Forcing user to use attacker-controlled session ID
- **Test**:
  1. Obtain session ID before login
  2. Login with fixed session ID
  3. Verify if session ID changes post-login
- **Expected**: New session ID is generated after successful authentication

### Security Test 7: Privilege Escalation via API
- **Attack Vector**: Using API endpoints to elevate privileges
- **Test**:
  1. Login as user@test.com
  2. Call API: PATCH /api/users/123 with {"role": "admin"}
  3. Attempt to modify own role
- **Expected**: API rejects role modification by non-admin users

## Post-Conditions
1. All test user accounts remain in system with original roles
2. No unauthorized changes to production data
3. Audit logs contain all test activities
4. Test sessions are properly terminated
5. Any temporary test data is cleaned up
6. Security alerts (if triggered) are documented and marked as test activities

## Dependencies
- User authentication system must be operational
- Database with role and permission tables must be accessible
- Audit logging system must be functional
- Session management service must be running

## Test Data Cleanup
After test execution:
1. Review and archive audit logs from test activities
2. Reset any modified user roles to original state
3. Remove any temporary test accounts created during testing
4. Clear test session data
5. Restore any modified system settings

## Notes
- This test should be performed on a staging environment before production validation
- Coordinate with security team before testing privilege escalation scenarios
- Document all access denied messages for consistency verification
- Verify that error messages do not expose sensitive system information
- Consider automated regression testing for critical RBAC scenarios
- Re-test after any changes to authentication or authorization mechanisms

## References
- OWASP Top 10: A01:2021 - Broken Access Control
- NIST SP 800-53: Access Control (AC) family
- CWE-285: Improper Authorization
- ISO 27001: A.9.2 User Access Management

## Author
QA Team - Nitin

## Version
1.0

## Last Updated
October 10, 2025

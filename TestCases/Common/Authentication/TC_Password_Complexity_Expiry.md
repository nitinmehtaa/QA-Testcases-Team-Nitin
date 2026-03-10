# TC_Password_Complexity_Expiry

## Overview
- Objective: Verify password complexity policy enforcement and password expiry workflow.
- Scope: New password creation, update, expiry notification, forced reset, grace period, and security constraints.
- Actors: End-user, System auth service, Email/SMS service.

## Preconditions
- User account exists and is active.
- Password policy configured: min length 12, at least 1 uppercase, 1 lowercase, 1 digit, 1 special, no 3 repeating chars, not matching last 5 passwords, no dictionary words, no username or email fragments.
- Password expiry set to 60 days with 7-day reminder window and 3-day grace period after expiry.
- Email/SMS notification channels are operational.

## Test Data
- Valid complex password examples: "S3cure!Passw0rd", "G00d#Password2025".
- Invalid samples: "password", "Password1", "abcdEFGH1234", "AAAbbb111!!!", "UserName123!".
- User: test.user+pwdpolicy@example.com.

## Assumptions
- Lockout threshold: 5 failed attempts (account locked 15 minutes).
- Session timeout: 15 minutes inactivity.

## References
- Security requirements doc SR-SEC-012, SR-SEC-045.
- OWASP ASVS 2.1.7, 2.1.10, 2.1.11.

## Test Steps and Expected Results

### TC1: Enforce minimum complexity on registration
1. Navigate to Sign Up and enter invalid password "password".
   - Expect: Inline error indicating complexity requirements; account not created.
2. Enter "Password1".
   - Expect: Error cites missing special character and length if <12.
3. Enter valid "S3cure!Passw0rd".
   - Expect: Registration succeeds; login possible.

### TC2: Enforce complexity on password change
1. Log in and open Change Password.
2. Enter current password and set new "AAAbbb111!!!" (3+ repeating char sequence).
   - Expect: Error: repeating character rule violated.
3. Enter "UserName123!" where username is part of password.
   - Expect: Error: contains profile attribute.
4. Enter "G00d#Password2025".
   - Expect: Password updated; receive confirmation notification.

### TC3: Disallow reuse of last N passwords
1. Attempt to set any of last 5 passwords.
   - Expect: Error: cannot reuse last 5 passwords.

### TC4: Dictionary word check
1. Try "Summer2025!" if "summer" in dictionary.
   - Expect: Error for dictionary word usage.

### TC5: Password expiry notification
1. For a password age of 53 days, trigger login.
   - Expect: Banner/email/SMS reminder with days remaining and change link.

### TC6: Forced reset at expiry
1. At day 60+, attempt login.
   - Expect: Redirect to forced change password screen; no access to app until changed.

### TC7: Grace period behavior
1. Within 3-day grace window post-expiry, login.
   - Expect: Access allowed but persistent reminder until changed.

### TC8: Session handling during forced reset
1. If session exists at expiry moment, attempt navigation.
   - Expect: Prompt to change password before continuing; non-destructive actions allowed until change screen.

### TC9: Lockout interaction
1. Repeatedly enter invalid new passwords 5 times during change.
   - Expect: Account lock for 15 minutes; clear message.

### TC10: Notifications and audit logs
1. On successful change, verify email/SMS content, timestamp, initiator IP, and device in audit logs.
   - Expect: All entries present; no sensitive password exposure.

## Acceptance Criteria
- All invalid passwords are rejected with specific actionable errors.
- Valid passwords are accepted and stored securely (non-reversible, salted, strong KDF).
- Expired passwords force change; reminders sent in configured windows.
- Reuse prevention and dictionary checks enforced.
- Full auditability and secure notifications.

## Traceability
- Story: ST-SEC-PASS-EXP-001
- Linked Bugs: TBD
- Owner: QA Team
- Priority: High
- State: Automated/Manual hybrid
- Sprint: S-02
- Regression: Yes

## Risks and Mitigations
- Risk: Overly strict rules increase support tickets.
  - Mitigation: Clear guidance, password hints, and passphrase alternative.
- Risk: Notification failures.
  - Mitigation: Retry with fallback channel; surface status to user.

## Automation Notes
- Tag: @auth @password @expiry @security
- Hooks to mock time travel for expiry scenarios; use seeded test user.
- Validate API responses and audit stream; sanitize logs in assertions.

# PM Buddy – Fix Login Issues

## Summary ✅
Fix login functionality so users authenticate correctly using O365 and EMP ID/password, and update the login UI to prevent unauthenticated access and improve clarity.

---

## Background 💡
Currently, the login function is not working correctly. Users are unable to log in using O365 or EMP ID and password. The system allows access without proper authentication, and the login page UI also requires updates to clearly indicate authentication state and errors.

---

## Goals 🎯
- Ensure secure, reliable authentication using O365 and EMP ID/password.
- Prevent access without valid authentication.
- Improve login page UI for clarity and error handling.
- Provide robust QA and monitoring to detect regressions.

---

## Scope (In / Out) 🔧
**In scope:**
- O365 (Azure AD / Microsoft Entra) SSO integration and flows.
- EMP ID + password authentication flow and backend validation.
- Login UI updates, error messages, accessibility checks, and left-side hero panel updates (copy, styling, and localization for English & Thai).
- Session creation, token handling, and logout flows.
- Automated and manual tests covering all login methods.

**Out of scope:**
- Full identity provider replacement or company-wide IAM changes beyond integration fixes.

---

## User Stories
- As a user, I want to sign in with my O365 account so I can access PM Buddy using corporate credentials.
- As an employee, I want to log in with my EMP ID and password so I can access the system when SSO is unavailable.
- As a user, I want to see clear error messages when login fails so I know how to resolve the issue.
- As a user, I want a welcoming, clear left-side hero panel with localized copy so the login page feels trustworthy and informative.
- As a security officer, I want to ensure the system blocks access when credentials are invalid.

---

## Acceptance Criteria ✅
1. Users can log in successfully using O365 account.
2. Users can log in using EMP ID and password.
3. System requires valid authentication before granting access (no unauthenticated access allowed).
4. QA verifies login works correctly for all login methods, including error conditions and session handling.
5. Left-side hero panel updated with approved copy and displays correctly in both English and Thai (no truncation or UI issues).

---

## Technical Requirements 🔍
- Implement or fix O365 SSO integration (OAuth 2.0 / OpenID Connect) with proper token validation and certificate handling.
- Ensure EMP ID/password flow authenticates against an authoritative identity service with hashed password verification and rate limiting.
- Enforce server-side session validation for every protected endpoint (no client-side-only checks).
- Update login UI: clear fields, informative error messages, loading states, and accessibility (keyboard and screen reader friendly).
- Update and localize the left-side hero panel (hero banner) copy and styles. Provide proposed copy options (e.g., "Welcome to PM Buddy", "Smart project management, now in English & Thai", "Manage projects with clarity and speed"); ensure all copy is translatable, supports Thai characters (UTF-8 and appropriate fonts), and displays correctly without layout breakage.
- Add logging and telemetry for authentication events (success, failure, suspicious activity) with alerting for high failure rates.
- Ensure secure cookie/session flags (HttpOnly, Secure, SameSite as appropriate) and CSRF protections.
- Validate input and sanitize to mitigate injection attacks; enforce strong password policy for EMP ID accounts.

---

## QA & Testing ✅
- Manual test checklist: O365 success/failure flows, EMP ID flows, session expiry and logout, invalid credentials, locked accounts, UI message checks in both languages (if applicable), and left-side hero panel copy/layout verification in both English and Thai across viewport sizes.
- Automated tests: integration tests for authentication endpoints, E2E tests for login flows, and security tests (rate limit and brute-force protection simulation).
- Regression tests run in CI to ensure no unauthenticated access is introduced.
- Verify monitoring/alerts trigger for anomalies in authentication failures.

---

## Rollout & Metrics 📊
- Roll out fixes behind a feature flag or phased deployment.
- Metrics: login success rate per method, authentication error rate, number of unauthenticated access incidents, and mean time to detect/fix authentication issues.

---

## Risks & Mitigations ⚠️
- Risk: SSO misconfiguration can block access. Mitigation: staged rollout, fallback EMP login, and runbook for immediate rollback.
- Risk: Unhandled token/session issues causing security gaps. Mitigation: add server-side checks, token expiry handling, and automated tests.

---

## Stakeholders
- Product: PM
- Engineering: Backend & Frontend leads
- Security: Security/Infra team
- QA: Test plan owner

---

> **Next steps:** Implement SSO fixes and EMP login validation, update UI, add tests, and run QA passes before phased rollout.

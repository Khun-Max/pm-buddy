# PM Buddy – Project Dashboard Data Display and Email Share Function Fix

## Summary ✅
Fix project data display and save issues on the Project Dashboard so saved data is complete and visible, ensure view/edit behavior matches the main dashboard, and repair the email share function (including adding a close icon and verifying successful sends).

---

## Background 💡
Currently, project data is not displayed correctly on the Project Dashboard, and some information is missing after saving. The email share function requires improvements — the popup lacks a close icon and sharing may not send the complete or correct data. View and edit functionality should behave consistently with the main dashboard so users can access and manage project data properly.

---

## Goals 🎯
- Ensure complete and accurate project data displays on the Project Dashboard after save.
- Make view and edit behavior consistent with the main dashboard (no missing data after edit/save).
- Repair and validate the email share workflow so it sends correct project data and includes a functioning close icon.
- Improve UX of share popup with clear success/failure feedback and accessibility.

---

## Scope (In / Out) 🔧
**In scope:**
- Fix data persistence/display bugs on Project Dashboard (list and detail views).
- Align view and edit flows with main dashboard behaviors (including API usage and client-side rendering).
- Email share popup: add close icon, validate payload, add success/failure state, and accessibility improvements.
- Tests for UI, API, and email sending (unit, integration, E2E).

**Out of scope:**
- Full redesign of dashboard pages outside display/edit parity fixes.
- Replacing the email provider unless necessary for functional reasons.

---

## User Stories
- As a user, I want project data to display correctly on the Project Dashboard after saving so I can review work reliably.
- As a user, I want to view and edit project data on the Project Dashboard with the same behavior as the main dashboard so edits persist and nothing is missing.
- As a user, I want to share project data by email and see a close icon on the share popup so I can exit easily if needed.
- As a user, I want to receive confirmation that the email was sent successfully (or an error explaining what went wrong).

---

## Acceptance Criteria ✅
1. Project data displays correctly on the Project Dashboard after saving (no missing fields).
2. Users can view and edit project data on Project Dashboard; edits persist and reflect immediately and consistently with the main dashboard.
3. Email share function sends complete and correct project data to recipients.
4. Share-email popup contains a visible, accessible close icon that dismisses the popup.
5. Share flow shows clear success and error states; errors include actionable messages and retry options.
6. QA verifies data display, view/edit parity, and email sharing across browsers and devices.

---

## Technical Requirements 🔍
- Frontend
  - Verify data-binding and rendering for Project Dashboard list and detail views; fix issues causing missing fields (ensure consistent field mapping and formatters used by main dashboard).
  - Reuse or align with main dashboard view/edit components to ensure parity (avoid divergent clients using different data sources or transformations).
  - Implement share popup improvements: add a close button (visible, keyboard-focusable), ensure Escape key and click-away dismiss work, add explicit Cancel action.
  - Include success and error UI states with clear copy, retry, and logging of error codes for diagnostics.
  - Ensure accessibility: aria labels for the close icon, proper focus management when popup opens/closes, and screen reader-friendly messages.

- Backend / API
  - Confirm API responses include all required fields for dashboard display; add or fix fields if missing (e.g., createdBy, updatedAt, key metadata).
  - Ensure save/edit endpoints return canonical, complete resource representations after save to allow immediate client re-render without additional fetches.
  - Implement or validate email share API endpoint that accepts project ID and recipient details and returns send status and message ID.
  - Add server-side validation of email payload and sanitize user-provided content.

- Email Integration
  - Verify email payload includes the full, canonical project representation (fields as displayed in the dashboard); add serialization logic if necessary.
  - Handle email provider responses and propagate success/failure back to client with meaningful messages.
  - Log email sends (who triggered, recipient list, project snapshot), store message ID for tracing, and provide retry mechanisms for transient failures.

- Data Integrity & Concurrency
  - Ensure save/edit operations are atomic and return up-to-date resource representations.
  - Add or respect versioning/ETags where applicable to avoid overwrites.

- Security & Validation
  - Enforce permission checks for view/edit and email-share operations.
  - Rate-limit email-share endpoint to prevent abuse.
  - Protect against injection via email content and sanitize HTML/markdown before sending.

---

## UI / Design Notes 🎨
- Share popup layout: title, recipient input (email or list), message preview containing serialized project data, Send and Cancel buttons, and a close icon in the top-right.
- Close icon should be visually distinct, accessible (aria-label="Close share dialog"), and keyboard-focusable; on close, focus returns to the invoking control.
- Show inline validation for invalid email addresses; disable Send until recipients and required fields are valid.
- Provide a small non-modal toast confirming send success or an inline error with a Retry button if failure.
- Keep copy concise and consistent with existing product tone.

---

## QA & Testing ✅
- Manual test checklist:
  - Save a project and verify Project Dashboard list and detail show all fields.
  - Edit a project from Project Dashboard and confirm edits persist and match main dashboard.
  - Open share popup, verify close icon works, Escape and click-away dismiss behavior, keyboard navigation, and screen reader announcements.
  - Send share email to test addresses; verify recipients receive expected content and that logs/store show send success.
  - Test invalid inputs (invalid email, missing recipients) show correct errors and prevent send.
  - Cross-browser and mobile checks for popup behavior and data display.

- Automated tests:
  - Unit tests for serialization and share payload formation.
  - Integration tests for email-send API endpoint with a test/mock provider.
  - E2E tests: from dashboard -> open share -> send -> verify UI success and backend logs (or email sink) contain the payload.
  - Visual regression tests for project detail rendering and share popup layout.

---

## Rollout & Metrics 📊
- Roll out behind a feature flag and monitor for a small cohort before full release.
- Metrics to track: % projects showing complete data, edit success rate, email share send success rate, number of share popup dismissals (close icon usage), and user-reported incidents about missing data.

---

## Risks & Mitigations ⚠️
- Risk: Fixes expose missing data in older records. Mitigation: add migration or fallback mapping for legacy records; provide a script to backfill critical fields.
- Risk: Spam or abuse of email share. Mitigation: rate-limiting, recipient validation, and monitoring.
- Risk: UI regressions in other dashboards. Mitigation: reuse components from main dashboard and add visual regression tests.

---

## Stakeholders
- Product: PM
- Engineering: Frontend & Backend leads
- Design: UX/UI owner
- QA: Test lead
- Security: AppSec reviewer

---

> **Next steps:** Agree on canonical project schema for dashboard display, implement API fixes to return complete resources after save, update frontend to reuse main dashboard view/edit components, add share popup close icon and send flow handling, and add tests and monitoring before staged rollout.

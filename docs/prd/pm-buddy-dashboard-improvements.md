# PM Buddy – Dashboard Data View, Edit, and Project Status Management Improvement

## Summary ✅
Improve the dashboard so users can reliably view and edit saved project data, manage project status (Draft / Pending / Approved) directly from the dashboard, and display date/time in a user-friendly format for better readability and usability.

---

## Background 💡
Currently, the dashboard does not allow users to properly view or edit saved data, which makes it difficult to review project information after saving. Project status management is also not available from the dashboard, and users cannot change status such as Draft, Pending, or Approved. In addition, date and time are displayed in system format (e.g., 2026-02-05T02:17:14.753661), which is not user-friendly. Currently, users can open the Project Dashboard by clicking the View button only, but clicking the project name should also navigate to the same Project Dashboard. The dashboard needs to be improved to support data visibility, edit functionality, project status updates, navigation improvement, and readable data formats.

---

## Goals 🎯
- Provide reliable, discoverable access to saved project data on the dashboard.
- Enable users to edit existing data after saving with proper validation and conflict handling.
- Allow direct management of project status (Draft, Pending, Approved) from the dashboard.
- Display date/time in user-friendly, localized formats (e.g., DD/MM/YYYY or localized format per user preference).
- Improve layout and data presentation for faster review and action.

---

## Scope (In / Out) 🔧
**In scope:**
- Dashboard list and detailed data view of saved projects.
- Edit flows from the dashboard (inline, modal, or secondary page) with validation and save.
- Status change controls (Draft / Pending / Approved) with permission checks and audit trail.
- User-friendly date/time formatting and timezone handling for display.
- UI/UX improvements for layout, truncation/expansion, and responsive viewports.
- Backend API endpoints and data model updates to support viewing/editing and status transitions.
- QA test coverage (unit, integration, E2E) for view/edit/status workflows.

**Out of scope:**
- Re-design of other application screens beyond dashboard (unless required for consistency).
- Full workflow approval engine (only simple Draft/Pending/Approved transitions and validations).

---

## User Stories
- As a user, I can view saved project data from the dashboard so I can review details after saving.
- As a user, I can edit existing project data from the dashboard so I can correct or update information quickly.
- As a user, I can change project status (Draft, Pending, Approved) from the dashboard to reflect current state.
- As a user, I see dates and times in a readable, localized format according to my preferences.
- As a PM, I can quickly identify projects needing action due to status or recent updates.

---

## Acceptance Criteria ✅
1. Users can view saved project data from the dashboard.
2. Users can edit existing project data after saving.
3. Project status can be changed from the dashboard (Draft, Pending, Approved).
4. Clicking the project name navigates to the Project Dashboard (same as View action).
5. Date and time are displayed in user-friendly format.
6. Dashboard layout and data display are improved for better usability.
7. QA verifies data viewing, editing, navigation, and status change work correctly.

---

## Technical Requirements 🔍
- Frontend
  - Add a dashboard detail view (expandable row, side panel, or modal) to display full project data.
  - Provide edit affordances: inline edit for simple fields, modal or edit page for complex edits.
  - Implement optimistic UI updates with server confirmation and error rollback on failure.
  - Add status control (select/toggle) with confirmation dialog for status changes and visible badges for current status.
  - Format date/time using a localized formatter (Intl.DateTimeFormat or equivalent), respect user locale/timezone preferences, and provide consistent short/long formats.
  - Handle long strings and Thai/English localized text without truncation or layout breakage; ensure accessibility and keyboard navigation.

- Backend / API
  - Expose endpoints: GET /projects (list), GET /projects/:id (details), PATCH /projects/:id (partial update), POST /projects/:id/status (status change) or PATCH with status in body.
  - Enforce permission checks on status changes and edits (roles: editor, approver, viewer).
  - Implement optimistic lock/versioning or ETag handling to detect concurrent edits and prevent silent overwrites.
  - Persist audit trail for edits and status changes (who, when, what changed).
  - Ensure date/time storage uses UTC and convert to user's timezone at display time.

- Data Model
  - Add or confirm fields: status (enum: Draft, Pending, Approved), updatedAt (timestamp), updatedBy (user id), version or etag for concurrency control.

- Security & Validation
  - Validate all input server-side and sanitize fields to prevent injection.
  - Rate-limit status-change endpoints to avoid mass status tampering.
  - Provide CSRF protection and require authenticated sessions for state-changing operations.

---

## UI / Design Notes 🎨
- Dashboard row should show key fields, status badge, last modified time (friendly format), and quick actions (view / edit / change status).
- Detail panel should include full fields with clear edit buttons and Save / Cancel actions.
- Use clear status colors (e.g., Draft = gray, Pending = amber, Approved = green) and tooltips explaining rules.
- For date/time, show localized short format in lists and full format in details (e.g., "5 Feb 2026, 10:17 GMT+7").
- Ensure responsive layout and test Thai text rendering for all components.

---

## QA & Testing ✅
- Unit tests for formatters, status logic, and API request transformations.
- Integration tests for API endpoints (view, edit, status change, concurrency checks).
- E2E tests for user flows: view -> edit -> save, and status change with permission variations.
- Visual regression tests for dashboard layout (snapshot tests or Percy/Applitools) including Thai content checks.
- Manual test cases: concurrent edit conflict, validation errors, unauthorized status change, date/time displays across timezones, responsive and accessibility checks.

---

## Rollout & Metrics 📊
- Roll out behind a feature flag with staged release (small % -> 100%).
- Metrics: number of edits performed, status change frequency, edit success rate, concurrency conflict rate, average time-to-complete status change, and user satisfaction feedback.

---

## Risks & Mitigations ⚠️
- Risk: Concurrent edits cause data loss. Mitigation: implement optimistic locking and user-visible conflict resolution.
- Risk: Status changes by unauthorized users. Mitigation: strict permission checks and logging.
- Risk: UI layout breaks with localized text. Mitigation: responsive design and visual QA with Thai and English content.

---

## Stakeholders
- Product: PM
- Engineering: Frontend & Backend leads
- Design: UX owner
- QA: Test lead
- Security: AppSec reviewer

---

> **Next steps:** Align on UX patterns for view vs. edit (inline vs modal), add API contract, implement backend versioning, and create QA test matrix for manual and automated tests.

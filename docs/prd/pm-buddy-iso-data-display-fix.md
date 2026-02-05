# PM Buddy – ISO Project Data Not Displayed and Dashboard Data Format Issue

## Summary ✅
Fix issues where ISO-format project data is not displayed after save, ensure the dashboard shows project code (not internal project ID), and correct incorrect project start dates so displayed values match user input or creation date.

---

## Background 💡
A new ISO format project was created and all required data was filled and saved successfully. However, when viewing or editing the project, the saved values are not displayed. This makes it difficult for users to review or update project information. On the dashboard, the project bar currently shows the project ID instead of the project code, which is not user-friendly. In addition, the project starting date is displayed incorrectly (showing a previous year such as 2025 instead of the actual created or selected date). The system needs to ensure correct data display, proper project identification, and accurate date handling across project pages and dashboard exports.

---

## Goals 🎯
- Ensure saved ISO project data is reliably persisted and displayed in view and edit pages.
- Display project code (human-friendly identifier) on the dashboard instead of internal project ID.
- Ensure project start dates (and other date fields) render correctly based on user input or creation date.
- Provide robust QA and monitoring to catch similar issues early.

---

## Scope (In / Out) 🔧
**In scope:**
- Fix view/edit pages for ISO-format projects so saved fields are shown correctly.
- Dashboard display changes to surface `projectCode` instead of `projectId` in project bars and lists.
- Correct date parsing/formatting for project start date and other date fields (storage, retrieval, and display).
- Add migration/backfill for records with incorrect/missing fields, if required.
- API, backend, and frontend fixes; automated and manual tests.

**Out of scope:**
- Reformatting of unrelated dashboards or unrelated export formats unless directly affected.

---

## User Stories
- As a user, I want previously saved ISO project values to be visible when I open a project so I can review and edit them.
- As a user, I want to see the project code on the dashboard so I can quickly identify projects.
- As a user, I want the project start date to display the correct date I entered or that was created so timeline data is trustworthy.

---

## Acceptance Criteria ✅
1. Saved ISO project data is correctly displayed in view and edit pages.
2. Users can view and edit previously saved values without missing data.
3. Dashboard displays project code instead of project ID.
4. Project starting date displays correctly based on user input or creation date.
5. Data displayed on dashboard and project pages is accurate and consistent.
6. QA verifies data saving, viewing, editing, and dashboard display work correctly.

---

## Technical Requirements 🔍
- Backend / API
  - Ensure save endpoints (POST/PATCH) persist the full, canonical ISO project representation and return the up-to-date resource in the response body.
  - Confirm API responses include `projectCode`, `startDate`, and all ISO-specific fields required by the UI.
  - Standardize date storage in UTC with explicit date-only or datetime semantics; include createdAt/updatedAt timestamps.
  - Add or correct serialization/deserialization logic to avoid off-by-one-year or timezone parsing errors (use strict ISO 8601 handling).
  - Add migration/backfill scripts to fix existing records with missing or incorrect `projectCode` or `startDate` values.

- Frontend
  - Map UI fields to the canonical response fields; ensure view and edit forms populate from the response rather than stale local state.
  - Replace dashboard label for projects to use `projectCode` with fallback to `displayName` then `projectId` if missing.
  - Use robust date parsing and display logic (e.g., Date.parse or Luxon/Day.js/Intl aware formatting); display localized, user-friendly date (e.g., "5 Feb 2026").
  - Add defensive UI to surface missing fields with a clear message (e.g., "Data missing — please reopen and save") and a link to report an issue.

- Data & Backfill
  - Create a data migration script that identifies records missing `projectCode` or with invalid `startDate`, attempts safe backfill from createdAt or other metadata, and logs changes for review.
  - Run migration in a staging environment and validate before production rollout.

- Monitoring & Logging
  - Add server-side logs and alerts for save operations returning incomplete resources or for parsing errors during reads.
  - Track metrics: % of projects with missing fields, frequency of startDate corrections, and number of edit/view errors reported.

---

## QA & Testing ✅
- Unit tests: serializer/deserializer, date parsing edge cases, and fallback logic for dashboard labels.
- Integration tests: save -> read roundtrip for ISO projects; ensure response contains full resource and UI displays it.
- E2E tests: create ISO project -> save -> view -> edit -> verify fields persist and display correctly; check dashboard shows `projectCode` and correct `startDate`.
- Regression tests: verify other project types are unaffected and backward compatibility is preserved.
- Manual tests: sample records with Thai and English content, old records with missing fields (after migration), and cross-timezone date verification.

---

## Rollout & Metrics 📊
- Deploy fixes behind a feature flag or perform phased rollout (canary users → wider audience).
- Metrics to monitor: percentage of ISO projects displaying all required fields, number of user reports about missing data, and rate of startDate mismatch incidents.

---

## Risks & Mitigations ⚠️
- Risk: Legacy records may lack canonical fields. Mitigation: run backfill migration and provide a manual review process for ambiguous records.
- Risk: Date parsing differences across browsers/timezones. Mitigation: use server-normalized UTC storage and client-side localized formatting libraries; include tests for timezone edge cases.
- Risk: UI shows stale local state. Mitigation: ensure save responses return the canonical resource and client re-renders from server data.

---

## Stakeholders
- Product: PM
- Engineering: Backend & Frontend leads
- Data: Migration owner
- QA: Test lead

---

> **Next steps:** Agree on canonical ISO project schema, implement API and UI fixes, create and validate migration on staging, add tests and monitoring, and schedule a staged rollout.

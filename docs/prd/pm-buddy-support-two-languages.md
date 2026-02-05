# PM Buddy – Support Two Languages (English & Thai)

## Summary ✅
Support full bilingual (English & Thai) user interface and content across PM Buddy to ensure consistent usability and accessibility for all users.

---

## Background 💡
Currently, PM Buddy does not fully support two languages across the system. Some parts of the application are not displayed correctly in Thai language. The system should support both English and Thai languages consistently across all modules, pages, functions, and generated ISO documents to ensure usability and a consistent user experience for all users. If users input data in Thai, the generated ISO format should display Thai content correctly.

---

## Goals 🎯
- Deliver consistent UI and content in both **English** and **Thai**.
- Allow users to switch languages easily with immediate, correct updates to content and UI.
- Remove missing or incorrect translations and eliminate UI display issues caused by localization.
- Ensure generated ISO documents correctly display content in the language entered by users (Thai or English).

---

## Scope (In / Out) 🔧
**In scope:**
- All user-facing pages, modules, and components (labels, buttons, error messages, notifications, help text).
- Language switcher control and persistence (user preference and session fallback).
- Translation management workflow for English ↔ Thai.
- QA and verification for bilingual support.
- Generated ISO documents and export templates to support localization and Thai character rendering.

**Out of scope:**
- Content translation for non-product-managed user-generated content unless explicitly requested.

---

## User Stories
- As a user, I want to change the UI language to Thai so I can use PM Buddy in my preferred language.
- As a user, I want content to display correctly in Thai (no truncation or layout breakage) so tasks & data remain usable.
- As a product manager, I want a clear process for adding and reviewing translations so future changes remain consistent.

---

## Acceptance Criteria ✅
1. System supports both English and Thai languages across all pages and modules.
2. Users can switch language and content updates correctly (immediate effect and persisted preference).
3. No missing or incorrect translations in Thai or English (strings reviewed and approved).
4. Language display works correctly without UI issues (no overlap, truncation, or layout breaks).
5. Generated ISO documents display content based on user input language (Thai or English), preserving characters and layout.

---

## Technical Requirements 🔍
- Use i18n library/framework compatible with the tech stack (e.g., react-i18next or equivalent).
- Store localized strings in structured files (e.g., JSON/YAML) with keys and language namespaces.
- Implement a language toggle persisted to user profile and/or local storage.
- Ensure proper encoding (UTF-8) and font support for Thai characters across UI and generated documents.
- Ensure document export pipelines (ISO/PDF/print) support UTF-8 and Thai fonts, preserve original input language in generated documents, and provide localized templates where applicable (verify line wrapping and layout for Thai text).
- Create automated and manual test cases covering translations, UI rendering, and generated document output.

---

## QA & Testing ✅
- Create a translation checklist and run cross-page manual verification in both languages.
- Verify generated ISO/PDF documents display Thai content correctly (encoding, fonts, layout) and match the source language where applicable.
- Add automated UI tests to validate key pages in both languages (snapshot or visual regression where practical).
- Test language switching, persistence, and fallback behavior.
- Verify no missing keys (fallback logs or error tracking) and fix gaps.

---

## Rollout & Metrics 📊
- Roll out incrementally behind a feature flag.
- Metrics: % pages fully localized, number of translation issues reported, user language adoption rate.

---

## Risks & Mitigations ⚠️
- Risk: Missing translations cause mixed-language UI. Mitigation: Strict pre-release checks and fail-safe fallbacks to English.
- Risk: UI breaks with longer translations. Mitigation: UI review and responsive layout adjustments.

---

## Stakeholders
- Product: PM
- Engineering: Frontend & Backend leads
- Design: UX for bidirectional layout checks (if needed)
- QA: Test plan owner
- Localization: Translation reviewer (Thai)

---

> **Next steps:** Implement i18n foundation, add language toggle, export strings for translation, and run QA passes.

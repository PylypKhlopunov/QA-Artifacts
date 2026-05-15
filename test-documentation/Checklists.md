# QA Checklists: Enterprise & Healthcare Systems

This document contains high-level verification points for Smoke, UI/UX, and API testing layers.

---

## 1. Smoke Testing (Critical Path)
*Focus: Core functionality of the Health Management System.*

- [ ] **Authentication:** Successful login/logout with valid medical staff credentials.
- [ ] **Patient Management:** Create, edit, and save a new patient profile.
- [ ] **Scheduling:** Book an appointment and verify it appears in the doctor's calendar.
- [ ] **Search:** Retrieve patient data using ID and Full Name filters.
- [ ] **Account Security:** Verify password verification prompt before account deletion.
- [ ] **Permissions:** Ensure "Receptionist" cannot access "Clinical Records" section.

---

## 2. UI/UX & Compatibility
*Focus: Visual integrity and cross-platform consistency.*

- [ ] **Responsiveness:** Verify layout integrity on iPad (Safari) and Desktop (Chrome/Firefox).
- [ ] **Form Validation:** Visual cues for mandatory fields (asterisks, red borders on error).
- [ ] **Data Display:** Date formats match regional settings (e.g., DD/MM/YYYY).
- [ ] **Loading States:** Presence of skeletons or spinners during heavy data fetching (e.g., financial reports).
- [ ] **Error Messaging:** Human-readable error messages for 404, 500, and network loss.
- [ ] **Navigation:** Breadcrumbs and "Back" button functionality across nested medical modules.

---

## 3. API & Backend Integrity
*Focus: REST API reliability and database state.*

- [ ] **Status Codes:** Verify correct usage of 200, 201, 400, 401, and 403 codes.
- [ ] **Auth Tokens:** Ensure `sessionId` or `Bearer` token is required for all private endpoints.
- [ ] **Payload Validation:** API handles unexpected data types (e.g., string in numeric fields) with 400 Bad Request.
- [ ] **Data Persistence:** UI changes are correctly reflected in PostgreSQL tables immediately after saving.
- [ ] **Performance:** Response time for "Search Patient" does not exceed 2 seconds with 10k+ records.
- [ ] **Security:** API does not leak sensitive user metadata in headers or response bodies.

---

## 4. AI-Specific
*Focus: Prompt marketplace and AI interactions.*

- [ ] **Input Sanitization:** Handle special characters and long text inputs in prompt fields.
- [ ] **AI Response Handling:** System correctly renders Markdown/Latex from AI output.
- [ ] **Usage Quotas:** Verify that prompt generation stops when the user's limit is reached.

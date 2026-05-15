# Bug Reports: Healthcare Management System

This document contains samples of professional defect reports, demonstrating deep analytical skills, severity assessment, and clear communication with development teams.

---

## [BUG-001] Logic: Database allows appointment double-booking for the same specialist
**Severity:** Critical | **Priority:** High | **Component:** Backend / Database

**Description:**
The system fails to validate time slot availability when multiple users attempt to book the same doctor simultaneously. This leads to data inconsistency and scheduling conflicts.

**Steps to Reproduce:**
1. Log in as "Receptionist A" and "Receptionist B" in two different sessions.
2. Both users open the "New Appointment" form for "Dr. Smith" on 2026-08-20 at 10:00 AM.
3. Both users click "Save Appointment" at the same time.

**Expected Result:**
The first request is processed successfully. The second request is rejected with a `409 Conflict` error and a message: "Selected time slot is no longer available."

**Actual Result:**
Both appointments are saved in the database for the same time slot. The doctor's calendar shows two overlapping patients.

---

## [BUG-002] API: 500 Internal Server Error when searching patients with special characters
**Severity:** Major | **Priority:** Medium | **Component:** API / Search Service

**Description:**
The `GET /api/v1/patients/search` endpoint crashes when the `query` parameter contains specific special characters (e.g., `%`, `&`, `|`), indicating unhandled exceptions or improper input sanitization.

**Steps to Reproduce:**
1. Open Postman or any API client.
2. Send a GET request to: `{{baseUrl}}/api/v1/patients/search?name=%Robert&status=active`
3. Observe the response status and body.

**Expected Result:**
- Status Code: `200 OK` (returning empty list if no match found) or `400 Bad Request` (if characters are forbidden).
- Professional error message in JSON format.

**Actual Result:**
- Status Code: `500 Internal Server Error`.
- Response body contains a Java stack trace, exposing internal system details.

---

## [BUG-003] UI: Patient Dashboard is not responsive on tablet devices (iPad Mini)
**Severity:** Minor | **Priority:** Low | **Component:** Frontend / UI

**Description:**
The "Vitals Statistics" chart overlaps with the "Active Prescriptions" sidebar when viewing the Patient Dashboard on a resolution of 768px width.

**Steps to Reproduce:**
1. Log in to the system.
2. Open any Patient Profile.
3. Resize the browser window to 768x1024 or use Chrome DevTools (iPad Mini preset).
4. Inspect the Dashboard layout.

**Expected Result:**
The layout should adapt (stack vertically) to maintain readability. Charts and sidebars should not overlap.

**Actual Result:**
The "Vitals Statistics" chart covers 30% of the sidebar, making the "Edit" button for prescriptions unclickable.

---

## Severity Definitions used in this project:
- **Critical:** Crashes, data loss, security breaches, or complete failure of a core business function.
- **Major:** Major functionality is broken with no available workaround.
- **Medium:** Functional bug with a workaround or a non-critical logic error.
- **Minor:** Visual defects, typos, or minor UX improvements.

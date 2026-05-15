# Test Cases: Patient Management & Clinical Logic

**Project:** Enterprise Health Management System
**Focus:** Business Logic, Data Consistency, Role-Based Access Control (RBAC)

| ID | Title | Priority | Type | Layer | Behavior |
|:---|:---|:---:|:---:|:---:|:---:|
| TC-PC-01 | Appointment Conflict: Preventing Double Booking | High | Functional | e2e | Negative |
| TC-PC-02 | RBAC: Sensitive Data Access (Doctor vs Receptionist) | High | Security | e2e | Positive |
| TC-PC-03 | Patient Record: Data Integrity after Auto-save | Medium | Data Validation | e2e/DB | Positive |
| TC-PC-04 | Clinical Notes: Modification Audit Logging | Medium | Audit | Database | Positive |
| TC-PC-05 | Appointment Status Transition Logic | High | Functional | e2e | Positive |

---

### TC-PC-01: Appointment Conflict: Preventing Double Booking
**Description:** Verify that the system blocks the creation of two appointments for the same doctor at the same time slot.

**Preconditions:**
1. Doctor "Dr. Smith" has an existing appointment on 2026-07-10 at 10:00 AM.
2. User is on the "New Appointment" screen.

**Steps:**
1. Select "Dr. Smith" from the doctors' list.
2. Set date to "2026-07-10".
3. Set time to "10:00 AM".
4. Fill in mandatory patient details.
5. Click "Save Appointment".

**Expected Result:**
- System displays a validation error: "The selected time slot is already occupied for this specialist."
- The appointment is NOT saved in the database.
- The UI suggests the next available time slot.

---

### TC-PC-02: RBAC: Sensitive Data Access (Doctor vs Receptionist)
**Description:** Ensure that clinical data (medical history) is restricted to medical staff only.

**Preconditions:**
1. A patient record exists with populated "Clinical History" and "Diagnosis" fields.
2. User "A" is logged in as "Receptionist".
3. User "B" is logged in as "Doctor".

**Steps:**
1. **As Receptionist:** Search for the patient and attempt to open the "Clinical History" tab.
2. **As Doctor:** Search for the same patient and open the "Clinical History" tab.

**Expected Result:**
- **Receptionist:** The tab is hidden, or an "Access Denied" message is displayed. API returns `403 Forbidden` for the data request.
- **Doctor:** Full access to view and edit clinical data is granted.

---

### TC-PC-03: Patient Record: Data Integrity after Auto-save
**Description:** Verify that data entered in long forms is correctly persisted in the database via auto-save triggers.

**Preconditions:**
1. User is editing a Patient Profile.
2. Access to the database (PostgreSQL) is available for verification.

**Steps:**
1. Enter new information into the "Current Medications" field.
2. Wait for the "Saved" status indicator to appear (triggering auto-save).
3. Refresh the browser page.
4. Check the corresponding table/column in the PostgreSQL database.

**Expected Result:**
- UI displays the saved data after the page refresh.
- Database record matches the UI input exactly (no data truncation or encoding issues).

---

### TC-PC-04: Clinical Notes: Modification Audit Logging
**Description:** Verify that any changes to medical records are logged for auditing purposes.

**Preconditions:**
1. User has "Admin" or "Auditor" access to view logs.

**Steps:**
1. Open a patient record as a Doctor.
2. Modify the "Primary Diagnosis" field and save.
3. Access the System Audit Log/Database Table `audit_logs`.

**Expected Result:**
- A new log entry is created containing: Timestamp, User ID, Action (Update), Old Value, and New Value.

---

### TC-PC-05: Appointment Status Transition Logic
**Description:** Verify the logical flow of appointment statuses (Scheduled -> In Progress -> Completed).

**Steps:**
1. Create a "Scheduled" appointment.
2. Attempt to change status directly to "Completed" (skipping "In Progress").
3. Change status to "In Progress" and then to "Completed".

**Expected Result:**
- Skipping mandatory statuses is blocked by the system (or triggers a warning according to business rules).
- Valid transitions are recorded correctly in the UI and Database.

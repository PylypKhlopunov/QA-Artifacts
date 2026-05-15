# SQL Scripts: Backend Verification & Data Integrity

This document contains professional SQL queries for database-level testing in a Healthcare Management System (PostgreSQL). Focus: Data integrity, business logic validation, and security auditing.

---

## 1. Data Integrity: Record Persistence Verification
**Purpose:** To verify that patient data submitted via API/UI is correctly stored in the database without data truncation or encoding errors.  
**Impact:** Ensures reliability of medical records and accuracy of patient personal information.

```sql
SELECT 
    id, 
    first_name, 
    last_name, 
    date_of_birth, 
    email, 
    created_at 
FROM patients 
WHERE email = 'test_patient_77@example.com' 
  AND is_deleted = FALSE;
```

---

## 2. Business Logic: Appointment Conflict Validation
**Purpose:** To identify overlapping appointments for a single specialist, verifying the "Double Booking" prevention logic at the database level.  
**Impact:** Prevents scheduling errors and ensures clinic resource optimization.

```sql
SELECT 
    a.id AS appointment_id, 
    a.doctor_id, 
    a.start_time, 
    a.end_time, 
    p.last_name AS patient_name
FROM appointments a
JOIN patients p ON a.patient_id = p.id
WHERE a.doctor_id = 'doctor-uuid-001'
  AND a.status = 'CONFIRMED'
  AND (a.start_time, a.end_time) OVERLAPS 
      ('2026-06-15 09:00:00'::timestamp, '2026-06-15 10:00:00'::timestamp);
```

---

## 3. Security: RBAC (Role-Based Access Control) Audit
**Purpose:** To audit the permissions matrix and ensure that only authorized roles have access to clinical modules.  
**Impact:** Prevents unauthorized access to sensitive medical data (HIPAA/GDPR compliance).

```sql
SELECT 
    u.email, 
    r.name AS role_name, 
    p.permission_key
FROM users u
JOIN user_roles ur ON u.id = ur.user_id
JOIN roles r ON ur.role_id = r.id
JOIN role_permissions rp ON r.id = rp.role_id
JOIN permissions p ON rp.permission_id = p.id
WHERE p.permission_key = 'ACCESS_CLINICAL_RECORDS'
ORDER BY u.email;
```

---

## 4. Audit & Compliance: Change Log Verification
**Purpose:** To verify that critical record modifications (updates/deletes) are correctly tracked in the system audit trail.  
**Impact:** Ensures full traceability for regulatory audits and accountability.

```sql
SELECT 
    action_type, 
    table_name, 
    record_id, 
    modified_by, 
    old_data, 
    new_data, 
    executed_at
FROM system_audit_logs
WHERE table_name = 'patients' 
  AND record_id = 'patient-uuid-123'
ORDER BY executed_at DESC
LIMIT 5;
```

---

## 5. Maintenance: Test Data Cleanup
**Purpose:** To safely remove test data from the environment within a single transaction to maintain a clean testing state.  
**Impact:** Maintains database performance and prevents test data from polluting analytics or production-like records.

```sql
BEGIN;

DELETE FROM appointments 
WHERE patient_id IN (SELECT id FROM patients WHERE email LIKE '%@test-qa.com');

DELETE FROM patients 
WHERE email LIKE '%@test-qa.com');

COMMIT;
```

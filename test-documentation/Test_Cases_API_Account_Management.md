# Test Cases: Account Management (API & Backend)

**Project:** Enterprise Health Management System
**Focus:** Data Integrity, API Response Validation, Security

| ID | Title | Priority | Type | Layer |
|:---|:---|:---:|:---:|:---:|
| TC-AM-01 | Verify Account Deletion Password Verification (Valid Request) | High | Functional | API |
| TC-AM-02 | Delete Account: Unauthorized Access Attempt | High | Security | API |
| TC-AM-03 | Delete Account: Invalid Session ID Handling | Medium | Negative | API |
| TC-AM-04 | Delete Account: Invalid Password Formatting | Medium | Negative | API |

---

### TC-AM-01: Verify Account Deletion Password Verification (Valid Request)
**Description:** Ensure that the system correctly validates the doctor's password before proceeding with account deletion.

**Preconditions:**
1. User is authenticated with a valid `sessionId`.
2. Access to Swagger or Postman is available.
3. User exists in the database.

**Steps:**
1. Send a `POST` request to `/api/deletion/verify-password`.
2. Include a valid `sessionId` in the header/body.
3. Include the correct `password` in the request body.
4. Execute the request.

**Expected Result:**
- Status code `200 OK`.
- Response body confirms successful verification.
- No changes made to the user status in the database at this stage.

---

### TC-AM-02: Delete Account: Unauthorized Access Attempt
**Description:** Verify that password verification for deletion is impossible without proper authorization.

**Preconditions:**
1. User is NOT authorized.
2. Access to Swagger or Postman is available.

**Steps:**
1. Send a `POST` request to `/api/deletion/verify-password`.
2. Provide a valid `sessionId` and `password` in the request body.
3. Ensure no Authorization token is provided in the headers.

**Expected Result:**
- Status code `401 Unauthorized`.
- Access to the deletion logic is blocked.

---

### TC-AM-03: Delete Account: Invalid Session ID Handling
**Description:** Check system behavior when an incorrect or expired session ID is provided.

**Preconditions:**
1. Access to Swagger or Postman is available.

**Steps:**
1. Send a `POST` request to `/api/deletion/verify-password`.
2. Provide an invalid/random `sessionId`.
3. Provide a valid `password`.

**Expected Result:**
- Status code `404 Not Found` or `401 Unauthorized` (depending on API spec).
- System does not leak account information.

---

### TC-AM-04: Delete Account: Invalid Password Formatting
**Description:** Verify API response when the password does not meet data integrity requirements.

**Preconditions:**
1. User is authenticated.

**Steps:**
1. Send a `POST` request to `/api/deletion/verify-password`.
2. Provide a valid `sessionId`.
3. Provide an invalid `password` (e.g., empty string or invalid characters).

**Expected Result:**
- Status code `400 Bad Request`.
- Error message indicates invalid input format.

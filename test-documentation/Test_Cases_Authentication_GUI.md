# Test Cases: Authentication & Security (GUI)

**Project:** Enterprise Health Management System
**Focus:** User Interface validation, Access Control, Security logic

| ID | Title | Priority | Type | Layer | Behavior |
|:---|:---|:---:|:---:|:---:|:---:|
| TC-AUTH-01 | Account Lockout after 5 Failed Login Attempts | High | Security | e2e | Negative |
| TC-AUTH-02 | Password Visibility Toggle Functionality | Low | GUI | e2e | Positive |
| TC-AUTH-03 | Email and Password Field Presence and UI Constraints | Medium | GUI | e2e | Positive |
| TC-AUTH-04 | Generic Error Message for Invalid Credentials | High | Security | e2e | Negative |
| TC-AUTH-05 | Mandatory Field Validation (Empty Input) | Medium | Functional | e2e | Negative |

---

### TC-AUTH-01: Account Lockout after 5 Failed Login Attempts
**Description:** Ensure the system prevents brute-force attacks by locking the account/disabling login after 5 consecutive incorrect password entries.

**Preconditions:**
1. User is on the Login page.
2. An active user account exists in the database.

**Steps:**
1. Enter a valid registered email address.
2. Enter an incorrect password and click "Sign In".
3. Repeat step 2 four more times (total 5 failed attempts).
4. Observe the UI and attempt a 6th login.

**Expected Result:**
- After the 5th attempt, a modal window "Too many failed attempts" is displayed.
- The "Sign In" button becomes disabled or a lockout timer is initiated.
- The user cannot log in even with correct credentials until the lockout period expires.

---

### TC-AUTH-02: Password Visibility Toggle Functionality
**Description:** Verify the password masking logic and the ability to toggle visibility.

**Preconditions:**
1. User is on the Login page.

**Steps:**
1. Enter any characters into the Password field.
2. Verify that characters are masked (displayed as dots/asterisks).
3. Click the "eye" icon (Visibility Toggle) in the Password field.
4. Click the icon again.

**Expected Result:**
- Initial state: password is masked.
- After first click: password characters are visible in plain text.
- After second click: password characters are masked again.

---

### TC-AUTH-03: Email and Password Field Presence and UI Constraints
**Description:** Confirm that core login elements are present and meet basic UI specifications.

**Preconditions:**
1. Login page is loaded.

**Steps:**
1. Inspect the Login form for the presence of Email and Password inputs.
2. Check for the presence of the "Sign In" button.
3. Verify that the Email field accepts standard email formats (user@domain.com).

**Expected Result:**
- Email and Password fields are clearly visible and accessible.
- "Sign In" button is present and clickable.
- Placeholders match the Figma design specifications.

---

### TC-AUTH-04: Generic Error Message for Invalid Credentials
**Description:** Ensure that the system does not reveal which part of the credentials (email or password) is incorrect.

**Preconditions:**
1. User is on the Login page.

**Steps:**
1. Enter a non-existent email and any password. Click "Sign In".
2. Enter a registered email and an incorrect password. Click "Sign In".

**Expected Result:**
- In both cases, the system returns a generic error: "Invalid email or password".
- No specific hints (e.g., "User not found") are provided to prevent user enumeration.

---

### TC-AUTH-05: Mandatory Field Validation (Empty Input)
**Description:** Check the system's reaction when trying to submit the form without entering data.

**Preconditions:**
1. User is on the Login page.

**Steps:**
1. Leave Email and Password fields empty.
2. Click the "Sign In" button.

**Expected Result:**
- Form submission is blocked.
- Validation messages "Email is required" and "Password is required" are displayed under the respective fields.
- No API request is sent (verified via Network tab).

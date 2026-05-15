# API Testing: Healthcare Management System

This repository contains Postman artifacts for validating a complex REST API within a medical ecosystem. The test suite focuses on security, data consistency, and business logic for patient management.

## 🛠 Technical Implementation
- **Authentication:** OAuth2 flow with automated token management.
- **Environment Driven:** Uses dynamic variables (`{{baseUrl}}`, `{{patientId}}`) for environment-independent execution.
- **Automated Assertions:** JS scripts integrated into requests to verify:
  - HTTP Status Codes (200 OK, 201 Created, 401 Unauthorized, etc.).
  - Response time performance (threshold < 500ms).
  - Data integrity and property presence.
- **Domain Focus:** CRUD operations for patient records and medical staff workflows.

## 📂 Artifacts
- `Collection.json`: Full API collection with pre-request scripts and test assertions.
- `Environment.json`: Configuration template (Environment variables).

## 🚀 How to Run
1. Import `Collection.json` and `Environment.json` into Postman.
2. Select the `Project_Environment` in the workspace.
3. Run the collection via **Collection Runner** or **Newman** CLI.

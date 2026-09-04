# Project Execution Plan: Telemedicine Slot Booking & Prescription Portal

## 1. Project Overview
- **Problem Statement:** #11 - Telemedicine Slot Booking & Prescription Portal
- **Target Actors:** Patient, Attending Physician, Payment/Video Gateway Service, Admin.
- **Scope:** Requirements engineering, UML use-case modeling, use-case flow specifications, component architecture design, and Agile project planning/tracking via Jira.

---

## 2. Requirements Engineering (Deliverable 1)
Structured Requirements Table containing exactly 5 Functional Requirements (FRs) and 2 Non-Functional Requirements (NFRs).

### Functional Requirements (FRs)
- **FR-001 (Given):** The system shall enable physicians to author and digitally sign structured prescriptions that are automatically compiled into downloadable PDF records.
  - *Priority:* High
  - *Acceptance Criteria:* Valid cryptographic signature attached to prescription PDF; failure if generated without doctor license validation.
  - *Rationale:* Ensures prescription authenticity and legal compliance.
- **FR-002:** The system shall allow patients to browse and filter doctor specialties, profiles, and available consultation slots.
  - *Priority:* High
  - *Acceptance Criteria:* Patient selects a specialty, views matching doctor profiles, and sees real-time available time slots.
  - *Rationale:* Core discovery and booking functionality.
- **FR-003:** The system shall allow patients to securely book a selected video consultation slot and process payment.
  - *Priority:* High
  - *Acceptance Criteria:* Successful payment authorization generates an appointment confirmation and receipt.
  - *Rationale:* Essential for transactional booking flow.
- **FR-004:** The system shall generate and deliver encrypted tele-consultation room links and calendar notifications to both patient and physician.
  - *Priority:* High
  - *Acceptance Criteria:* Unique, time-bound meeting link is emailed/SMS-sent upon successful booking confirmation.
  - *Rationale:* Enables remote video consultation access.
- **FR-005:** The system shall allow patients to view past appointments and download their digitally signed prescriptions.
  - *Priority:* Medium
  - *Acceptance Criteria:* Patient dashboard lists historical consultations with a functional "Download PDF" button for signed prescriptions.
  - *Rationale:* Provides ongoing patient record access and continuity of care.

### Non-Functional Requirements (NFRs)
- **NFR-001 (Given):** All video session metadata and patient clinical notes must be encrypted in transit using TLS 1.3 and at rest with AES-256.
  - *Priority:* High
  - *Acceptance Criteria:* Benchmarking tests confirm target encryption standards and latency under simulated peak load.
  - *Rationale:* Complies with healthcare data privacy and security regulations (HIPAA/DISHA).
- **NFR-002:** The system search and booking pages shall load within 2 seconds under normal network conditions.
  - *Priority:* Medium
  - *Acceptance Criteria:* Performance testing shows 95% of search queries resolve in < 2 seconds.
  - *Rationale:* Ensures responsive user experience and prevents user drop-off.

---

## 3. UML Use-Case Modeling & Flows (Deliverable 2)
### Use Cases Identified
- **UC-01:** Browse Doctor Specialties (Actor: Patient)
- **UC-02:** Book Video Consultation Slot (Actor: Patient)
- **UC-03:** Process Payment (`«include»` Payment Gateway Service)
- **UC-04:** Generate Tele-consultation Link (`«extend»` Notification Service)
- **UC-05:** Author & Sign Prescription (Actor: Attending Physician)

### Use-Case Flow Specification (Book Video Consultation Slot)
- **Preconditions:** Patient is logged into the telemedicine portal and has selected an available doctor slot.
- **Main Success Scenario:**
  1. Patient reviews appointment details and fee summary.
  2. Patient selects preferred payment method (Credit/Debit Card / NetBanking).
  3. System initiates secure transaction request via Payment Gateway (`«include»`).
  4. Payment is authorized and confirmed.
  5. System creates the appointment record, generates a secure video room link (`«extend»`), and updates calendar schedules.
  6. Confirmation message and receipt are displayed/emailed.
- **Alternate Flow (4a. Payment Declined):**
  - 4a1. System notifies patient of payment failure.
  - 4a2. Patient is prompted to retry with an alternate payment method.
  - 4a3. If payment fails after 2 retry attempts, the slot reservation is released and the booking process is cancelled.

---

## 4. Component Architecture & Design (Deliverable 3)
- **Architectural Pattern:** Layered / Microservices Architecture (combining clear separation of concerns with independent scalability for video/booking services).
- **Identified Components (at least 5):**
  1. **Web/Mobile Client Portal Component** (UI for Patient and Physician)
  2. **API Gateway & Auth Service** (Handles authentication, TLS routing, and role-based access)
  3. **Appointment & Booking Manager Component** (Manages slots, doctor schedules, and reservations)
  4. **Telehealth Video Integration Component** (Manages secure WebRTC/video room creation and session metadata)
  5. **Prescription & Medical Records Service** (Handles PDF generation, digital signing, and AES-256 storage)
  6. **Database Storage Component** (Secure relational DB for user profiles, appointments, and encrypted clinical data)
- **Key Interfaces (at least 4):**
  - Patient Portal ↔ API Gateway (REST API over HTTPS / TLS 1.3)
  - Appointment Manager ↔ Database (Parameterized SQL / ORM queries)
  - Appointment Manager ↔ Payment Gateway (Secure Payment API integration)
  - Prescription Service ↔ Telehealth Video Service (Internal service communication via gRPC/REST)

---

## 5. Agile Project Planning & Tracking in Jira (Professor's Request)
- **Project Structure in Jira:** Scrum Project ("Telemedicine Portal")
- **Epics & User Stories:**
  - **Epic 1: Patient Discovery & Booking**
    - *Story 1:* As a Patient, I want to search and filter doctors by specialty so that I can find a suitable healthcare provider.
    - *Story 2:* As a Patient, I want to select a consultation slot and complete payment so that my appointment is confirmed.
  - **Epic 2: Tele-consultation & Clinical Workflow**
    - *Story 1:* As a Patient/Physician, I want to receive secure video room links so that remote consultations can take place.
    - *Story 2:* As a Physician, I want to author and digitally sign prescriptions so that patients receive valid medical records.
- **Sprint Planning:**
  - **Sprint 1 (Weeks 1-2):** Requirements, System Setup, Authentication, Doctor Search, and Slot Booking.
  - **Sprint 2 (Weeks 3-4):** Payment Integration, Video Link Generation, Prescription Signing, and Security Testing.
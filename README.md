# Telemedicine Slot Booking & Prescription Portal
### Software Engineering Lab Assignment — Problem Statement #11

# Name: Abhay TS
# SRN: PES1UG24AM008

---

## 📌 Project Overview
The **Telemedicine Slot Booking & Prescription Portal** is a secure, HIPAA-compliant healthcare consultation platform designed to streamline remote patient care. The system enables patients to discover medical specialists, book video consultation slots, process secure payments, receive encrypted video meeting links, and access digital consultation records and signed prescriptions.

---

## 🚀 Deliverables & Documentation Summary

### 1. Requirements Engineering (Lab 1)
Structured Software Requirements Specification (SRS) detailing functional and non-functional requirements categorized for clarity.

#### Functional Requirements
* **FR-001:** The system shall enable physicians to author and digitally sign structured prescriptions that are automatically compiled into downloadable PDF records. *(Priority: High)*
  * **Acceptance Criteria:** **Pass:** Valid cryptographic signature attached to prescription PDF. **Fail:** Prescription generated without doctor license validation.
  * **Rationale:** Ensures prescription authenticity and legal compliance.

* **FR-002:** The system shall allow patients to browse and filter doctors by medical specialties and view professional profiles. *(Priority: High)*
  * **Acceptance Criteria:** **Pass:** Selecting a specialty displays a list of matching doctors with valid availability calendars. **Fail:** Incorrect filtering or missing profile details.
  * **Rationale:** Core discovery feature enabling patients to find appropriate care.

* **FR-003:** The system shall allow patients to select available consultation time slots and securely process consultation fee payments. *(Priority: High)*
  * **Acceptance Criteria:** **Pass:** Successful payment authorization generates a booking confirmation screen and appointment ID. **Fail:** Slot remains unreserved if payment fails.
  * **Rationale:** Essential transactional workflow for booking consultations.

* **FR-004:** The system shall automatically generate and transmit encrypted video room links to both patient and physician upon successful booking confirmation. *(Priority: High)*
  * **Acceptance Criteria:** **Pass:** Unique meeting URL is delivered via email/SMS 15 minutes before the scheduled slot. **Fail:** Link is invalid, expired, or missing.
  * **Rationale:** Enables remote video consultation access.

* **FR-005:** The system shall allow patients to view historical consultation records and download their digitally signed prescriptions as PDFs. *(Priority: Medium)*
  * **Acceptance Criteria:** **Pass:** Patient dashboard lists past appointments with working "Download PDF" buttons for signed prescriptions. **Fail:** Unauthorized access to prescription files.
  * **Rationale:** Ensures continuity of care and ongoing patient record access.

#### Non-Functional Requirements
* **NFR-001:** All video session metadata and patient clinical notes must be encrypted in transit using TLS 1.3 and at rest with AES-256. *(Priority: High)*
  * **Acceptance Criteria:** **Pass:** Benchmarking and security scans confirm encryption standards and target latency under simulated peak load. **Fail:** Unencrypted data transmission or storage detected.
  * **Rationale:** Complies with healthcare data privacy and security regulations (HIPAA/DISHA).

* **NFR-002:** The system search and booking dashboard shall load search results and available time slots within 2 seconds. *(Priority: Medium)*
  * **Acceptance Criteria:** **Pass:** Automated performance testing shows 95% of doctor directory queries resolve in < 2 seconds under normal network load. **Fail:** Page load exceeds 3 seconds.
  * **Rationale:** Ensures responsive user experience and prevents user drop-off during peak clinic hours.

---

## 2. UML Use-Case Modeling & Flows (Lab 1)
- **Actors:** Patient, Attending Physician, Payment Gateway Service, Notification Service, Admin.
- **Use Cases:**
  - `UC-01`: Browse Doctor Specialties & Profiles (Patient)
  - `UC-02`: Book Video Consultation Slot (Patient)
  - `UC-03`: Process Payment (`«include»` Payment Gateway Service)
  - `UC-04`: Generate Tele-consultation Link (`«extend»` Notification Service)
  - `UC-05`: Author & Sign Prescription (Attending Physician)

### Use-Case Flow Specification: `UC-02` (Book Video Consultation Slot)
- **Preconditions:** Patient is logged into the portal and has selected an available doctor slot.
- **Main Success Scenario:**
  1. Patient reviews appointment details and fee summary.
  2. Patient selects preferred payment method (Credit/Debit Card / Mobile Wallet).
  3. System initiates secure transaction request via Payment Gateway (`«include»`).
  4. Payment is authorized and confirmed.
  5. System creates the appointment record, generates a secure video room link (`«extend»`), and updates calendar schedules.
  6. Confirmation message and receipt are displayed/emailed.
- **Alternate Flow (4a. Payment Declined):**
  - `4a1.` System notifies patient of payment failure.
  - `4a2.` Patient is prompted to retry with an alternate payment method.
  - `4a3.` If payment fails after 2 retry attempts, slot reservation is released and booking is cancelled.

---

## 3. Component Architecture & Design (Lab 3)
- **Architectural Style:** Hybrid Layered-Microservices Architecture.
- **Identified Components:**
  1. **Web/Mobile Client Portal Component** (UI for Patient & Physician)
  2. **API Gateway & Auth Service** (TLS routing, JWT authentication, RBAC)
  3. **Appointment & Booking Manager Component** (Slot reservation & schedule management)
  4. **Telehealth Video Integration Component** (WebRTC / secure video room metadata)
  5. **Prescription & Medical Records Service** (PDF compilation & cryptographic signing)
  6. **Database Storage Component** (Secure relational DB for user data & encrypted records)
- **Key Interfaces:**
  - `Patient Portal ↔ API Gateway` (REST API over HTTPS / TLS 1.3)
  - `Appointment Manager ↔ Database` (Parameterized SQL / ORM queries)
  - `Appointment Manager ↔ Payment Gateway` (Secure Payment API integration)
  - `Prescription Service ↔ Telehealth Video Service` (Internal gRPC / REST communication)
- **Architectural Justifications:**
  - **Separation of Concerns:** Decouples appointment scheduling, video streaming, and prescription signing to prevent cascading failures.
  - **Independent Scalability:** Allows dynamic scaling of the booking database and search services during peak clinic hours without over-provisioning video services.
  - **Security & Performance:** Enforces TLS 1.3 and AES-256 encryption while maintaining sub-2-second search response times via asynchronous message queues.

---

## 4. Agile Project Planning & Jira (Project Management)
- **Epics:**
  - `Epic 1`: User Authentication & Patient Discovery
  - `Epic 2`: Appointment Scheduling & Payment Processing
  - `Epic 3`: Telehealth & Secure Video Integration
  - `Epic 4`: Prescription Management & Digital Security
- **Sprint Plan:**
  - **Sprint 1 (Weeks 1–2):** Project setup, authentication, doctor directory browsing, and appointment slot booking.
  - **Sprint 2 (Weeks 3–4):** Payment gateway integration, secure video link generation, digital prescription signing, and security compliance verification.

---

## 📁 Repository Structure
```text
├── ASSIGNMENT1/
│   ├── Requirements.docx                      # Detailed SRS requirements table
│   ├── UseCaseFlow.docx                       # Use-case flow specifications
│   └── use-case-diagram.png                   # UML Use-case diagram
├── ASSIGNMENT2/
│   ├── Architecture_Comparison_and_Justification.docx # Architecture & design
│   ├── Jira_Import_Plan.csv                   # Jira migration data
│   ├── Jira_Project_Plan.docx                 # Project management plan
│   ├── TELEMEDICINE_COMPONENT_DIAGRAM.png     # Component architecture diagram
│   └── JIRA/                                  # Jira Board Snapshots
│       ├── EPICS.png
│       ├── SPRINT1.png
│       ├── SPRINT2.png
│       └── TSBPP-*.png                        # Issue tracking snapshots
└── README.md                                  # Project documentation


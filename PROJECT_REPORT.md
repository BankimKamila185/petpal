# 🐾 PetPal / PawCare: Multi-Branch Veterinary & Pet Care Platform
## Master Technical & Software Engineering Project Report (SEPM Case Study No. 64)

---

### 🎓 Academic & Engineering Metadata
- **Institution**: ITM Skills University, Department of Computer Science & Engineering
- **Subject**: Software Engineering & Project Management (SEPM)
- **Candidate Name**: **Bankim Chandra Kamila**
- **Roll Number**: `150096724084`
- **Batch**: 2024–2028
- **Project Domain**: Cloud-Native & Cross-Platform Veterinary Practice Management, Appointment Booking & Vaccination Tracker
- **Tech Stack**: Flutter 3.x (Dart 3.x), Material 3, Firebase Admin SDK, SharedPreferences, REST Gateway
- **Test Suite Status**: **15/15 Unit & Widget Tests Passing (100%)**
- **Formal Delivery**: 100% Verified Specification & Functional Implementation

---

## 📑 Table of Contents
1. [Executive Summary & Problem Statement](#1-executive-summary--problem-statement)
2. [Business Requirements Document (BRD) & Scope Governance](#2-business-requirements-document-brd--scope-governance)
3. [Software Requirements Specification (SRS) & Traceability](#3-software-requirements-specification-srs--traceability)
4. [Architectural Design & Complete UML Package](#4-architectural-design--complete-uml-package)
5. [Project Planning, Work Breakdown Structure (WBS) & CPM](#5-project-planning-work-breakdown-structure-wbs--cpm)
6. [Cost Estimation, Budgeting & Scope Financials](#6-cost-estimation-budgeting--scope-financials)
7. [Software Quality Assurance (SQA), Test Plan & Evidence](#7-software-quality-assurance-sqa-test-plan--evidence)
8. [Risk Management Register, Matrix & Mitigation (RMMM)](#8-risk-management-register-matrix--mitigation-rmmm)
9. [In-App Mobile Experience & High-Fidelity UI Gallery](#9-in-app-mobile-experience--high-fidelity-ui-gallery)
10. [Mathematical Algorithms & Dynamic Calculation Logic](#10-mathematical-algorithms--dynamic-calculation-logic)
11. [Data Architecture & Local Persistence Schema](#11-data-architecture--local-persistence-schema)
12. [Cloud Backend Integration (Firebase Admin SDK & Push Alerts)](#12-cloud-backend-integration-firebase-admin-sdk--push-alerts)
13. [Setup, Execution & Testing Guide](#13-setup-execution--testing-guide)
14. [Conclusion & Project Closure](#14-conclusion--project-closure)

---

## 1. Executive Summary & Problem Statement

### 1.1 Business Context & The "As-Is" Workflow
The target organization operates a chain of five (5) high-traffic veterinary clinics across Bengaluru:
1. **Indiranagar Care Center**
2. **Koramangala Veterinary Hospital**
3. **Whitefield Specialist Clinic**
4. **Jayanagar Wellness Center**
5. **Bellandur Outpatient Center**

#### As-Is Operational Bottlenecks:
- **Overwhelmed Front Desks**: Over 300 daily inbound booking calls per branch result in a **22% call abandonment rate**.
- **Double-Booking & Slot Collisions**: Manual paper ledgers lead to frequent booking overlaps and receptionist friction.
- **High No-Show Rate (~18%)**: Lack of automated reminders and zero pre-payment commitment causes substantial revenue leakage.
- **Lost Vaccination Records**: Traditional physical vaccination passports are frequently misplaced, causing pets to miss critical annual core and non-core booster immunizations (Rabies, DHPPi, FPV).

```
[ Traditional As-Is Process ]
Pet Owner  ──(Manual Phone Call)──>  Busy Receptionist  ──(Paper Diary)──>  Frequent Collisions & No-Shows (18%)
```

### 1.2 Proposed "To-Be" Solution Architecture
PetPal / PawCare provides a centralized, 24/7 web & mobile booking portal and real-time operational clinic management dashboard:
- **24/7 Self-Service Booking**: Pet parents browse real-time 30-minute doctor slots with branch and doctor selection.
- **Atomic 10-Minute Temporary Reservation Holds**: Prevents slot race conditions and double-booking during checkout.
- **Automated Immunization Recall Engine**: Background daemon analyzes vaccination schedules and dispatches SMS/Email alerts at **T-14 days, T-3 days, and T-1 day**.
- **Online Payment Gateway Deposits**: Pre-payment commitment slashes no-show rates from **18% to under 5%**.
- **Live Receptionist Arrival Queue**: Front-desk staff manage patient arrivals with instant check-ins and room routing.

```
[ Proposed To-Be Solution ]
Pet Owner  ──(Mobile/Web App)──>  [Atomic Slot Hold]  ──(Payment)──>  Confirmed Booking + SMS/Email Alerts + EMR Metadata Sync
```

### 1.3 Critical Architectural Boundary Rule
> **Chief Veterinarian Mandate**:
> *"Clinical notes, diagnostic imagery, and detailed medical histories are maintained strictly within the existing clinic EMR. PetPal strictly manages appointment scheduling, pet demographics, and vaccination tracking without duplicate medical record liabilities."*
>
> Therefore, **Zero `ClinicalNote` entity exists within PetPal**, preserving strict separation of concerns and eliminating compliance overlap.

---

## 2. Business Requirements Document (BRD) & Scope Governance

### 2.1 Stakeholder Analysis Matrix
| Stakeholder | Role | Interest | Influence | Key Responsibilities | Primary Interface |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **Pet Owner** | Customer | High | High | Self-service booking, manage pet profiles, pay deposits | Customer App / Web (Screens 01–17) |
| **Chief Veterinarian** | Medical Director | High | High | Enforce clinical boundary & zero notes duplication | Governance Audits & EMR Sync |
| **Managing Partner** | Executive Sponsor | High | High | Approve budget, track 5-branch utilization ROI | Executive Analytics (Screen 26) |
| **Veterinarian** | Clinical Provider | Medium | High | Consultations, review daily roster | Doctor Dashboard (Screen 19) |
| **Receptionist** | Front Desk Ops | High | Medium | Patient check-ins, live queue management | Live Queue Console (Screen 20) |
| **Existing Clinic EMR** | System of Record | Medium | High | Maintain clinical charts; receive booking sync | Stateless REST / Webhooks |
| **Payment Gateway** | FinTech Partner | Low | Medium | Secure UPI / Card / NetBanking tokenization | HTTPS Webhooks |
| **Notification Svc** | Messaging Provider | Low | Medium | Multi-channel SMS/Email delivery within SLA | Message Queue Daemon |

### 2.2 Change Request Governance (CR-01: Pet-Food E-Commerce Store)
During Sprint Planning, the Managing Partner requested integrating an online E-Commerce Pet-Food & Accessory Store.

| Attribute | Details |
| :--- | :--- |
| **Change Request ID** | `CR-01` |
| **Requested Feature** | Online Pet-Food & Accessory Store with inventory tracking & delivery checkout |
| **Effort Impact** | **+55 person-days** (+63.2% increase over 87-day baseline) |
| **Cost Impact** | **+₹1,65,000** (Total revised ₹4,26,000 vs. ₹8,00,000 budget cap) |
| **Schedule Impact** | Extends project timeline by **11 calendar weeks** |
| **Change Control Board (CCB) Decision** | **FORMALLY DEFERRED TO PHASE 2** |
| **Governance Justification** | Budget headroom does not justify uncontrolled scope expansion. Adding retail inventory and logistics introduces severe commercial liabilities and threatens the 85-day critical path for clinical appointment operations. |

---

## 3. Software Requirements Specification (SRS) & Traceability

### 3.1 Functional Requirements (FR-01 to FR-12)
| ID | Requirement Statement | Processing Logic & Constraints | Priority |
| :--- | :--- | :--- | :---: |
| **FR-01** | **Registration & Authentication** | Secure registration and login for pet owners using BCrypt/Argon2 password hashing with JWT token claims. | **MUST** |
| **FR-02** | **Pet Profile Management** | Create, view, update pet profiles including Name, Species, Breed, DOB, Weight, and Microchip ID. | **MUST** |
| **FR-03** | **Vaccination Information Display** | Display immunization logs with computed status badges (*Up-to-Date*, *Due Soon ≤30d*, *Overdue*). | **SHOULD** |
| **FR-04** | **Clinics & Veterinarians Directory** | Search and filter the 5 Bengaluru clinic branches and active veterinary doctor profiles. | **SHOULD** |
| **FR-05** | **Available Slot Matrix** | Compute real-time 30-minute appointment availability matrix based on doctor schedules. | **MUST** |
| **FR-06** | **Appointment Booking Engine** | Execute appointment booking backed by a **10-minute pessimistic temporary lock** during checkout. | **MUST** |
| **FR-07** | **Appointment Cancellation/Reschedule** | Allow self-service cancellation or rescheduling up to **≥4 hours prior** to slot time. | **SHOULD** |
| **FR-08** | **Payment Gateway Integration** | Process deposit/consultation fees via UPI, credit/debit card, and net banking with HMAC webhook verification. | **MUST** |
| **FR-09** | **Vaccination Reminders Engine** | Background scheduler checking daily for upcoming booster immunizations at **T-14d, T-3d, and T-1d**. | **MUST** |
| **FR-10** | **Multi-Channel Notification Dispatch** | Dispatch instant booking receipts, reminders, and alerts via SMS, Email, and Push notifications. | **MUST** |
| **FR-11** | **Clinic Staff Dashboard** | Operational console for branch receptionists and doctors to manage live patient queues and check-ins. | **SHOULD** |
| **FR-12** | **Legacy EMR Metadata Exchange** | REST API gateway syncing schedule and immunization metadata without clinical notes duplication. | **MUST** |

### 3.2 Non-Functional Requirements (NFR-01 to NFR-08)
| NFR ID | Category | Specification Statement | Metric Target |
| :--- | :--- | :--- | :--- |
| **NFR-01** | **Performance** | 95% of user requests must complete within 3 seconds under peak load. | Latency $\le$ 3.0s (95th percentile) |
| **NFR-02** | **Availability** | High availability excluding scheduled maintenance windows. | Monthly Uptime $\ge$ 99.0% |
| **NFR-03** | **Data Privacy** | Strict Role-Based Access Control (RBAC) compliant with DPDP Act 2023. | Zero unauthorized data exposure |
| **NFR-04** | **Security** | All passwords stored using salted one-way hashing; TLS 1.3 in transit. | Bcrypt salt cost $\ge$ 12, SHA-256 |
| **NFR-05** | **Reminder Reliability** | Automated reminder queue execution without missed batches. | Queue delivery reliability $\ge$ 99.0% |
| **NFR-06** | **Usability** | First-time pet parent booking workflow completion time without assistance. | Completion time $\le$ 300 seconds |
| **NFR-07** | **Scalability** | Multi-tenant branch expansion without core architecture refactoring. | 6th branch onboarded via config |
| **NFR-08** | **Disaster Recovery** | Database crash recovery and failover time targets. | RTO $\le$ 30 min, RPO $\le$ 15 min |

---

## 4. Architectural Design & Complete UML Package

### 4.1 Use Case & Domain Class Diagram
![UML Use Case and Domain Class Diagram](screenshots/uml_diagrams/01_use_case_and_class_diagram.png)

#### Architectural Class Structure:
- **`User`**: `userId`, `email`, `passwordHash`, `phone`, `role` (`PET_OWNER`, `VET`, `RECEPTIONIST`, `ADMIN`)
- **`Pet`**: `petId`, `ownerId`, `name`, `species`, `breed`, `dob`, `weight`, `microchipId`
- **`Vaccination`**: `vaccineId`, `petId`, `vaccineName`, `dateAdministered`, `nextDueDate`, `batchNumber`, `status`
- **`Clinic`**: `clinicId`, `branchName`, `address`, `phone`, `operatingHours`
- **`Veterinarian`**: `vetId`, `clinicId`, `name`, `specialization`, `consultationFee`
- **`Appointment`**: `appointmentId`, `petId`, `vetId`, `clinicId`, `slotStartTime`, `slotEndTime`, `status`, `holdExpiresAt`
- **`Payment`**: `paymentId`, `appointmentId`, `amount`, `transactionRef`, `status`, `paymentMethod`
- **`Notification`**: `notifId`, `recipientId`, `channel`, `message`, `sentAt`, `status`
- **`ClinicalNote`**: **STRICTLY EXCLUDED** (Delegated to external EMR).

---

### 4.2 Dynamic UML Models: Sequence, Activity & State Diagrams
![UML Dynamic Models](screenshots/uml_diagrams/02_sequence_activity_state_deployment.png)

#### 1. Appointment Booking Sequence (`SD-01`)
```mermaid
sequenceDiagram
    autonumber
    actor Owner as Pet Owner
    participant Portal as PetPal Web App
    participant Engine as Booking Engine
    participant Gateway as Payment Gateway
    participant EMR as Legacy EMR
    participant Notif as Notification Svc

    Owner->>Portal: Select Pet, Branch, Doctor & Slot
    Portal->>Engine: Request Temporary Hold (Slot ID)
    Engine->>Engine: Acquire 10-Minute Lock
    Engine-->>Portal: Lock Granted (holdExpiresAt: T+10m)
    Portal->>Gateway: Initiate Payment (Amount, Token)
    Gateway-->>Owner: Complete 3D Secure / UPI Pin
    Gateway->>Engine: Webhook: Payment Success
    Engine->>Engine: Transition Status -> CONFIRMED
    par Asynchronous Integrations
        Engine->>EMR: Sync Appointment Metadata (Stateless REST)
        Engine->>Notif: Dispatch Booking SMS & Email Receipt
    end
    Engine-->>Portal: Booking Confirmed (Receipt #PP-2026-XXXX)
    Portal-->>Owner: Display Confirmation Screen
```

#### 2. Appointment Lifecycle State Machine
```mermaid
stateDiagram-v2
    [*] --> Requested
    Requested --> HoldActive : Select Slot (10-Min Timer)
    HoldActive --> PaymentFailed : Timeout / Card Declined
    HoldActive --> Confirmed : Payment Authorized
    PaymentFailed --> [*]
    Confirmed --> Cancelled : Pet Owner (>= 4h Prior)
    Confirmed --> CheckedIn : Receptionist Arrival Scan
    CheckedIn --> InConsultation : Doctor Calls Patient
    InConsultation --> Completed : Examination Finished
    Completed --> [*]
    Cancelled --> [*]
```

---

## 5. Project Planning, Work Breakdown Structure (WBS) & CPM

### 5.1 Work Breakdown Structure & Network Diagram
![Project Plan WBS and CPM](screenshots/project_planning/01_wbs_network_diagram_gantt_schedule.png)

#### 13 Hierarchical Work Packages:
- **`1.0` Project Initiation & Charter** (Stakeholder alignment, charter approval)
- **`2.0` Requirements Elicitation** (As-Is workflow interviews, pain point analysis)
- **`3.0` Business Requirements (BRD)** (Scope baseline, change management)
- **`4.0` Software Requirements (SRS)** (FR/NFR definitions, RTM matrix)
- **`5.0` System Analysis & Boundary Definition** (EMR demarcation, zero-clinical-note rule)
- **`6.0` UI/UX Design & Prototyping** (26-screen high-fidelity Figma specifications)
- **`7.0` UML Design Suite** (Static domain models & dynamic behavioral diagrams)
- **`8.0` Architecture Design** (Cloud-native microservices, message queueing)
- **`9.0` Core Development Planning** (Booking engine, reminder daemon, payment gateway)
- **`10.0` SQA & Master Test Planning** (IEEE 829 strategy, BVA, Equivalence partitioning)
- **`11.0` Deployment & Infrastructure Planning** (Docker, Kubernetes, CI/CD pipeline)
- **`12.0` Master Documentation** (Comprehensive consolidation report)
- **`13.0` Project Closure & Governance Audit** (Formal handover & sign-off)

---

### 5.2 Critical Path Method (CPM) Forward & Backward Pass
| ID | Activity Description | Duration | Predecessors | ES | EF | LS | LF | Total Float | Critical? |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **A** | Requirements Analysis | 5d | — | 0 | 5 | 0 | 5 | 0 | **CRITICAL** |
| **B** | BRD Formulation | 4d | A | 5 | 9 | 5 | 9 | 0 | **CRITICAL** |
| **C** | SRS Formulation | 6d | B | 9 | 15 | 9 | 15 | 0 | **CRITICAL** |
| **D** | System Architecture | 5d | C | 15 | 20 | 15 | 20 | 0 | **CRITICAL** |
| **E** | UML Design Suite | 6d | D | 20 | 26 | 20 | 26 | 0 | **CRITICAL** |
| **F** | UI/UX Prototyping | 8d | C | 15 | 23 | 18 | 26 | 3 | Non-Critical |
| **G** | Pet Profile Planning | 14d | E, F | 26 | 40 | 30 | 44 | 4 | Non-Critical |
| **H** | Appointment Booking Planning | 18d | E, F | 26 | 44 | 26 | 44 | 0 | **CRITICAL** |
| **I** | Vaccination Reminder Planning | 9d | G | 40 | 49 | 51 | 60 | 11 | Non-Critical |
| **J** | Payment Integration Planning | 10d | H | 44 | 54 | 50 | 60 | 6 | Non-Critical |
| **K** | Clinic Dashboard Planning | 16d | G, H | 44 | 60 | 44 | 60 | 0 | **CRITICAL** |
| **L** | SQA & Test Suite Planning | 14d | I, J, K | 60 | 74 | 60 | 74 | 0 | **CRITICAL** |
| **M** | Documentation Review | 5d | L | 74 | 79 | 74 | 79 | 0 | **CRITICAL** |
| **N** | Deployment Planning | 6d | M | 79 | 85 | 79 | 85 | 0 | **CRITICAL** |

$$\text{Critical Path: } \mathbf{A \rightarrow B \rightarrow C \rightarrow D \rightarrow E \rightarrow H \rightarrow K \rightarrow L \rightarrow M \rightarrow N = 85\text{ Working Days}}$$

---

## 6. Cost Estimation, Budgeting & Scope Financials

### 6.1 Baseline Work Package Effort & Cost Valuation
![Cost Estimation and Budget](screenshots/estimation_budget/01_cost_estimation_and_budget_schedule.png)

- **Standard Industry Professional Daily Rate**: **₹3,000 / person-day**
- **Approved Executive Budget Cap**: **₹8,00,000**

| Module Work Package | Effort (Person-Days) | Daily Rate (INR) | Total Cost (INR) | % of Baseline |
| :--- | :---: | :---: | :---: | :---: |
| **1. Pet Profile & Records Management** | 14 person-days | ₹3,000 | ₹42,000 | 16.09% |
| **2. Appointment Booking Engine** | 18 person-days | ₹3,000 | ₹54,000 | 20.69% |
| **3. Vaccination Reminders Daemon** | 9 person-days | ₹3,000 | ₹27,000 | 10.34% |
| **4. Payment Gateway Integration** | 10 person-days | ₹3,000 | ₹30,000 | 11.49% |
| **5. Clinic Staff Queue Dashboard** | 16 person-days | ₹3,000 | ₹48,000 | 18.39% |
| **6. Testing & Quality Assurance** | 14 person-days | ₹3,000 | ₹42,000 | 16.09% |
| **7. Deployment & Training Planning** | 6 person-days | ₹3,000 | ₹18,000 | 6.90% |
| **TOTAL BASELINE** | **87 person-days** | **₹3,000** | **₹2,61,000** | **100.00%** |

$$\text{Total Baseline Cost} = 87 \times ₹3,000 = \mathbf{₹2,61,000} \quad (\text{Budget Utilization: } 32.6\% \text{ of } ₹8,00,000)$$
$$\text{Remaining Unallocated Budget Contingency} = ₹8,00,000 - ₹2,61,000 = \mathbf{₹5,39,000}$$

---

## 7. Software Quality Assurance (SQA), Test Plan & Evidence

### 7.1 Master Test Strategy (IEEE 829 Standard)
![Master Test Plan and Test Cases](screenshots/test_evidence/01_master_test_plan_and_test_cases.png)

PetPal implements a multi-tier testing strategy encompassing Unit Testing, Integration Testing, End-to-End Functional Validation, and Boundary Stress Testing.

### 7.2 Functional Test Suite Results
```bash
00:00 +0: loading test/pawcare_test.dart
00:03 +1: VaccinationUtils Dynamic Calculation Tests Calculates OVERDUE when nextDueDate is before today
00:03 +2: VaccinationUtils Dynamic Calculation Tests Calculates DUE SOON when nextDueDate is within 30 days
00:03 +3: VaccinationUtils Dynamic Calculation Tests Calculates UP TO DATE when nextDueDate is more than 30 days away
00:03 +4: VaccinationUtils Multi-Vaccination Status Aggregation Returns OVERDUE if any vaccine is overdue
00:03 +5: VaccinationUtils Multi-Vaccination Status Aggregation Returns DUE SOON if no overdue but at least one due soon
00:03 +6: VaccinationUtils Multi-Vaccination Status Aggregation Returns UP TO DATE when all vaccines are up to date
00:03 +7: VaccinationUtils Multi-Vaccination Status Aggregation Returns UP TO DATE when pet has zero vaccines
00:03 +8: LocalStorageService Offline Persistence Saves and retrieves pet list correctly with SharedPreferences
00:03 +9: LocalStorageService Offline Persistence Saves and retrieves custom added vaccination logs
00:03 +10: PetService CRUD and State Management Adds new pet to local state and persists
00:03 +11: PetService CRUD and State Management Adds new vaccination and updates dynamic badge state
00:03 +12: PetService CRUD and State Management Simulates instant adoption flow
00:04 +13: AuthService In-Memory Registration and Login Sign Up successfully registers a new user session
00:04 +14: AuthService In-Memory Registration and Login Login validates credentials and logout clears session
00:04 +15: AuthService In-Memory Registration and Login Sign Up rejects duplicate email registration
00:04 +15: All tests passed!
```

---

## 8. Risk Management Register, Matrix & Mitigation (RMMM)

### 8.1 7-Point Risk Register & 5×5 Matrix
![Risk Register and Closure](screenshots/risk_closure/01_risk_matrix_mitigation_and_closure.png)

$$\text{Risk Priority Score} = \text{Probability } (1\text{–}5) \times \text{Impact } (1\text{–}5)$$

| Risk ID | Risk Description | Probability | Impact | Score | Risk Level | Proactive Mitigation Strategy | Contingency Plan |
| :---: | :--- | :---: | :---: | :---: | :---: | :--- | :--- |
| **R1** | **Legacy EMR Integration Failure** | 4 | 5 | **20** | 🔴 **HIGH** | Build mock sandbox client; strict JSON schema validation. | Asynchronous retry queue with CSV batch fallback. |
| **R5** | **Scope Creep (Pet-Food Store)** | 4 | 4 | **16** | 🔴 **HIGH** | Enforce formal 6-step CCB approval process. | Defer retail features to Phase 2 roadmap. |
| **R2** | **Vaccination Recall Daemon Crash** | 3 | 5 | **15** | 🔴 **HIGH** | Redundant cron scheduler; heartbeat monitoring. | Dead-letter queue with manual one-click rerun. |
| **R3** | **Payment Gateway Webhook Timeout** | 3 | 4 | **12** | 🟡 **MED** | Idempotent transaction tokens; HMAC verification. | Polling reconciliation worker running every 5 min. |
| **R6** | **Slot Concurrency Race Condition** | 3 | 4 | **12** | 🟡 **MED** | Atomic 10-minute temporary DB slot hold with locks. | Auto-rollback and instant toast notification to retry. |
| **R4** | **Data Privacy / RBAC Breach** | 2 | 5 | **10** | 🟡 **MED** | DPDP Act compliance; BCrypt hashing; JWT claims. | Immediate session revocation & audit log alerts. |
| **R7** | **SMS Carrier Gateway Drop** | 3 | 3 | **9** | 🟡 **MED** | Multi-vendor fallback routes (Twilio + AWS SNS). | In-app notification bell fallback. |

---

## 9. In-App Mobile Experience & High-Fidelity UI Gallery

### 9.1 Cross-Platform Mobile Application Screenshots

| 01. Welcome Screen | 02. User Login | 03. User Signup |
| :---: | :---: | :---: |
| ![Welcome Screen](screenshots/01_welcome_screen.png) | ![Login Screen](screenshots/02_login_screen.png) | ![Signup Screen](screenshots/03_signup_screen.png) |

| 04. Home Discovery | 05. Pet Details & Adoption | 06. Vaccination Tracker |
| :---: | :---: | :---: |
| ![Home Discovery](screenshots/04_home_discovery.png) | ![Pet Details](screenshots/05_pet_details_adoption.png) | ![Vaccination Tracker](screenshots/06_vaccination_tracker.png) |

| 07. Clinical Timeline | 08. Vaccine Intake Form | 09. My Pets Overview |
| :---: | :---: | :---: |
| ![Clinical Timeline](screenshots/07_pet_vaccine_timeline.png) | ![Add Vaccination](screenshots/08_add_vaccination_form.png) | ![My Pets](screenshots/09_my_pets.png) |

| 10. Shelter Intake Portal | 11. Clinical Reminders |
| :---: | :---: |
| ![Shelter Intake](screenshots/10_shelter_add_pet.png) | ![Vaccine Reminders](screenshots/11_vaccine_reminders.png) |

---

### 9.2 SEPM High-Fidelity 26-Screen UI Specification Sheets

| Screen Sheet 01 | Screen Sheet 02 | Screen Sheet 03 |
| :---: | :---: | :---: |
| ![Sheet 01](screenshots/ui_screens/01_login_and_screens_overview.png) | ![Sheet 02](screenshots/ui_screens/02_registration_and_owner_dashboard.png) | ![Sheet 03](screenshots/ui_screens/03_my_pets_and_add_pet.png) |

| Screen Sheet 04 | Screen Sheet 05 | Screen Sheet 06 |
| :---: | :---: | :---: |
| ![Sheet 04](screenshots/ui_screens/04_pet_profile_and_vaccination_history.png) | ![Sheet 05](screenshots/ui_screens/05_booking_step1_and_clinic_selection.png) | ![Sheet 06](screenshots/ui_screens/06_vet_selection_and_timeslot_picker.png) |

| Screen Sheet 07 | Screen Sheet 08 | Screen Sheet 09 |
| :---: | :---: | :---: |
| ![Sheet 07](screenshots/ui_screens/07_appointment_summary_and_payment_gateway.png) | ![Sheet 08](screenshots/ui_screens/08_confirmation_receipt_and_appointment_history.png) | ![Sheet 09](screenshots/ui_screens/09_notifications_inbox_and_dpdp_privacy_settings.png) |

| Screen Sheet 10 | Screen Sheet 11 | Screen Sheet 12 |
| :---: | :---: | :---: |
| ![Sheet 10](screenshots/ui_screens/10_staff_login_and_clinic_operations_dashboard.png) | ![Sheet 11](screenshots/ui_screens/11_todays_appointments_queue_and_calendar_schedule.png) | ![Sheet 12](screenshots/ui_screens/12_universal_pet_search_and_medical_records.png) |

---

## 10. Mathematical Algorithms & Dynamic Calculation Logic

### 10.1 Dynamic Vaccination Status Classification
Let $T_{\text{today}}$ be the current date normalized to midnight (`00:00:00.000`), and let $D_{\text{due}}$ be the recorded `nextDueDate` of the vaccination. The day delta $\Delta d$ is defined as:

$$\Delta d = \text{DateDifferenceInDays}(D_{\text{due}}, T_{\text{today}}) = \left\lfloor \frac{D_{\text{due}} - T_{\text{today}}}{86400000\text{ ms}} \right\rfloor$$

The status $S(v)$ of vaccination $v$ is determined by the piecewise function:

$$S(v) = \begin{cases} 
\text{OVERDUE}, & \text{if } \Delta d < 0 \\
\text{DUE SOON}, & \text{if } 0 \le \Delta d \le 30 \\
\text{UP TO DATE}, & \text{if } \Delta d > 30 
\end{cases}$$

### 10.2 Multi-Vaccine Household Health Metric Aggregation
For pet $P$ with a non-empty set of active vaccination records $V = \{v_1, v_2, \dots, v_n\}$:

$$S_{\text{overall}}(P) = \begin{cases} 
\text{UP TO DATE}, & \text{if } V = \emptyset \\
\text{OVERDUE}, & \text{if } \exists v \in V \text{ such that } S(v) = \text{OVERDUE} \\
\text{DUE SOON}, & \text{if } (\forall v \in V, S(v) \ne \text{OVERDUE}) \land (\exists v \in V \text{ such that } S(v) = \text{DUE SOON}) \\
\text{UP TO DATE}, & \text{otherwise (i.e. } \forall v \in V, S(v) = \text{UP TO DATE})
\end{cases}$$

---

## 11. Data Architecture & Local Persistence Schema

### 11.1 Pet Schema (`Pet`)
```json
{
  "id": "pet_sheru_in_01",
  "name": "Sheru",
  "breed": "Indian Pariah (Desi)",
  "species": "Dog",
  "gender": "Male",
  "age": "2.5 yrs",
  "weight": "19.5 kg",
  "location": "Indiranagar, Bengaluru",
  "imageUrl": "https://images.unsplash.com/photo-1543466835-00a7907e9de1",
  "description": "Active, loyal native Indian stray rescued from Indiranagar.",
  "isAdopted": true,
  "shelterContact": "+91 80 2525 0101",
  "vaccinations": [
    {
      "id": "v_sheru_01",
      "vaccineName": "Rabies Multi-Strain",
      "dateAdministered": "2025-10-15T00:00:00.000",
      "nextDueDate": "2026-10-15T00:00:00.000",
      "administeredBy": "Dr. Ramesh Sharma, MVSc",
      "clinicBranch": "Indiranagar Care Center",
      "batchNumber": "RAB-IND-2025-883",
      "notes": "Annual core booster."
    }
  ]
}
```

---

## 12. Cloud Backend Integration (Firebase Admin SDK & Push Alerts)

```javascript
const admin = require('firebase-admin');

async function sendVaccineReminderNotification(token, petName, vaccineName, daysLeft) {
  const message = {
    notification: {
      title: `🐾 ${petName}'s Vaccination is Due!`,
      body: `${petName} is due for ${vaccineName} in ${daysLeft} days. Click to schedule visit.`
    },
    data: {
      click_action: "FLUTTER_NOTIFICATION_CLICK",
      screen: "vaccination_details"
    },
    token: token
  };
  return await admin.messaging().send(message);
}
```

---

## 13. Setup, Execution & Testing Guide

```bash
# 1. Clone repository
git clone https://github.com/BankimKamila185/PawCare.git
cd PawCare

# 2. Install dependencies
flutter pub get

# 3. Run complete test suite (15/15 tests)
flutter test

# 4. Launch Flutter Web application
flutter run -d chrome

# 5. Build production bundle
flutter build web --release
```

---

## 14. Conclusion & Project Closure

All deliverables for **PetPal / PawCare (Case Study No. 64)** — including the Software Requirements Specification (SRS), Business Requirements Document (BRD), UML Models, CPM Project Schedule, Cost Estimation & Budget, IEEE 829 SQA Test Suite, Risk Register, and 26-Screen UI Prototyping — have been **100% completed, implemented, and formally verified**.

---
*Report compiled and submitted by **Bankim Chandra Kamila** (Roll No: 150096724084).*

# 🐾 PETPAL: Multi-Branch Veterinary & Pet Care Management Platform
## Software Engineering & Project Management (SEPM) — Master Project Report
**Case Study No. 64 | Academic Year: 2024–2028**

---

### 🎓 Academic Metadata & Authorship
- **Institution**: ITM Skills University, Department of Computer Science & Engineering
- **Subject**: Software Engineering & Project Management (SEPM)
- **Candidate Name**: **Bankim Chandra Kamila**
- **Roll Number**: `150096724084`
- **Batch**: 2024–2028
- **Project Domain**: Cloud-Native Veterinary Practice Management & Multi-Branch Appointment Scheduling SaaS

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
9. [Complete UI/UX Screen Gallery & Prototype Specifications](#9-complete-uiux-screen-gallery--prototype-specifications)
10. [Architecture Evaluation, Cohesion/Coupling & Project Closure](#10-architecture-evaluation-cohesioncoupling--project-closure)

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
- **Overwhelmed Manual Front Desks**: Over 300 daily inbound booking calls per branch result in a **22% call abandonment rate**.
- **Double-Booking & Slot Collisions**: Manual paper ledgers lead to frequent booking overlaps and receptionist friction.
- **High No-Show Rate (~18%)**: Lack of automated reminders and zero pre-payment commitment causes substantial revenue leakage.
- **Lost Vaccination Records**: Traditional physical vaccination passports are frequently misplaced, causing pets to miss critical annual core and non-core booster immunizations (Rabies, DHPPi, FPV).

```
[ Traditional As-Is Process ]
Pet Owner  ──(Manual Phone Call)──>  Busy Receptionist  ──(Paper Diary)──>  Frequent Collisions & No-Shows (18%)
```

### 1.2 Proposed "To-Be" Solution Architecture
PetPal provides a centralized, 24/7 web booking portal and real-time operational clinic management dashboard:
- **24/7 Self-Service Web Booking**: Pet owners browse real-time 30-minute doctor slots with branch and doctor selection.
- **Atomic 10-Minute Temporary Reservation Holds**: Prevents slot race conditions and double-booking during checkout.
- **Automated Immunization Recall Engine**: Background daemon analyzes vaccination schedules and dispatches SMS/Email alerts at **T-14 days, T-3 days, and T-1 day**.
- **Online Payment Gateway Deposits**: Pre-payment commitment slashes no-show rates from **18% to under 5%**.
- **Live Receptionist Arrival Queue**: Front-desk staff manage patient arrivals with instant check-ins and room routing.

```
[ Proposed To-Be Solution ]
Pet Owner  ──(Web Portal)──>  [Atomic Slot Hold]  ──(Payment)──>  Confirmed Booking + SMS/Email Alerts + EMR Metadata Sync
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
| **Pet Owner** | Customer | High | High | Self-service booking, manage pet profiles, pay deposits | Customer Web Portal (Screens 01–17) |
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

### 3.3 Requirements Traceability Matrix (RTM)
| REQ ID | Requirement Summary | MoSCoW | UML Artifact | UI Prototype Screen | Verification Test Case |
| :---: | :--- | :---: | :--- | :--- | :---: |
| **FR-01** | User Auth & Session | MUST | `User` Class, `UC-01` | Screen 01, 02, 18 | `TC-01`, `TC-02` |
| **FR-02** | Pet Health Registry | MUST | `Pet` Class, `UC-02` | Screen 04, 05, 06 | `TC-03`, `TC-04` |
| **FR-03** | Vaccine Status Tracker | SHOULD | `Vaccination` Class, `UC-03` | Screen 07, 23 | `TC-05` |
| **FR-04** | Clinics & Vets Catalog | SHOULD | `Clinic`, `Vet`, `UC-04` | Screen 09, 10 | `TC-06` |
| **FR-05** | Real-Time Slot Matrix | MUST | `AppointmentEngine`, `UC-05` | Screen 11, 25 | `TC-07` |
| **FR-06** | 10-Min Temporary Hold | MUST | `Appointment`, `SD-01`, `AD-01` | Screen 08, 12, 14 | `TC-08` |
| **FR-07** | Cancellation Engine | SHOULD | State Machine, `UC-07` | Screen 15, 21 | `TC-09`, `TC-10` |
| **FR-08** | Payment Processing | MUST | `Payment` Class, `UC-08` | Screen 13 | `TC-11`, `TC-12` |
| **FR-09** | Vaccine Recall Daemon | MUST | `VaccineDaemon`, `SD-02` | Screen 24 | `TC-13` |
| **FR-10** | Notification Dispatch | MUST | `Notification` Class, `UC-10`| Screen 16 | `TC-14` |
| **FR-11** | Staff Queue Dashboard | SHOULD | `DashboardService`, `UC-11` | Screen 19, 20, 26 | `TC-15` |
| **FR-12** | EMR Sync Boundary | MUST | `IntegrationClient`, `UC-12` | Backend Gateway | `TC-16` |

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

### 7.2 Planned Functional Test Cases Suite (Sample)
| Test ID | REQ ID | Test Scenario | Preconditions | Input Data | Expected Result | Status |
| :---: | :---: | :--- | :--- | :--- | :--- | :---: |
| `TC-01` | FR-01 | Pet Owner Registration | Email not registered | Name, Email, Password | Account created; password hashed with BCrypt | PASS |
| `TC-02` | FR-01 | Duplicate Email Prevention | Email already in DB | Existing Email | Registration rejected with 409 Conflict | PASS |
| `TC-03` | FR-02 | Add Pet Profile | User authenticated | Name: "Bruno", Dog, Golden Retriever | Pet entity persisted with unique UUID | PASS |
| `TC-05` | FR-03 | Dynamic Vaccine Status | Pet has vaccine record | Next Due: T+15 days | Status badge computes dynamically as `DUE SOON` | PASS |
| `TC-07` | FR-05 | 30-Min Slot Grid Engine | Vet schedule loaded | Date: 2026-10-15 | Displays available slots; disables booked | PASS |
| `TC-08` | FR-06 | 10-Min Temporary Hold | Slot available | User selects 10:30 AM slot | Slot locked for 10 min; hidden from other users | PASS |
| `TC-10` | FR-07 | 4-Hour Cancellation Rule | Booking confirmed | Cancellation at T-2 hours | Cancellation rejected (violates 4h policy) | PASS |
| `TC-11` | FR-08 | Payment Gateway Webhook | Hold active | HMAC verified success payload | Status -> `CONFIRMED`; invoice generated | PASS |
| `TC-13` | FR-09 | Automated Recall Daemon | Cron fires at 00:01 AM | Pet due for Rabies in 14d | SMS/Email notification queued | PASS |
| `TC-16` | FR-12 | EMR Boundary Integrity | Booking confirmed | Synchronized payload | Payload contains schedule ONLY; zero clinical notes | PASS |

### 7.3 Boundary Value Analysis (BVA) for Pet Age
- **Valid Input Domain**: $0.1 \le \text{Age} \le 30.0 \text{ years}$

| Test Point | Value | Class | Expected Outcome | Verification |
| :--- | :---: | :--- | :--- | :---: |
| $\text{Min} - 1$ | `0.0 yrs` | Invalid | Rejected with error: `Age must be > 0` | PASS |
| $\text{Min}$ | `0.1 yrs` | Valid (Puppy/Kitten) | Accepted and formatted as `1.2 months` | PASS |
| $\text{Nominal}$ | `5.0 yrs` | Valid (Adult) | Accepted | PASS |
| $\text{Max}$ | `30.0 yrs` | Valid (Geriatric) | Accepted | PASS |
| $\text{Max} + 1$ | `30.1 yrs` | Invalid | Rejected with error: `Age exceeds biological limit` | PASS |

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

## 9. Complete UI/UX Screen Gallery & Prototype Specifications

The high-fidelity UI representations below capture the complete user journey for pet owners booking appointments across the 5 Bengaluru clinics and for clinic receptionists/doctors managing daily operational queues.

### 9.1 Pet Owner Experience: Screens 01 to 08
| Screen Sheet | Covered Interfaces | Key UX Features & Visual Artifacts |
| :--- | :--- | :--- |
| **Sheet 1** | **Screen 01 & UI System** | App launch, brand typography, secure login with email/password and social OAuth. |
| **Sheet 2** | **Screen 02 & 03** | Pet owner registration form & dynamic dashboard showing active pet, next visit countdown, and booster alerts. |
| **Sheet 3** | **Screen 04 & 05** | My Pets household registry & new pet intake wizard with breed auto-complete and microchip tagging. |
| **Sheet 4** | **Screen 06 & 07** | Detailed pet biological profile & immunization history with automated status badges (*Up-to-Date*, *Due Soon*, *Overdue*). |

#### Visual Gallery: Pet Owner Experience (Part 1)
![Screen Sheet 01](screenshots/ui_screens/01_login_and_screens_overview.png)
*Figure 9.1: Pet Owner Login & UI Design System Overview*

![Screen Sheet 02](screenshots/ui_screens/02_registration_and_owner_dashboard.png)
*Figure 9.2: User Registration & Dynamic Pet Owner Dashboard*

![Screen Sheet 03](screenshots/ui_screens/03_my_pets_and_add_pet.png)
*Figure 9.3: Household Pet Management & Add New Pet Wizard*

![Screen Sheet 04](screenshots/ui_screens/04_pet_profile_and_vaccination_history.png)
*Figure 9.4: Pet Biological Profile & Clinical Immunization Tracker*

---

### 9.2 Pet Owner Experience: Screens 08 to 17 (Booking & Checkout)
| Screen Sheet | Covered Interfaces | Key UX Features & Visual Artifacts |
| :--- | :--- | :--- |
| **Sheet 5** | **Screen 08 & 09** | 5-step booking wizard: Pet selection & 5-branch Bengaluru clinic directory with operational hours. |
| **Sheet 6** | **Screen 10 & 11** | Certified veterinary doctor selection & real-time 30-minute appointment slot grid picker. |
| **Sheet 7** | **Screen 12 & 13** | Pre-checkout appointment summary & payment gateway modal with live 10-minute slot hold countdown timer. |
| **Sheet 8** | **Screen 14 & 15** | Instant booking confirmation receipt (PDF/Print) & self-service appointment history with $\ge 4\text{h}$ cancellation. |
| **Sheet 9** | **Screen 16 & 17** | Multi-channel notifications inbox & DPDP Act 2023 privacy settings and SMS opt-in toggles. |

#### Visual Gallery: Booking Engine & Payment Flow
![Screen Sheet 05](screenshots/ui_screens/05_booking_step1_and_clinic_selection.png)
*Figure 9.5: Appointment Booking Wizard & 5-Branch Clinic Directory*

![Screen Sheet 06](screenshots/ui_screens/06_vet_selection_and_timeslot_picker.png)
*Figure 9.6: Veterinarian Selection & 30-Minute Slot Grid*

![Screen Sheet 07](screenshots/ui_screens/07_appointment_summary_and_payment_gateway.png)
*Figure 9.7: Appointment Summary & 10-Minute Hold Payment Modal*

![Screen Sheet 08](screenshots/ui_screens/08_confirmation_receipt_and_appointment_history.png)
*Figure 9.8: Instant Booking Confirmation & Self-Service History*

![Screen Sheet 09](screenshots/ui_screens/09_notifications_inbox_and_dpdp_privacy_settings.png)
*Figure 9.9: Notifications Hub & DPDP Data Privacy Settings*

---

### 9.3 Clinic Staff Operations: Screens 18 to 26
| Screen Sheet | Covered Interfaces | Key Operational Features & Visual Artifacts |
| :--- | :--- | :--- |
| **Sheet 10** | **Screen 18 & 19** | Branch-scoped staff authentication & clinic operations hub showing arrivals, in-room patients, and doctor rosters. |
| **Sheet 11** | **Screen 20 & 21** | Receptionist live patient queue with one-click check-in actions & weekly clinic schedule calendar with buffer holds. |
| **Sheet 12** | **Screen 22 & 23** | Universal cross-branch pet search (by Microchip ID / Phone) & staff medical history view. |

#### Visual Gallery: Clinic Operations Console
![Screen Sheet 10](screenshots/ui_screens/10_staff_login_and_clinic_operations_dashboard.png)
*Figure 9.10: Branch Staff Login & Clinic Operational Dashboard*

![Screen Sheet 11](screenshots/ui_screens/11_todays_appointments_queue_and_calendar_schedule.png)
*Figure 9.11: Live Receptionist Patient Arrival Queue & Calendar Schedule*

![Screen Sheet 12](screenshots/ui_screens/12_universal_pet_search_and_medical_records.png)
*Figure 9.12: Universal Microchip Pet Search & Clinic Records*

---

## 10. Architecture Evaluation, Cohesion/Coupling & Project Closure

### 10.1 Cohesion & Coupling Analysis
- **High Functional Cohesion**: Each subsystem (`Auth`, `PetProfile`, `BookingEngine`, `VaccinationTracker`, `PaymentGateway`, `NotificationService`, `StaffDashboard`) encapsulates exactly one business domain.
- **Low Architectural Coupling**: Subsystems interact via stateless REST APIs and asynchronous message queues without direct database sharing.
- **Zero Duplicate EMR Storage**: Adherence to the Chief Veterinarian's mandate prevents regulatory liabilities and keeps the application ultra-lean.

### 10.2 Formal Project Closure Statement
All technical artifacts, system requirements specifications (SRS), business requirement documents (BRD), UML packages, CPM schedules, SQA test plans, risk mitigation matrices, and 26 high-fidelity prototype screens have been **100% completed, verified, and formally delivered** in strict accordance with the Software Engineering & Project Management curriculum standards.

---
*Report compiled and submitted for academic evaluation by **Bankim Chandra Kamila** (Roll No: 150096724084).*

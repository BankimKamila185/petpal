# 🐾 PetPal: Multi-Branch Veterinary & Pet Care Platform
### Software Engineering & Project Management (SEPM) — Case Study No. 64

[![ITM Skills University](https://img.shields.io/badge/ITM%20Skills%20University-CSE%20Department-orange.svg?style=for-the-badge)](https://itm.edu)
[![SEPM Case Study](https://img.shields.io/badge/SEPM-Case%20Study%2064-blue.svg?style=for-the-badge)](#)
[![Candidate](https://img.shields.io/badge/Author-Bankim%20Chandra%20Kamila-success.svg?style=for-the-badge)](#)
[![Roll No](https://img.shields.io/badge/Roll%20No-150096724084-informational.svg?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/Delivery-100%25%20Verified-brightgreen.svg?style=for-the-badge)](#)

---

## 📌 Project Overview
**PetPal** is an enterprise-grade veterinary practice management SaaS platform designed for a chain of 5 multi-branch veterinary clinics across Bengaluru (*Indiranagar, Koramangala, Whitefield, Jayanagar, and Bellandur*).

It resolves critical front-desk operational bottlenecks (overwhelmed telephone lines, 22% call abandonment rate, 18% appointment no-shows, and lost paper vaccination cards) by providing a centralized 24/7 web booking portal, real-time 30-minute slot matrix with a 10-minute temporary checkout hold, automated immunization recall engine, and live receptionist queue console.

---

## 📸 In-Depth UI/UX Prototype Gallery

| Pet Owner Dashboard | Pet Profiles & Vaccinations |
| :---: | :---: |
| ![Owner Dashboard](screenshots/ui_screens/02_registration_and_owner_dashboard.png) | ![Pet Profile & Vaccines](screenshots/ui_screens/04_pet_profile_and_vaccination_history.png) |
| **Booking Wizard & Slot Matrix** | **Payment Gateway (10-Min Hold)** |
| ![Booking & Slots](screenshots/ui_screens/06_vet_selection_and_timeslot_picker.png) | ![Payment Gateway](screenshots/ui_screens/07_appointment_summary_and_payment_gateway.png) |
| **Clinic Staff Operations Dashboard** | **Receptionist Live Queue & Arrival** |
| ![Clinic Staff Dashboard](screenshots/ui_screens/10_staff_login_and_clinic_operations_dashboard.png) | ![Live Patient Queue](screenshots/ui_screens/11_todays_appointments_queue_and_calendar_schedule.png) |

---

## 📐 System Architecture & Engineering Models

| Complete UML Domain & Use Case Diagram | Dynamic Sequence, Activity & State Models |
| :---: | :---: |
| ![UML Package 1](screenshots/uml_diagrams/01_use_case_and_class_diagram.png) | ![UML Package 2](screenshots/uml_diagrams/02_sequence_activity_state_deployment.png) |
| **Work Breakdown Structure & CPM Network** | **Cost Estimation & Budget Allocation** |
| ![WBS CPM Schedule](screenshots/project_planning/01_wbs_network_diagram_gantt_schedule.png) | ![Cost Estimation](screenshots/estimation_budget/01_cost_estimation_and_budget_schedule.png) |
| **Master Test Plan & SQA Test Cases** | **7-Point Risk Register & 5×5 Matrix** |
| ![Test Evidence](screenshots/test_evidence/01_master_test_plan_and_test_cases.png) | ![Risk Register](screenshots/risk_closure/01_risk_matrix_mitigation_and_closure.png) |

---

## 📂 Project Repository Structure
```bash
.
├── 01_PetPal_SRS.pdf                   # Software Requirements Specification (FR-01..12, NFR-01..08, RTM)
├── 02_PetPal_BRD.pdf                   # Business Requirements Document (As-Is/To-Be, CR-01 Scope Analysis)
├── 03_PetPal_UML_Package.pdf           # UML Diagrams (Use Case, Class, Sequence, Activity, State, Deployment)
├── 04_PetPal_Project_Plan.pdf          # WBS, Activity Network Diagram, CPM Schedule (85 Working Days)
├── 05_PetPal_Estimation_Budget.pdf     # Effort Breakdown (87 Person-Days, ₹2,61,000 / ₹8,00,000 Budget Cap)
├── 06_PetPal_Test_Plan_Evidence.pdf    # IEEE 829 Master Test Plan, TC-01..16, Boundary Value Analysis (BVA)
├── 07_PetPal_Risk_Register_Closure.pdf # 7-Point Risk Register, 5x5 RMMM Matrix, Project Closure Note
├── 08_PetPal_UI_UX_Screens.pdf         # Complete 26-Screen UI Specification & Interface Flows
├── PETPAL_COMPLETE_PROJECT_REPORT.pdf  # 33-Page Master Consolidated Project Report
├── PROJECT_REPORT.md                   # Comprehensive Markdown Engineering Report with embedded screenshots
├── README.md                           # Repository Documentation & Overview
└── screenshots/                        # Extracted High-Resolution Visual Artifacts
    ├── estimation_budget/              # Cost estimation & budget schedule graphics
    ├── project_planning/               # WBS, CPM network diagrams, and Gantt charts
    ├── risk_closure/                   # Risk matrices & RMMM plans
    ├── test_evidence/                  # IEEE 829 test cases & BVA tables
    ├── ui_screens/                     # 13 High-Fidelity UI Prototype Sheets (Screens 01-26)
    └── uml_diagrams/                   # Structural & Behavioral UML Diagrams
```

---

## 🌟 Key Functional Highlights

1. **24/7 Self-Service Web Booking (`FR-01` to `FR-06`)**:
   - 5-branch multi-tenant architecture across Bengaluru.
   - Doctor selection with specialization filtering and consultation fees.
   - Real-time 30-minute slot matrix backed by an **atomic 10-minute temporary reservation lock**.
2. **Clinical Immunization Recall Engine (`FR-09`, `FR-10`)**:
   - Automated background scheduler running daily at 00:01 AM.
   - Computes vaccination status badges: `UP TO DATE`, `DUE SOON (≤30d)`, `OVERDUE`.
   - Dispatches multi-channel SMS and Email reminders at **T-14d, T-3d, and T-1d**.
3. **Strict EMR Separation of Concerns (`FR-12`)**:
   - Mandated by Chief Veterinarian: **Zero `ClinicalNote` entity inside PetPal**.
   - Clinical notes, diagnostic imaging, and surgery logs reside exclusively in the legacy EMR.
   - PetPal manages appointment scheduling and pet demographics via stateless REST APIs.
4. **Scope Creep Governance (`CR-01`)**:
   - Rigorous evaluation of the Managing Partner's E-Commerce Pet-Food Store request (+55 person-days, +₹1,65,000).
   - Formally **Deferred to Phase 2** to safeguard the 85-day critical path and core clinical reliability.

---

## 📊 Summary of Engineering Metrics

| Engineering Dimension | Metric / Specification | Status |
| :--- | :--- | :---: |
| **Total Functional Requirements** | 12 Requirements (FR-01 to FR-12) | ✅ 100% Traceable |
| **Non-Functional Requirements** | 8 Requirements (Latency $\le$ 3.0s, Uptime $\ge$ 99.0%, DPDP RBAC) | ✅ Verified |
| **Critical Path Duration** | 85 Working Days ($A \rightarrow B \rightarrow C \rightarrow D \rightarrow E \rightarrow H \rightarrow K \rightarrow L \rightarrow M \rightarrow N$) | ✅ Optimized |
| **Total Baseline Effort** | 87 Person-Days | ✅ Budgeted |
| **Total Baseline Cost** | ₹2,61,000 (Utilization: 32.6% of ₹8,00,000 cap) | ✅ Within Budget |
| **Test Cases Planned** | 16 Functional Test Cases + 7-Point BVA + Decision Tables | ✅ PASS |
| **Risk Management** | 7 Key Risks with RMMM & Contingency fallbacks | ✅ Mitigated |

---

## 👨‍💻 Author & Evaluation Note
- **Author**: **Bankim Chandra Kamila**
- **Roll Number**: `150096724084`
- **Department**: Computer Science & Engineering, ITM Skills University
- **Course**: Software Engineering & Project Management (SEPM)
- **Academic Year**: 2024–2028

---
*For in-depth analysis, architectural formulas, and complete data tables, please refer to [**`PROJECT_REPORT.md`**](PROJECT_REPORT.md).*

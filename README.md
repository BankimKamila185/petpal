# 🐾 PawCare / PetPal: Intelligent Pet Care & Multi-Branch Clinic Platform
### Software Engineering & Project Management (SEPM) — Case Study No. 64

[![Flutter](https://img.shields.io/badge/Flutter-3.47.0-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.13.0-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Material 3](https://img.shields.io/badge/Design-Material%203-7C4DFF?style=for-the-badge)](https://m3.material.io/)
[![Unit & Widget Tests](https://img.shields.io/badge/Tests-15%2F15%20Passed%20(100%25)-brightgreen?style=for-the-badge&logo=checkmarx)](test/pawcare_test.dart)
[![ITM Skills University](https://img.shields.io/badge/ITM%20Skills%20University-CSE%20Department-orange.svg?style=for-the-badge)](https://itm.edu)
[![Candidate](https://img.shields.io/badge/Author-Bankim%20Chandra%20Kamila-success.svg?style=for-the-badge)](#)

---

## 📌 Project Overview
**PawCare / PetPal** is a cross-platform pet care and veterinary management ecosystem designed for a chain of 5 multi-branch veterinary clinics across Bengaluru (*Indiranagar, Koramangala, Whitefield, Jayanagar, and Bellandur*).

The application solves key veterinary practice bottlenecks (overwhelmed telephone lines, 22% call abandonment rate, 18% appointment no-shows, and lost paper vaccination records) through a centralized 24/7 web & mobile booking portal, real-time 30-minute slot matrix with a 10-minute temporary checkout hold, automated immunization recall engine, and live receptionist queue console.

---

## 📸 In-App Live Mobile Showcase

| 01. Welcome Screen | 02. Login Screen | 03. Signup Screen |
| :---: | :---: | :---: |
| ![Welcome](screenshots/01_welcome_screen.png) | ![Login](screenshots/02_login_screen.png) | ![Signup](screenshots/03_signup_screen.png) |

| 04. Discovery & Adoption | 05. Pet Bio & Adoption | 06. Vaccination Tracker |
| :---: | :---: | :---: |
| ![Discovery](screenshots/04_home_discovery.png) | ![Pet Bio](screenshots/05_pet_details_adoption.png) | ![Vaccine Tracker](screenshots/06_vaccination_tracker.png) |

| 07. Clinical Timeline | 08. Vaccine Intake Form | 09. My Pets Registry |
| :---: | :---: | :---: |
| ![Timeline](screenshots/07_pet_vaccine_timeline.png) | ![Add Vaccine](screenshots/08_add_vaccination_form.png) | ![My Pets](screenshots/09_my_pets.png) |

| 10. Shelter Intake Portal | 11. Clinical Reminders Hub |
| :---: | :---: |
| ![Shelter Intake](screenshots/10_shelter_add_pet.png) | ![Reminders](screenshots/11_vaccine_reminders.png) |

---

## 📐 System Architecture & Engineering Models

| UML Domain & Use Case Diagram | Dynamic Sequence, Activity & State Models |
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
├── 05_PetPal_Estimation_Budget.pdf     # Effort Breakdown (87 Person-Days, ₹2,61,00,000 / ₹8,00,000 Budget Cap)
├── 06_PetPal_Test_Plan_Evidence.pdf    # IEEE 829 Master Test Plan, TC-01..16, Boundary Value Analysis (BVA)
├── 07_PetPal_Risk_Register_Closure.pdf # 7-Point Risk Register, 5x5 RMMM Matrix, Project Closure Note
├── 08_PetPal_UI_UX_Screens.pdf         # Complete 26-Screen UI Specification & Interface Flows
├── PETPAL_COMPLETE_PROJECT_REPORT.pdf  # 33-Page Master Consolidated Project Report
├── PROJECT_REPORT.md                   # Comprehensive Markdown Engineering Report with embedded screenshots
├── README.md                           # Master Project Documentation & Overview
├── lib/                                # Flutter Cross-Platform Production Source Code
│   ├── models/                         # Domain entities (Pet, User, Vaccination, Enums)
│   ├── screens/                        # UI screens (Home, Adoption, Vaccines, Reminders, Shelter, Profile)
│   ├── services/                       # Local storage & Auth services
│   ├── theme/                          # Material 3 Pet Wellness theme tokens
│   └── utils/                          # Dynamic date arithmetic & aggregation algorithms
├── screenshots/                        # Extracted High-Resolution Visual Artifacts
│   ├── 01_welcome_screen.png           # In-app screenshot
│   ├── 04_home_discovery.png           # In-app screenshot
│   ├── 06_vaccination_tracker.png      # In-app screenshot
│   ├── estimation_budget/              # Cost estimation & budget schedule graphics
│   ├── project_planning/               # WBS, CPM network diagrams, and Gantt charts
│   ├── risk_closure/                   # Risk matrices & RMMM plans
│   ├── test_evidence/                  # IEEE 829 test cases & BVA tables
│   ├── ui_screens/                     # 13 High-Fidelity UI Prototype Sheets (Screens 01-26)
│   └── uml_diagrams/                   # Structural & Behavioral UML Diagrams
└── test/                               # Comprehensive Automated Test Suite (15/15 passing)
    ├── pawcare_test.dart               # Core logic, date arithmetic & widget tests
    └── widget_test.dart                # Application smoke test
```

---

## 🌟 Key Features & Engineering Highlights

1. **24/7 Self-Service Web & Mobile Booking (`FR-01` to `FR-06`)**:
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

## 🧪 Testing & Execution

```bash
# 1. Install dependencies
flutter pub get

# 2. Run automated test suite
flutter test

# 3. Launch Flutter Web Application
flutter run -d chrome
```

---

## 👨‍💻 Author & Academic Attribution
- **Author**: **Bankim Chandra Kamila**
- **Roll Number**: `150096724084`
- **Department**: Computer Science & Engineering, ITM Skills University
- **Course**: Software Engineering & Project Management (SEPM)
- **Academic Year**: 2024–2028

---
*For in-depth analysis, architectural formulas, and complete data tables, please refer to [**`PROJECT_REPORT.md`**](PROJECT_REPORT.md).*

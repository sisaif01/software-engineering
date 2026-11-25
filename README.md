# Smart Campus Management System

**Team F · Software Engineering Project · Dongshin University, Republic of Korea**

A web platform that brings key university services into one place. Students, faculty and administrators can manage course enrollment, attendance, room and laboratory booking, announcements and campus information.

> **Status (25 Nov 2026):** Planning and analysis complete · Design in progress · Implementation starts 30 Nov · Final demonstration **18 Dec 2026**

---

## Contents

1. [Project Overview](#1-project-overview)
2. [Team F](#2-team-f)
3. [Scope and Features](#3-scope-and-features)
4. [Users](#4-users)
5. [System Architecture](#5-system-architecture)
6. [System Diagrams](#6-system-diagrams)
7. [Database](#7-database)
8. [Project Documents](#8-project-documents)
9. [Timeline and Milestones](#9-timeline-and-milestones)
10. [Repository Structure](#10-repository-structure)
11. [Project Status](#11-project-status)
12. [Open Decisions](#12-open-decisions)

---

## 1. Project Overview

The Smart Campus Management System is a centralized digital platform designed to improve university campus operations and academic services.

**Problem:** Campus services such as attendance, enrollment, room booking and announcements are fragmented and largely manual. As a result, attendance is slow and easy to falsify, enrollments and bookings suffer from duplicates and conflicts, and users lack one reliable place to access campus information.

**Solution:** One secure web platform with role-based access, backed by a PostgreSQL database, that:

- verifies attendance with a time-limited **QR code** (and optionally location)
- prevents **duplicate enrollments** and **over-full courses**
- prevents **double booking** of rooms and laboratories
- delivers **announcements** to the right audience

## 2. Team F

| Member | Role | Main responsibilities |
|---|---|---|
| Shrestha Sujan | Project Manager | Schedule and task tracking, weekly meetings, final report and demo |
| Thapa Janak | Documentation Lead | Planning and analysis documents, README, design diagrams, user guide |
| Islam Md Saiful | Backend Lead | Repository owner, application server and REST API, authentication, enrollment and booking logic, code review |
| Yadav Om | Frontend Lead | Web pages and role dashboards, forms, QR scanning screen |
| Rabby Omi Hasan | Database Lead | PostgreSQL schema, sample data, transactions |
| Dhungana Hemkanta | QA / Test Lead | Test cases, sprint testing, bug tracking |

Detailed task ownership is in the [RACI Matrix](Our%20Project/Planning/RACI_Matrix.xlsx/RACI_Matrix.xlsx).

## 3. Scope and Features

The SRS defines **62 functional requirements** across 11 modules. The project is delivered in phases:

| Phase | Modules | Delivery |
|---|---|---|
| **Core** (must have) | User authentication and role-based access · Student and faculty management · Course management and enrollment · Attendance with QR verification · Room and laboratory booking | Required for the final demo |
| **Extended** (should have) | Announcements · Campus map and facility information · Admissions | If time allows after core is tested |
| **Future** (could have) | Smart library · Practical examination · IoT device management · University single sign-on (SSO) | Documented as future work |

**Out of scope:** payment processing, biometric attendance, real-time indoor navigation, replacing existing university systems.

Full requirement list with IDs (e.g. `REQ-ATT-03`): [Requirements document](Our%20Project/Analysis/Requirements/Requirements.docx).

## 4. Users

| User | Can |
|---|---|
| **Student** | Sign in, view and enroll in courses, mark attendance by QR code, view own attendance, book rooms and labs, read announcements, view campus map |
| **Faculty** | Open attendance sessions, manage attendance for assigned courses, view attendance reports, post announcements, book rooms and labs |
| **Administrator** | Manage user accounts, courses, rooms, campus map data, bookings, announcements, admissions and devices |
| **IoT device** | Send status updates for a registered facility (future module) |

## 5. System Architecture

The system follows a **five-layer architecture**:

| # | Layer | Responsibility |
|---|---|---|
| 1 | **User Interface** | Web screens, dashboards, forms and reports for each role |
| 2 | **User Communication** | Receives requests, validates input, routes to services, returns responses |
| 3 | **Application / Business Logic** | Student, course, enrollment and attendance management, authentication and role access, business rules |
| 4 | **Information Retrieval / Data Access** | Database queries, insert, update, delete, read, transaction handling |
| 5 | **System Database** | PostgreSQL storage of all system records |

### Technology

| Component | Choice | Status |
|---|---|---|
| Database | PostgreSQL | ✅ Decided |
| Architecture | Five-layer web application, REST API (JSON over HTTPS) | ✅ Decided |
| Front-end framework | — | ⏳ To be decided by 29 Nov |
| Back-end framework | — | ⏳ To be decided by 29 Nov |
| Version control | GitHub (this repository) | ✅ In use |

## 6. System Diagrams

All diagrams are in [`Our Project/Design`](Our%20Project/Design).

### Use Case Diagram

![Use Case Diagram](Our%20Project/Design/Use-case%20Diagram.png)

**Actors:** Student, Faculty, Admin, IoT Devices.
**Main use cases:** Login, Mark Attendance, Verify QR Code / Location, View Campus Map, Book Room / Laboratory, Check Availability, Manage Attendance, Post Announcement, Manage Users, Facilities and IoT Devices.
**Relationships:** *Mark Attendance* includes *Verify QR Code / Location*; *Book Room / Laboratory* includes *Check Availability*.

> The diagram shows login through single sign-on. University SSO is not yet confirmed; local accounts are the fallback (see [Open Decisions](#12-open-decisions)).

### Sequence Diagram

![Sequence Diagram](Our%20Project/Design/Sequence_Diagram.png)

Interactions between users, the Smart Campus Interface, the Application Controller and the PostgreSQL database for four processes:

- Student Registration
- Course Enrollment, including the *successful enrollment* and *already enrolled* alternatives
- Attendance Management
- View Attendance

### System Architecture Diagram

![System Architecture Diagram](Our%20Project/Design/System%20Architecture%20Diagram.png)

The five layers described in [Section 5](#5-system-architecture).

### Context Diagram

[Context Diagram (PDF)](Our%20Project/Design/Context_Diagram): the system boundary and its external entities.

## 7. Database

**DBMS:** PostgreSQL

Core entities from the SRS conceptual model:

| Entity | Purpose | Key relationships |
|---|---|---|
| User | Login identity and role | Parent of Student, Faculty and announcement author |
| Student | Student academic identity | References User; linked to Enrollment |
| Faculty | Faculty identity | References User; assigned to Course |
| Course | Course catalog and offering | Assigned Faculty; linked to Enrollment |
| Enrollment | A student's course registration | References Student and Course; parent of Attendance |
| Attendance | Attendance record | References Enrollment and the recording User |
| Room | Campus room or laboratory | Parent of Booking and devices |
| Booking | Room or lab reservation | References Room and requesting User |
| Announcement | Published campus notice | References author User |
| Facility_Device | Registered IoT or facility device | May reference Room |

**Key rules:** no duplicate active enrollment per student, course and term · no duplicate attendance per student and session · no overlapping confirmed bookings for the same room · enrollment, attendance and booking writes use transactions.

## 8. Project Documents

| Document | Description | Location |
|---|---|---|
| Problem Statement | Current problems, objectives, scope, success criteria | [Problem_Statement.docx](Our%20Project/Analysis/Problem_Statement/Problem_Statement.docx) |
| Feasibility Study | Technical, operational, economic, schedule and legal feasibility, risks | [Feasibility_Study.docx](Our%20Project/Analysis/Feasibility_Study/Feasibility_Study.docx) |
| Requirements | 62 functional and 16 non-functional requirements by module and phase | [Requirements.docx](Our%20Project/Analysis/Requirements/Requirements.docx) |
| SRS v1.0.0 | Full Software Requirements Specification with analysis models | [SRS.docx](Our%20Project/Analysis/SRS/SRS.docx) |
| Requirements Traceability Matrix | Links goals, requirements and use cases | [Smart_Campus_RTM (2).xlsx](Our%20Project/Analysis/Smart_Campus_RTM%20(2).xlsx) |
| Project Plan | Team, schedule, milestones, deliverables, working method | [Project_Plan.docx](Our%20Project/Planning/Project_Plan/Project_Plan.docx) |
| RACI Matrix | Who is Responsible, Accountable, Consulted, Informed for 27 tasks | [RACI_Matrix.xlsx](Our%20Project/Planning/RACI_Matrix.xlsx/RACI_Matrix.xlsx) |

## 9. Timeline and Milestones

| Phase | Dates | Main work |
|---|---|---|
| Planning and analysis | until 24 Nov | Problem statement, feasibility study, requirements, SRS, RTM ✅ |
| Design | 25 – 29 Nov | Frameworks, database schema (ERD), API list, wireframes |
| Sprint 1 | 30 Nov – 6 Dec | Login and roles, user records, courses and enrollment |
| Sprint 2 | 7 – 13 Dec | QR attendance, room and lab booking, dashboards |
| Testing | 14 – 16 Dec | Test cases, bug fixing, security and performance checks |
| Delivery | 17 – 18 Dec | Final report, user guide, slides, **final demo 18 Dec** |

| Milestone | Date |
|---|---|
| M1 Analysis documents complete | 24 Nov ✅ |
| M2 Design approved | 29 Nov |
| M3 Sprint 1 demo | 6 Dec |
| M4 Sprint 2 demo | 13 Dec |
| M5 Testing complete | 16 Dec |
| M6 Final demonstration | 18 Dec |

Weekly team meeting every Monday.

## 10. Repository Structure

```
software-engineering/
├── README.md
└── Our Project/
    ├── Analysis/
    │   ├── Problem_Statement/Problem_Statement.docx
    │   ├── Feasibility_Study/Feasibility_Study.docx
    │   ├── Requirements/Requirements.docx
    │   ├── SRS/SRS.docx
    │   └── Smart_Campus_RTM (2).xlsx
    ├── Design/
    │   ├── Use-case Diagram.png
    │   ├── Sequence_Diagram.png
    │   ├── System Architecture Diagram.png
    │   └── Context_Diagram
    ├── Planning/
    │   ├── Project_Plan/Project_Plan.docx
    │   ├── RACI_Matrix.xlsx/RACI_Matrix.xlsx
    │   ├── Gantt_Chart.png/
    │   └── Meeeting_Notes/
    └── Implementation/          (source code – from 30 Nov)
```

## 11. Project Status

- [x] Problem statement
- [x] Feasibility study
- [x] Requirements and SRS
- [x] Requirements traceability matrix
- [x] Project plan and RACI matrix
- [x] Use case, sequence, architecture and context diagrams
- [ ] Gantt chart uploaded
- [ ] Database schema (ERD)
- [ ] API list
- [ ] Screen wireframes
- [ ] Sprint 1 – login, records, enrollment
- [ ] Sprint 2 – QR attendance, room booking
- [ ] Test cases and testing
- [ ] User guide and final report
- [ ] Final demonstration

### Getting Started

Source code is not available yet. Setup and run instructions will be added here once implementation begins (Sprint 1, from 30 Nov).

## 12. Open Decisions

1. Front-end and back-end frameworks (by 29 Nov).
2. Attendance verification: QR only, or QR plus location.
3. Room booking: confirmed automatically or after administrator approval.
4. Login method: local accounts or university single sign-on.
5. Whether smart library and practical examination are built in this project or left as future work.

---

*Team F · Dongshin University · 2026*

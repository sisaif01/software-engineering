# software-engineering
About SRS
# Smart Campus Management System

## 1. Project Overview

The Smart Campus Management System is a centralized digital platform designed to improve university campus operations and academic services.

The system provides functionalities for students, faculty members, and administrators to manage academic activities, attendance, campus facilities, room reservations, announcements, and IoT-based services.

## 2. System Diagrams

This repository includes three important diagrams that describe the system's functionality, interactions, and technical architecture.

### 2.1 Sequence Diagram

**File:** `Diagrams/Sequence_Diagram.png`

The Sequence Diagram illustrates the interactions between system users, the Smart Campus Interface, Application Controller, and PostgreSQL Database.

It demonstrates four major processes:

- Student Registration
- Course Enrollment
- Attendance Management
- View Attendance

The diagram also demonstrates alternative enrollment conditions, including successful enrollment and already-enrolled scenarios.

### 2.2 Use Case Diagram

**File:** `Diagrams/Use_Case_Diagram.png`

The Use Case Diagram illustrates the functional requirements of the Smart Campus Management System and the interactions between its main actors.

**Actors:**
- Student
- Faculty
- Admin
- IoT Devices

**Main functionalities:**
- Login (Single Sign-On)
- Mark Attendance
- Verify QR Code / Location
- View Campus Map
- Book Room / Laboratory
- Check Availability
- Manage Attendance
- Post Announcement
- Manage Users, Facilities, and IoT Devices

The diagram also illustrates the `<<include>>` relationships between attendance verification and room availability checking.

### 2.3 System Architecture Diagram

**File:** `Diagrams/System_Architecture.png`

The System Architecture Diagram presents the layered architecture of the Smart Campus Management System.

The architecture consists of five layers:

1. **User Interface Layer:** Provides user-facing screens, dashboards, forms, and reports.
2. **User Communication Layer:** Handles requests, input validation, and communication between components.
3. **Application / Business Logic Layer:** Manages student information, courses, enrollment, attendance, authentication, and business rules.
4. **Information Retrieval / Data Access Layer:** Handles database operations, including insertion, updating, deletion, and data retrieval.
5. **System Database:** Uses PostgreSQL to store student, course, enrollment, and attendance information.

## 3. Main System Features

- User Authentication
- Student Management
- Course Management
- Course Enrollment
- Attendance Management
- QR Code / Location Verification
- Campus Map
- Room and Laboratory Booking
- Facility Management
- Announcements
- IoT Integration

## 4. Database

**Database Management System:** PostgreSQL

Main data categories:
- Students
- Courses
- Enrollments
- Attendance Records

## 5. Repository Structure

```text
Smart-Campus-Management-System/
│
├── README.md
├── Documentation/
├── Diagrams/
│   ├── Sequence_Diagram.png
│   ├── Use_Case_Diagram.png
│   └── System_Architecture.png
├── Source_Code/
└── Screenshots/
```

## 6. Project Purpose

The purpose of this project is to develop an integrated campus management solution that simplifies academic processes, improves administrative efficiency, and provides convenient digital services for university students and faculty members.

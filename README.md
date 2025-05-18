# University Enrollment System

A database-driven application built with **Python** and **SQLite3** to manage students, professors, courses, departments, and course enrollments in a university setting.

---

## Overview

This system handles:
- Department and professor management
- Course creation and assignment
- Student management
- Enrollment with unit and capacity constraints
- Relationship enforcement via foreign keys and constraints

---

## System Architecture

- **Backend:** SQLite3
- **Language:** Python
- **Database Structure:** 5 core tables
- **Design:** Modular/OOP-ready

---

## Database Schema

### Departments Table
- `department_id` (INTEGER, PK)
- `department_name` (TEXT, UNIQUE)

Relationship:  
One department can have many professors and courses (1:N)

---

### Professors Table
- `professor_id` (INTEGER, PK)
- `first_name` (TEXT)
- `last_name` (TEXT)
- `email` (TEXT, UNIQUE)
- `department_id` (INTEGER, FK → departments.id)

Relationship:  
- Each professor belongs to one department (N:1)  
- One professor can teach many courses (1:N)

---

### Courses Table
- `course_id` (INTEGER, PK)
- `course_name` (TEXT)
- `professor_id` (INTEGER, FK → professors.id)
- `department_id` (INTEGER, FK → departments.id)
- `units` (INTEGER)
- `capacity` (INTEGER, DEFAULT 30)
- `day` (TEXT)
- `start_time` (TEXT)
- `end_time` (TEXT)

Relationship:  
- Each course belongs to one department (N:1)  
- Each course is taught by one professor (N:1)  
- One course can have many enrolled students via `enrollments` (1:N)

---

### Students Table
- `student_id` (INTEGER, PK)
- `first_name` (TEXT)
- `last_name` (TEXT)
- `email` (TEXT)
- `date_of_birth` (DATE)

Relationship:  
- One student can enroll in many courses via `enrollments` (1:N)

---

### Enrollments Table (Junction Table)
- `enrollment_id` (INTEGER, PK)
- `student_id` (INTEGER, FK → students.id)
- `course_id` (INTEGER, FK → courses.id)
- `date_of_birth` (DATE)

Relationship:
- Many-to-Many (M:N) between students and courses  
- Each combination of student and course is unique (`UNIQUE(student_id, course_id)`)

---

## Relationship Summary

| Entity A         | Relationship | Entity B         |
|------------------|--------------|------------------|
| Department       | 1 → N        | Professors       |
| Department       | 1 → N        | Courses          |
| Professor        | 1 → N        | Courses          |
| Student          | M → N        | Courses          |
| Courses          | M → N        | Students         |
| Students/Courses | M ↔ N        | Enrollments Table|

---

## Features

- Add/update/delete:
  - Students
  - Professors
  - Departments
  - Courses
- Course assignment to professors and departments
- Student enrollment with:
  - Unit validation (e.g., max 18 units)
  - Course capacity checks
- Duplicate enrollment prevention
- Full relational data views (e.g., course + prof + department)

---
## Extended Features

7. **View Course Roster**  
   Allows administrators or professors to view a list of students enrolled in a specific course. Useful for taking attendance, tracking class size, or contacting enrollees.

8. **View Student Timetable**  
   Displays the list of courses a student is enrolled in, along with their units and schedules (if schedules are implemented). Helps students manage their workload and avoid conflicts.

9. **Department Level Summary**  
   Generates a summary of courses and professors under each department, including total courses offered and enrolled student count per course. Provides insights at the academic unit level.

10. **Student Course Enrollment**  
   Enables manual or self-service enrollment of students in available courses while enforcing unit and capacity rules. This is one of the system’s core features.

11. **CSV Report Generator**  
   Allows exporting student lists, course rosters, or department summaries into CSV format for academic reporting, administrative auditing, or external use.

---

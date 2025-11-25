Database Design Lab: Relational Modeling and Normalization

Objective

Understand and apply the core principles of relational database design, including defining keys, establishing relationships, and achieving database normalization (up to 3NF) to ensure data integrity and minimize redundancy.

Concepts Covered

This lab focuses on the theoretical groundwork of structured data:

Fundamental Components: Tables, records (rows), and attributes (columns).

Key Constraints: Primary Keys (unique identification) and Foreign Keys (linking tables).

Relationships: Modeling 1:1, 1:M (One-to-Many), and M:M (Many-to-Many) connections.

Normalization: The process of organizing data to meet two basic requirements: reducing redundancy and ensuring data dependencies make sense (Data Normalization Forms: 1NF, 2NF, 3NF).

Scenario: Designing a University Course Management System

Design an Entity-Relationship Diagram (ERD) and define the schema for a system that needs to track:

Students: ID, Name, Major.

Courses: Code, Title, Credits.

Instructors: ID, Name, Department.

Enrollment: Students can enroll in multiple courses, and courses have multiple students.

Teaching Assignments: Each course section is taught by one Instructor.

Sample Schema Design Notes

Required Tables and Keys

Table Name

Primary Key (PK)

Foreign Key(s) (FK)

Students

student_id

major_id (FK to Majors table, if modeled)

Courses

course_code

-

Instructors

instructor_id

department_id (FK to Departments table)

Enrollments (Junction Table)

(student_id, course_code) (Composite PK)

student_id (FK to Students), course_code (FK to Courses)

Key Design Principle Applied

The many-to-many (M:M) relationship between Students and Courses is resolved by creating the Enrollments junction table. This table uses a composite primary key derived from the foreign keys of the two tables it connects.

Reflection

Database design is about reducing duplication (the core goal of normalization) and organizing data to accurately reflect the complexity and relationships of the real world. The most challenging aspect is correctly identifying and resolving M:M relationships, as this often dictates the structure and complexity of the resulting SQL queries. Tools like \href{https://dbdiagram.io}{dbdiagram.io} or Lucidchart are invaluable for visualizing these relationships before writing any code.

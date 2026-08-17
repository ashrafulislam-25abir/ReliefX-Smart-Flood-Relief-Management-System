# ReliefX – Smart Flood Relief Management System

> **A Smart Database Management System for Efficient, Transparent, and Fair Flood Relief Operations**

## 📌 Overview

**ReliefX – Smart Flood Relief Management System** is a database-driven platform developed as a **Database Management System (DBMS) Lab Course Project**.

The primary goal of ReliefX is to improve the management, coordination, and monitoring of flood-relief activities involving multiple NGOs. The system provides a centralized environment for managing **beneficiaries, NGOs, relief items, stock, distribution points, and relief distribution records**.

Beyond basic data management, ReliefX focuses on solving practical challenges in relief operations, including **duplicate distribution, beneficiary privacy, resource management, data integrity, transparency, and area-wise fairness**.

The system demonstrates how database-level mechanisms can be used to enforce business rules and maintain reliable, consistent, and secure relief-management data.

---

## 🎯 Objectives

The major objectives of ReliefX are to:

* Centralize flood-relief information in a structured database.
* Efficiently manage beneficiaries, NGOs, relief items, and distribution points.
* Maintain accurate stock and distribution records.
* Prevent repeated distribution of the same relief category within a defined period.
* Protect sensitive beneficiary identity information.
* Maintain an audit trail for restricted or blocked distribution attempts.
* Provide area-wise insights for evaluating relief distribution fairness.
* Ensure data consistency, integrity, and transparency throughout the system.

---

## ✨ Core Features

### 👥 Beneficiary Management

ReliefX maintains structured records of beneficiaries and their associated relief information. The system is designed to handle beneficiary data while protecting sensitive identity information.

### 🏢 NGO Management

The platform allows information related to participating NGOs to be organized and maintained centrally, supporting coordinated relief operations involving multiple organizations.

### 📦 Relief Item & Stock Management

The system manages relief item categories and stock information, allowing available resources to be monitored and associated with distribution activities.

### 📍 Distribution Point Management

Relief distribution points are maintained with their relevant area information, allowing distributions to be properly organized and analyzed geographically.

### 🚚 Relief Distribution Management

ReliefX records distribution activities, including beneficiary-related distribution information and the relief categories provided. This creates a structured history of relief operations.

### 🛡️ Database-Level Duplicate Prevention

One of the key features of ReliefX is its **database-level duplicate prevention mechanism**.

The system prevents a family from receiving the **same category of relief within seven days**.

This rule is enforced through database-level logic using:

* Stored Procedures
* Triggers
* Validation Rules
* Audit Records

This approach ensures that the restriction is enforced directly at the database level rather than depending only on the application interface.

### 🔐 Beneficiary Identity Protection

Sensitive beneficiary identity information is protected through **salted cryptographic hashing**, reducing the risk of exposing raw identity data within the database.

### 📝 Audit Trail

When a duplicate relief attempt is blocked, the system maintains an audit record of the attempt. This supports transparency, monitoring, and future investigation of distribution activities.

### 📊 Area-wise Fairness Analysis

ReliefX provides an **area-wise fairness view** that helps analyze how relief has been distributed across different areas. This can support better understanding of distribution patterns and resource allocation.

---

## 🗄️ DBMS Concepts Implemented

The project demonstrates practical implementation of several fundamental and advanced database concepts:

* Relational Database Design
* Entity-Relationship Modeling
* Primary Keys
* Foreign Keys
* Unique Constraints
* NOT NULL Constraints
* CHECK Constraints
* Referential Integrity
* CRUD Operations
* SQL Queries
* Table Relationships
* Joins
* Aggregate Functions
* Stored Procedures
* Database Triggers
* Views
* Data Validation
* Audit Logging
* Data Security
* Transaction-based Database Operations

---

## 🏗️ System Workflow

The general workflow of ReliefX can be summarized as:

```text
Beneficiary Registration
          │
          ▼
   Beneficiary Records
          │
          ▼
Relief & Stock Management
          │
          ▼
Distribution Point Selection
          │
          ▼
  Relief Distribution
          │
          ▼
Duplicate Validation
          │
     ┌────┴────┐
     │         │
  Allowed    Blocked
     │         │
     ▼         ▼
Distribution  Audit Record
     │
     ▼
Distribution History
     │
     ▼
Area-wise Fairness Analysis
```

The workflow combines application operations with database-level validation to ensure that relief distribution rules are consistently enforced.

---

## 🔒 Data Integrity & Security

Data reliability and security are important aspects of ReliefX.

The system uses database constraints and relationships to maintain **data integrity and consistency**. Primary and foreign keys establish valid relationships between entities, while validation constraints help prevent invalid data.

For sensitive beneficiary information, **salted cryptographic hashing** is used to provide an additional layer of privacy.

The use of **stored procedures and triggers** ensures that important business rules, particularly duplicate relief prevention, are enforced at the database level.

---

## 🧪 Quality Assurance

Quality assurance was performed throughout the development process to verify the correctness and reliability of the system.

Testing focused on:

* Database operations and CRUD functionality
* Data validation
* Primary and foreign key relationships
* Constraint enforcement
* Stored procedure execution
* Trigger behavior
* Duplicate relief prevention
* Audit record generation
* Distribution record consistency
* Area-wise fairness analysis
* Invalid and restricted operations

The testing process helped identify potential defects and verify that the implemented database rules behaved as expected.

---

## 🛠️ Technology Stack

* **Database:** MySQL
* **Query Language:** SQL
* **Frontend:** HTML, CSS
* **Version Control:** Git & GitHub

---

## ⚙️ Database Setup

To run the database component locally:

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/ReliefX-Smart-Flood-Relief-Management-System.git
```

### 2. Open the Project

```bash
cd ReliefX-Smart-Flood-Relief-Management-System
```

### 3. Create the Database

Open MySQL or MySQL Workbench and create the project database:

```sql
CREATE DATABASE relief_db;
```

### 4. Import the SQL Implementation

Execute the SQL files provided in the repository according to their intended order.

The database implementation contains the required:

* Tables
* Relationships
* Constraints
* Sample Data
* Stored Procedures
* Triggers
* Views
* Queries

### 5. Run the Application

Follow the project files and configuration included in the repository to run the application locally.

---

## 📁 Repository Contents

The repository contains the project implementation and supporting materials, including:

* Database schema and table definitions
* SQL implementation
* Stored procedures
* Database triggers
* Views
* SQL queries
* Application source code
* Project documentation
* Supporting project resources

---

## 👨‍💻 Team

### StratifyX

| Member                 | Contribution                                                                                     |
| ---------------------- | ------------------------------------------------------------------------------------------------ |
| **Gulam Sakaria**      | Project Development & Team Collaboration                                                         |
| **MD. Ashraful Islam** | Requirement Analysis, Proposal, Database Implementation, Feature Development, QA & Documentation |
| **Mir Samiul Haque**   | Feature Development & Project Collaboration                                                      |
| **Nafil Ardul Ridin**  | ERD Design, Frontend Development & Database Concepts                                             |
| **Zidne Hasan**        | Bug Fixing, Documentation & Overall Review                                                       |

---

## 🎓 Academic Information

**Course:** Database Management System (DBMS) Lab
**Project:** ReliefX – Smart Flood Relief Management System
**Team:** StratifyX
**Institution:** Daffodil International University

---

## 🔗 Project Links

🌐Live Website: https://reliefx.stratifyxglobal.com/

---

## 📚 Learning Outcomes

Developing ReliefX provided practical experience in translating a real-world problem into a structured database solution.

Through this project, we gained hands-on experience in:

* Database design and implementation
* SQL query development
* Database constraints and relationships
* Stored procedures and triggers
* Data validation and integrity
* Database-level business rule enforcement
* Data privacy and security
* Quality assurance and testing
* Technical documentation
* Collaborative software development

---

## 🙏 Acknowledgement

This project was developed as part of the **Database Management System (DBMS) Lab Course** to demonstrate the practical application of database concepts in solving a real-world problem.

We are grateful to our course instructor for the guidance, feedback, and support provided throughout the project development process.

---

## 📄 License

This project was developed for **academic and educational purposes**.

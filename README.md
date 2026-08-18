ReliefX — Smart Flood Relief Distribution System

A database-driven flood relief management system designed to make relief distribution fairer, safer, and more accountable.

ReliefX is a full-stack web application developed to manage post-flood relief distribution through a centralized database system. The platform focuses on duplicate prevention, beneficiary identity protection, NGO stock management, concurrency control, audit logging, and population-based fairness analysis.

The system is built using HTML, CSS, JavaScript, PHP, and MySQL and can be deployed locally using XAMPP.

📌 Project Overview

During large-scale flood relief operations, multiple NGOs may distribute assistance to the same communities at the same time. Without a centralized mechanism, several problems can occur:

The same family may receive the same category of relief multiple times.
Some families may remain underserved.
Sensitive NID information may be exposed.
Two operators may process the same beneficiary simultaneously.
An NGO may attempt to distribute more items than it has in stock.
Bengali spelling variations may result in duplicate family registration.

ReliefX addresses these problems through database-level business rules, rather than depending only on frontend validation.

🎯 Main Objectives
Prevent duplicate relief distribution within a 7-day category-based period.
Protect beneficiary NID information using salted SHA-256 hashing.
Identify a family through either its head or registered member.
Prevent the same person from being registered across multiple families.
Handle concurrent relief requests safely using transactions and row locking.
Track NGO-wise relief inventory.
Maintain an audit trail of duplicate attempts and identity lookups.
Provide population-based fairness analysis.
Support Bengali-aware fuzzy name matching.
Implement role-based access for administrators and NGO operators.
🏗️ System Architecture

The system follows a simple layered architecture where the browser communicates with PHP application logic, and PHP interacts with the MySQL database.

<img width="1380" height="816" alt="image" src="https://github.com/user-attachments/assets/79ed487d-8526-427b-a40a-382b1541603a" />

How the architecture works

Frontend

The user interacts with pages such as:

Login
Family Registration
Relief Distribution
Dashboard
Stock Management
Duplicate Log

PHP Application Layer

PHP handles:

Authentication
Role verification
Form/API processing
CSRF validation
Input handling
Database communication

Database Layer

MySQL is responsible for the most important business rules:

Duplicate detection
Identity conflict prevention
Stock validation
Transaction management
Concurrency control
Audit logging

This means the system does not trust the frontend alone.

🔄 Overall System Workflow

<img width="1829" height="4351" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/ea2d3b34-84d4-4a36-a33c-484303e77106" />

👨‍👩‍👧 Family Registration Flow

Family registration has more than a simple INSERT.

Before creating a new family, the system checks whether the submitted identity already belongs to another registered person.

<img width="1331" height="2777" alt="mermaid-diagram (1)" src="https://github.com/user-attachments/assets/baad1381-3b8e-4e4a-b4f2-edbb12b3e712" />
Why v_person_registry is important

Normally, the family head is stored in family while other members are stored in family_member.

To make identity lookup easier, the system creates:

v_person_registry

Conceptually:

family
   │
   ├── Head Identity
   │
   └──────────────┐
                  │
                  ▼
          v_person_registry
                  ▲
                  │
   ┌──────────────┘
   │
family_member
   │
   └── Member Identity

So the application can ask:

"Does this identity belong to any registered person?"

instead of checking only one table.

🆔 Identity Resolution

A major improvement in Version 2.0 is that the system no longer requires the family head's identity only.

<img width="1055" height="1900" alt="mermaid-diagram (2)" src="https://github.com/user-attachments/assets/edf8f7d1-eadb-4e0e-9ab4-566223d3821e" />
Therefore:

Family Head NID
       ↓
     Family


Member NID
       ↓
     Family

Both paths eventually reach the same family_id.

🚚 Relief Distribution Flow

This is the core workflow of ReliefX.

<img width="2216" height="2562" alt="mermaid-diagram (3)" src="https://github.com/user-attachments/assets/ff5838a0-e982-48c5-81a0-21addc3878bd" />

🛑 Duplicate Prevention

The system does not rely only on JavaScript to stop duplicates.

Instead, duplicate prevention works in multiple layers.
<img width="971" height="2846" alt="mermaid-diagram (4)" src="https://github.com/user-attachments/assets/b5f2be9c-8389-4f76-a7e8-9d31ffd9c922" />
Defense-in-Depth

The protection can therefore be viewed as:

Frontend Warning
       ↓
Application Validation
       ↓
Stored Procedure
       ↓
Database Trigger
       ↓
Final Database Protection

Even if someone bypasses the frontend, the database-level rules remain active.

🔒 Concurrency / Race Condition Handling

This was one of the important Version 2.0 improvements.

Previous problem

Imagine two NGO operators submit relief for the same family almost simultaneously:

Operator A                     Operator B
    │                              │
    ├── Check recent = 0           │
    │                              ├── Check recent = 0
    │                              │
    ├── Insert distribution        ├── Insert distribution
    │                              │
    ▼                              ▼
          DUPLICATE OCCURS

Both requests could previously see no recent distribution.

Current solution

The procedure now locks the family row:
<img width="1524" height="1496" alt="mermaid-diagram (5)" src="https://github.com/user-attachments/assets/10907b50-7776-4636-a225-85e464d39768" />
So the second request cannot simply pass the duplicate check using stale state.

📦 NGO Stock Management

Every NGO has inventory for different relief items.
<img width="1231" height="1800" alt="mermaid-diagram (6)" src="https://github.com/user-attachments/assets/42cf7231-007e-40a3-8b21-4c6f6f6dbbf1" />
This prevents the system from approving a distribution when the NGO does not have enough inventory.

🧾 Audit & Duplicate Logging

ReliefX keeps track of important system activities.
<img width="1877" height="1120" alt="mermaid-diagram (7)" src="https://github.com/user-attachments/assets/63494cd5-9907-40b1-b9c0-2e187dea6243" />
This allows administrators to investigate both:

successful identity lookups
blocked duplicate attempts
👥 Role-Based Access

The application supports different capabilities depending on the logged-in user's role.
<img width="4121" height="832" alt="mermaid-diagram (8)" src="https://github.com/user-attachments/assets/597c3d6b-218f-4f48-8f6b-3f4f7e00014d" />
📊 Fairness Analysis

The dashboard uses the v_area_fairness view to compare relief distribution with population.

Conceptually:

Area Population
       +
Distributed Relief
       ↓
Fairness Calculation
       ↓
v_area_fairness
       ↓
Dashboard
       ├── Fairness Gauge
       ├── NGO Performance
       ├── Weekly Trend
       └── Relief Zone Map

The objective is not simply to count how many relief items were distributed, but to understand whether distribution is reasonably aligned with population needs.

🗺️ Relief Zone Map

The dashboard includes a Bangladesh relief-zone visualization using:

Leaflet.js
OpenStreetMap

The map displays selected relief areas and uses the fairness ratio to visually indicate their relative distribution status.
<img width="2825" height="130" alt="mermaid-diagram (9)" src="https://github.com/user-attachments/assets/c72bd86a-df8b-4741-b5c8-04c2407b1876" />
No separate map API key is required for the current implementation.

🧠 Bengali Fuzzy Matching

Bengali names can have spelling variations.

For example:

রহিম উদ্দিন
রহীম উদ্দীন

A simple exact string comparison may fail to recognize that these names are likely similar.

ReliefX therefore uses:

Database Candidate Filter
          ↓
Unicode-aware comparison
          ↓
mb_levenshtein()
          ↓
Possible Duplicate Warning

The custom mb_levenshtein() function works with Unicode characters rather than treating a Bengali UTF-8 string simply as a sequence of raw bytes.

🔐 Security Model

The project includes several security mechanisms:
<img width="1332" height="2615" alt="mermaid-diagram (10)" src="https://github.com/user-attachments/assets/097d402b-519f-47ff-9c19-b4db40968745" />
Security-related components include:

Password hashing
Session authentication
Role-based authorization
CSRF protection
Salted SHA-256 identity hashing
Prepared database queries
Database constraints
Transactions
Row locking
Protected .env
.htaccess protection
Installer removal after setup
🗄️ Database Components

The database contains the major entities required for relief management, including:

Geographic Information
        │
        ├── Division
        ├── District
        ├── Upazila
        └── Area


Beneficiary Information
        │
        ├── Family
        └── Family Member


Relief Management
        │
        ├── Relief Item
        ├── NGO Stock
        ├── Distribution Point
        └── Distribution


Monitoring
        │
        ├── Duplicate Log
        ├── Query Log
        └── Users
Important Views
v_person_registry
v_area_fairness
Important Triggers
trg_block_duplicate
trg_family_no_cross_dup
trg_family_member_no_cross_dup
Main Stored Procedure
sp_distribute_relief
📁 Project Structure
relief_system/
│
├── database.sql
├── config.php
├── .env.example
├── .gitignore
├── .htaccess
├── install.php
├── index.php
├── logout.php
├── distribute.php
├── register_family.php
├── dashboard.php
├── duplicate_log.php
├── stock.php
│
├── includes/
│   ├── auth.php
│   ├── env.php
│   ├── header.php
│   └── footer.php
│
├── api/
│   ├── check_duplicate.php
│   ├── fuzzy_check.php
│   ├── save_family.php
│   ├── save_distribution.php
│   ├── save_distribution_point.php
│   └── save_stock.php
│
├── css/
│   └── style.css
│
├── js/
│   └── app.js
│
└── img/
    ├── logo-full.jpeg
    └── logo-icon.png
⚙️ Technology Stack
Layer	Technology
Frontend	HTML5, CSS3, JavaScript
Backend	PHP
Database	MySQL
Server	Apache / XAMPP
DB Management	phpMyAdmin
Mapping	Leaflet.js + OpenStreetMap
Authentication	PHP Sessions
Security	CSRF + Salted SHA-256
DB Logic	Triggers + Stored Procedure + Views
Concurrency	Transactions + FOR UPDATE
🚀 Installation
1. Copy Project

Place the project inside XAMPP:

C:\xampp\htdocs\relief_system\
2. Start XAMPP

Start:

Apache
MySQL
3. Import Database

Open:

http://localhost/phpmyadmin

Then:

Import → database.sql → Go

4. Configure Environment

Create .env from .env.example.

Example:

DB_HOST=localhost
DB_NAME=relief_db
DB_USER=root
DB_PASS=
NID_SALT=your_private_random_salt
5. Install Demo Accounts

Open:

http://localhost/relief_system/install.php

After successful installation, delete install.php.

6. Run the Application
http://localhost/relief_system/
🧪 Viva Demonstration Checklist
Duplicate Prevention
Login as NGO Operator
        ↓
Enter Demo NID
        ↓
Select Relief Item
        ↓
Submit
        ↓
Success
        ↓
Submit same category again
        ↓
Blocked
        ↓
Show duplicate_log.php
Identity Conflict
Register existing member NID as new family head
        ↓
v_person_registry detects identity
        ↓
Error shown
        ↓
Registration rejected
Member NID Distribution
Enter Member NID
        ↓
v_person_registry
        ↓
Family resolved
        ↓
7-day duplicate check
        ↓
Distribution continues
Stock Enforcement
Reduce NGO stock
        ↓
Request larger quantity
        ↓
Insufficient stock
        ↓
Distribution rejected
Concurrency

Show in database.sql:

START TRANSACTION
        ↓
FOR UPDATE
        ↓
Duplicate Check
        ↓
Stock Check
        ↓
INSERT
        ↓
COMMIT / ROLLBACK
🐛 Major Bugs Fixed in Version 2.0
Bug #1 — Cross-table identity duplication

A person registered as a family member could previously register again as a family head.

Fix: v_person_registry + cross-table triggers + application-level validation.

Bug #2 — Distribution race condition

Two simultaneous requests could bypass the duplicate check.

Fix: Transaction + FOR UPDATE row locking.

Bug #3 — Hardcoded secrets

Database credentials and hashing configuration were directly stored in code.

Fix: .env + .env.example + .gitignore.

Bug #4 — Installer accounts could not log in

Freshly generated demo accounts were not marked as verified.

Fix: Installer explicitly sets is_verified = 1.

Bug #5 — API session expiration caused 404

API requests were receiving normal browser redirects.

Fix: API-aware authentication responses with JSON 401/403.

Bug #6 — Invalid demo NID hashes

The original seeded NID hashes were incomplete, causing demo identities to fail lookup.

Fix: Recomputed and replaced the demo SHA-256 hashes.

✨ UI & Frontend Improvements

The project also includes several frontend improvements:

Responsive dashboard layout
Branded ReliefX logo
Animated KPI counters
Animated fairness gauges
Card entrance animations
Button hover/active feedback
Gradient login background
Role-aware navigation
Relief-zone map
Dashboard charts
Icon-based visual indicators
📚 DBMS Guideline Coverage

The project demonstrates:

Requirement analysis
ER modeling
Relational database design
Primary and foreign keys
Constraints
Normalization / 3NF
CRUD operations
Filtering
Sorting
Grouping
Aggregation
Joins
Subqueries
Views
Triggers
Stored procedures
Transactions
Concurrency control
Audit logging
Investigation queries
Role-based access

Additional SQL examples and investigation queries are available in:

queries_for_report.sql
🔮 Future Improvements

Possible future extensions include:

Cloud deployment
Real-time NGO coordination
SMS notifications
Advanced beneficiary verification
More detailed geographic analytics
Automated demand prediction
Mobile application
Multi-language support
Production-grade secret management
Advanced reporting and export functionality
👨‍💻 Project Information

Project Name: ReliefX — Smart Flood Relief Distribution System

Domain: Flood Relief Management / Database Systems

Stack: PHP + MySQL + HTML + CSS + JavaScript

Environment: XAMPP / Localhost

Primary Focus: Duplicate Prevention, Identity Protection, Fair Distribution, Stock Management & Database Integrity

⚠️ Disclaimer

This is an academic/demo implementation developed to demonstrate database design, security concepts, transaction management, concurrency handling, and practical software engineering.

All sample beneficiary information, credentials, geographic records, and relief data are demonstration data only and should not be treated as real-world personal information.








    

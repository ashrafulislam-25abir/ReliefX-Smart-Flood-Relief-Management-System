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

flowchart TD
    A["User / NGO Operator / Admin"] --> B["Web Interface<br>HTML + CSS + JavaScript"]

    B --> C["PHP Application Layer"]

    C --> D["Authentication & Authorization"]
    C --> E["API Endpoints"]
    C --> F["Business Logic"]

    D --> G["Session + Role Checking"]
    E --> F

    F --> H["MySQL Database"]

    H --> I["Tables"]
    H --> J["Views"]
    H --> K["Triggers"]
    H --> L["Stored Procedure"]
    H --> M["Transactions + Row Locks"]

    C --> N["Leaflet + OpenStreetMap"]

    

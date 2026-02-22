# Peer-2-Peer-Lending-System

A full-stack database project that designs, implements, and analyzes a **Peer-to-Peer (P2P) Lending Platform** connecting Northeastern University students (borrowers) with alumni investors (lenders). The platform eliminates traditional banking intermediaries by enabling direct loan transactions tied to student startup ideas — built with MySQL, MongoDB, and Python analytics.

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [System Requirements](#system-requirements)
- [Database Design](#database-design)
  - [ER Diagram](#er-diagram)
  - [Schema Overview](#schema-overview)
- [Implementation](#implementation)
  - [SQL (MySQL)](#sql-mysql)
  - [NoSQL (MongoDB)](#nosql-mongodb)
  - [Python Analytics & Visualizations](#python-analytics--visualizations)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)

---

## Problem Statement

The traditional funding process for college students aspiring to launch startups — bank loans, scholarships, research grants — is cumbersome, time-consuming, and loaded with eligibility constraints. This project eliminates the middleman (bank) by creating a platform that connects **Northeastern students to alumni investors** through a P2P lending system.

Students (borrowers) showcase their startup ideas and request loans, while alumni (lenders) invest directly and earn returns. The platform provides a secure environment for loan transactions that benefits both parties: flexible financing for students and profitable investment opportunities for alumni, all powered by Northeastern's strong alumni network.

---

## System Requirements

The platform was designed around these business rules:

- A borrower (student) can submit **multiple startup ideas** to their profile
- A borrower can request **multiple loans** from different lenders
- A lender (alumni) can invest in **multiple ideas**
- One loan is tied to **one borrower** and **one lender**
- Each loan has **one repayment schedule**, but a borrower may have different schedules across their loans
- One loan can have **multiple transaction records**
- A loan can have different statuses (Approved, Pending, Active, Defaulted, etc.) that change over time
- The system supports **dispute management**, **notifications**, **feedback**, **ratings**, and **audit logs** for transparency
- A **referral program** allows users to recommend others to the platform

---

## Database Design

### ER Diagram

The Enhanced Entity-Relationship diagram models the complete P2P lending ecosystem with 16+ entities and their cardinality constraints:

<p align="center">
  <img src="images/er_diagram.png" alt="ER Diagram" width="850"/>
</p>

Key relationships include the User → Borrower/Lender specialization hierarchy, the central Loan Application entity connecting borrowers and lenders, and supporting entities for disputes, transactions, collateral, audit logs, and startup ideas.

### Schema Overview

The relational schema consists of **16 tables** with full referential integrity via foreign keys and cascading deletes/updates:

| Table | Description |
|---|---|
| `User` | Base user table (30 records) — lenders and borrowers inherit from here |
| `Lender` | Alumni who fund loans (IDs 16–30), linked to User via FK |
| `Borrower` | Students who request loans (IDs 1–15), linked to User via FK |
| `Loan_Application` | Core loan records linking borrowers, lenders, audit logs, history, and repayment plans |
| `Loan_Status` | Current status per loan (Approved, Pending, Active, Defaulted, etc.) |
| `Repayment_Plan` | Installment amounts and next payment dates |
| `Transaction_Record` | Actual payment transactions against loans |
| `Collateral` | Assets pledged against loans (Real Estate, Vehicles, Gold, IP, etc.) |
| `Dispute` | Dispute records with reasons and resolution statuses |
| `Audit_Log` | Timestamped change log for loan application modifications |
| `Loan_History` | Historical date records for loans |
| `Notification` | System notifications sent to users |
| `Feedback` | User comments about the platform |
| `Rating` | Numeric ratings (1–5) with comments |
| `Referral` | Tracks which users referred others |
| `Startup_Idea` | Startup concepts submitted by borrowers |
| `Applies_for` | Junction table — borrower loan application statuses |
| `Funds` | Junction table — lender funding statuses per loan |

---

## Implementation

### SQL (MySQL)

The SQL script includes complete DDL (table creation with constraints), DML (30 users, 15 loans, and data across all tables), and **10 analytical queries** demonstrating a range of SQL techniques:

| # | Query | SQL Techniques |
|---|---|---|
| 1 | Track modifications made to loan applications | `INNER JOIN` across 3 tables (Borrower ↔ Loan_Application ↔ Audit_Log) |
| 2 | Find borrower(s) with the highest loan amount | Subquery with `MAX()` |
| 3 | Identify borrowers with more than one loan | Derived table with `GROUP BY` + `HAVING COUNT() > 1` |
| 4 | List borrowers with above-average loan amounts | Subquery with `AVG()` |
| 5 | Find borrowers with resolved disputes | `EXISTS` correlated subquery |
| 6 | Lenders with above-average ratings | Correlated subquery + `HAVING` + `AVG()` |
| 7 | Borrowers who never received a notification | `NOT IN` subquery |
| 8 | List startup ideas per borrower | Multi-table join + `GROUP_CONCAT` |
| 9 | Identify which startup ideas each lender funds | Three-table join |
| 10 | List approved loans by borrower and lender | Multi-table join with `WHERE IN` |

### NoSQL (MongoDB)

The project also includes a **MongoDB implementation** on Atlas, demonstrating the same data model in a document-oriented paradigm:

| # | Operation | MongoDB Techniques |
|---|---|---|
| 1 | Total loan amount disbursed per lender | `$group` + `$sum` + `$sort` + `$limit` aggregation pipeline |
| 2 | Borrowers with more than 2 approved loans | `$match` + `$group` + `$match` (SQL `HAVING` equivalent) |
| 3 | List all loans sorted by amount | `find()` with projection |
| 4 | Loan details with borrower information | `$lookup` + `$unwind` + `$project` (SQL `INNER JOIN` equivalent) |
| 5 | Loans with lender details (left outer join) | `$lookup` + `$ifNull` for handling missing lenders |

### Python Analytics & Visualizations

The Jupyter notebook connects to MySQL via `mysql-connector-python` and performs end-to-end analytics with **10+ interactive visualizations**:

| Analysis | Visualization Type | Libraries |
|---|---|---|
| Startup ideas by borrower | Interactive Treemap | Plotly |
| Most frequent terms in startup pitches | Word Cloud | WordCloud, Matplotlib |
| Monthly trend of loan application changes | Color-scaled Bar Chart | Plotly |
| Startup idea categorization (Tech, Health, Finance, etc.) | Pie Chart + Bar Chart | Plotly, Seaborn |
| Lender-funded startup ideas with approval counts | Styled DataFrame with conditional bars | Pandas |
| Funding status distribution | Pie Chart | Plotly |
| Top borrowers by transaction volume | Interactive Bar Chart | Plotly |
| Monthly transaction trends (correlated subquery) | Line Chart | Matplotlib |
| Top 5 lenders: loan amounts vs. disputes | Dual-axis Bar + Line Chart | Matplotlib |
| Lender investment capacity (`ANY` subquery) | Horizontal Bar Chart | Matplotlib, Seaborn |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Relational Database | MySQL |
| NoSQL Database | MongoDB Atlas |
| Backend / Analytics | Python 3, Pandas |
| DB Connectors | `mysql-connector-python`, MongoDB Shell (`mongosh`) |
| Visualization | Plotly, Matplotlib, Seaborn, WordCloud |
| Environment | Jupyter Notebook |

---

## Getting Started

### Prerequisites

- MySQL Server (5.7+)
- Python 3.8+
- Jupyter Notebook
- MongoDB Atlas account (optional, for NoSQL portion)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/Peer-2-Peer-Lending-Platform.git
   cd Peer-2-Peer-Lending-Platform
   ```

2. **Create the database and load schema + data**
   ```bash
   mysql -u root -p -e "CREATE DATABASE DMA_PRO2;"
   mysql -u root -p DMA_PRO2 < DMA_SQL_SCRIPT.sql
   ```

3. **Install Python dependencies**
   ```bash
   pip install mysql-connector-python pandas plotly matplotlib seaborn wordcloud
   ```

4. **Update database credentials** in the notebook  
   Open `implementation_in_python_application.ipynb` and update the `host`, `user`, `password`, and `database` fields in the connection cells.

5. **Launch the notebook**
   ```bash
   jupyter notebook implementation_in_python_application.ipynb
   ```

---

## Project Structure

```
Peer-2-Peer-Lending-Platform/
├── DMA_SQL_SCRIPT.sql                            # Full DDL + DML + 10 analytical queries
├── implementation_in_python_application.ipynb     # Python analytics & visualizations
├── images/
│   └── er_diagram.png                            # Entity-Relationship diagram
└── README.md
```

---

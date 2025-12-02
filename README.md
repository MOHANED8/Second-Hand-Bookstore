# 📚 Second-Hand Bookstore Database Management System  
*A complete SQL Server database project for managing used book sales, trades, and user activity.*

<p align="center">
  <img src="https://img.shields.io/badge/Project-Type-Database--Design-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/Language-SQL%20%26%20JSON-brightgreen?style=flat-square" />
  <img src="https://img.shields.io/badge/DBMS-SQL%20Server-red?style=flat-square" />
  <img src="https://img.shields.io/badge/Model-EER%20%2F%203NF-orange?style=flat-square" />
</p>

---

## 📖 Table of Contents
1. [About the Project](#-about-the-project)
2. [System Features](#-system-features)
3. [Database Architecture](#-database-architecture)
4. [Enhanced ER Diagram](#-enhanced-er-diagram)
5. [Relational Schema](#-relational-schema)
6. [Normalization](#-normalization)
7. [Functional Dependencies](#-functional-dependencies)
8. [SQL Scripts](#-sql-scripts)
9. [Setup & Installation](#-setup--installation)
10. [Sample Queries](#-sample-queries)
11. [Future Improvements](#-future-improvements)
12. [Contributors](#-contributors)
13. [License](#-license)

---

# 📘 About the Project

The **Second-Hand Bookstore Database Management System (SHB-DBMS)** is a complete relational database designed to support an online marketplace for buying, selling, and trading used books.  
It includes:

- User account management  
- Book inventory and metadata  
- Listings of used book copies  
- Order processing  
- Payments and shipments  
- Ratings and reviews  
- User activity tracking using JSON  

This project was developed as part of the **CSE5041: Database Design & Development** course.

---

# 🚀 System Features

### 👤 User Management
- Customer & Admin roles  
- Multiple addresses per user  
- User authentication information  

### 📚 Books & Categories
- Store metadata for all books  
- Multi-author support  
- Hierarchical categories  
- ISBN-based unique identification  

### 🛒 Listings (Used Books)
- Seller-created listings  
- Conditions (Like New, Very Good, Good, Acceptable)  
- Pricing and quantity  
- Status control (Active, PendingApproval, Cancelled, SoldOut)  

### 📦 Orders & Transactions
- Multi-item orders  
- Order status workflow  
- Payments with transaction logs  
- Shipment & tracking details  

### ⭐ Reviews System
- Book reviews (rating + comments)  
- Seller reviews  

### 📊 User Activity (JSON)
- Recently viewed books  
- Search history  
- Stored in JSON format  

---

# 🏗 Database Architecture

This project follows the complete lifecycle:

1. Requirements Analysis  
2. Enhanced ER (EER) Modeling  
3. Relational Schema Mapping  
4. Normalization up to 3rd Normal Form  
5. SQL Server Implementation  
6. Indexing, Constraints, and Triggers  
7. JSON/NoSQL Integration  
8. Data Seeding (~3,300+ rows automatically generated)  

---

# 🖼 Enhanced ER Diagram

> 📌 Placeholder — insert your EER Diagram image here  
(Upload your diagram as: `docs/eer-diagram.png`)


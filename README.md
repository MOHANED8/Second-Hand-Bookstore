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

The **Second-Hand Bookstore Database Management System (SHB-DBMS)** is a robust and fully normalized relational database designed for online trading of used books.  
It supports:

- User registration, authentication & roles  
- Book metadata management  
- Seller-created book listings  
- Order processing & multi-item carts  
- Payment tracking  
- Shipment tracking  
- Book & seller reviews  
- JSON-based user activity logs  

Developed for the course **CSE5041: Database Design & Development**, this project demonstrates full lifecycle database design and implementation using **SQL Server**.

---

# 🚀 System Features

### 👤 User Management
- Customer & Admin roles  
- Multiple addresses (shipping/billing)  
- Secure hashed passwords  

### 📚 Books & Categories
- Full metadata for each book  
- Multi-author support (M:N)  
- Multi-category support (M:N)  
- ISBN uniqueness checking  

### 🛒 Listings (Used Books)
- Book condition (Like New, Very Good, Good, Acceptable)  
- Pricing, quantity & status  
- Seller identity tracking  
- Approval workflow  

### 📦 Orders & Transactions
- Multi-item orders with quantities  
- Order life cycle: Pending → Paid → Shipped → Completed  
- Total cost calculation  

### 💳 Payments
- One payment per order  
- Methods: Card, PayPal, Bank Transfer  
- Payment status tracking  

### 🚚 Shipments
- Carriers, tracking numbers  
- Shipped & delivered dates  

### ⭐ Reviews System
- Book reviews (ratings & comments)  
- Seller reviews (ratings)  
- Timestamped entries  

### 📊 User Activity Logs (JSON)
- Recent searches  
- Recently viewed books  
- Stored in JSON format per user  

---

# 🏗 Database Architecture

This project follows a complete database engineering workflow:

1. **Requirement Collection & Analysis**  
2. **Enhanced ER (EER) Modeling**  
3. **Relational Mapping (PK/FK design)**  
4. **Normalization (UNF → 1NF → 2NF → 3NF)**  
5. **SQL Schema Implementation**  
6. **Constraints, Indexes, Keys, Views, Procedures, Triggers**  
7. **Data Seeding (~3,300+ records)**  
8. **JSON/NoSQL Integration**  

---

# 🖼 Enhanced ER Diagram


![EER Diagram](https://github.com/user-attachments/assets/8e5fc16b-62e5-4a50-99bd-ba0e6b84f2d8)



# 🗂 Relational Schema

<img width="1024" height="1024" alt="Image" src="https://github.com/user-attachments/assets/b20f98a9-fa04-4a1a-8f5f-c28367378ab1" />

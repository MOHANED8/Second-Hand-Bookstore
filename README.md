# 📚 Second-Hand Bookstore Database Management System

<p align="center">
  <em>A comprehensive SQL Server database for managing used book marketplace operations</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/SQL%20Server-2016+-CC2927?style=flat-square&logo=microsoft-sql-server" />
  <img src="https://img.shields.io/badge/Normalization-3NF-success?style=flat-square" />
  <img src="https://img.shields.io/badge/Records-3300+-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" />
</p>

---

## 📋 Overview

**SHB-DBMS** is a fully normalized relational database built for online trading of used books. Developed for **CSE5041: Database Design & Development**, this project demonstrates enterprise-level database architecture with complete CRUD operations, transaction management, and NoSQL integration.

**Core Capabilities:** User authentication | Book catalog | Marketplace listings | Order processing | Payment & shipment tracking | Review system | JSON activity logs

---

## 🎯 Key Features

<table>
<tr>
<td width="50%">

**User & Inventory Management**
- Role-based access (Customer/Admin)
- Multi-author & multi-category support
- ISBN validation & uniqueness
- Book condition grading system

</td>
<td width="50%">

**Transactions & Analytics**
- Multi-item cart processing
- Status-driven order workflow
- Payment method tracking
- Shipment & delivery monitoring

</td>
</tr>
<tr>
<td>

**Social & Activity**
- 5-star rating system
- Book & seller reviews
- JSON-based user activity logs
- Search & viewing history

</td>
<td>

**Automation**
- Auto-inventory updates (triggers)
- Stored procedures for operations
- Pre-built analytical views
- XML import/export support

</td>
</tr>
</table>

---

## 🏗️ Database Architecture

### Entity Relationship Diagram
![EER Diagram](https://github.com/user-attachments/assets/8e5fc16b-62e5-4a50-99bd-ba0e6b84f2d8)

### Relational Schema
![Relational Schema](https://github.com/user-attachments/assets/b20f98a9-fa04-4a1a-8f5f-c28367378ab1)

### Schema Summary

| Entity | Type | Key Relationships |
|--------|------|-------------------|
| **Users** | Core | → Addresses (1:M), Listings (1:M), Orders (1:M) |
| **Books** | Core | ↔ Authors (M:N), Categories (M:N) |
| **Listings** | Core | → Books (M:1), Conditions (M:1), OrderItems (1:M) |
| **Orders** | Core | → OrderItems (1:M), Payment (1:1), Shipment (1:1) |
| **Reviews** | Supplementary | → Books/Sellers (M:1) |
| **UserActivity** | NoSQL | Stores JSON for flexible querying |

**Total Tables:** 14 | **Bridge Tables:** 2 (BookAuthors, BookCategories)

---

## 🔐 Normalization & Dependencies

### Normalization Level: **3NF**

| Form | Implementation | Example |
|------|----------------|---------|
| **1NF** | Atomic values, no repeating groups | Multi-author via `BookAuthors` bridge table |
| **2NF** | No partial dependencies | `OrderItems(order_id, listing_id)` → all attrs depend on both keys |
| **3NF** | No transitive dependencies | Price in `Listings` (not `Books`) - depends on seller & condition |

### Key Functional Dependencies

```sql
-- Primary Determinants
user_id → username, email, password_hash, role
book_id → title, isbn, publication_year, language
listing_id → book_id, seller_id, condition_id, price, quantity, status
order_id → buyer_id, order_date, status, shipping_address_id, total_amount

-- Composite Keys
(order_id, listing_id) → seller_id, quantity, unit_price
(book_id, author_id) → [bridge table, no additional attributes]

-- 1:1 Relationships
order_id ↔ payment_id
order_id ↔ shipment_id
```

---

## ⚙️ Quick Start

### Prerequisites
- SQL Server 2016+ | SSMS/Azure Data Studio | 100MB disk space

### Installation

```sql
-- 1. Create database
:r "DATABASE SecondHandBookstore.sql"

-- 2. Build schema
:r "Core Tables.sql"
:r "Indexes for Performance.sql"

-- 3. Load data (choose one)
:r "DATA1.sql"  -- 3,300+ records (recommended)
:r "DATA.sql"   -- 250 records (testing)

-- 4. Add business logic
:r "Stored Procedure Add Listing.sql"
:r "Stored Procedure Approve Listing.sql"
:r "Stored Procedure Create Order.sql"
:r "Trigger Reduce Stock on OrderItems Insert.sql"
:r "Example Views.sql"
```

### Verify Installation

```sql
-- Check tables
SELECT name FROM sys.tables ORDER BY name;

-- Verify data (DATA1.sql expected counts)
SELECT 
    t.name AS [Table],
    SUM(p.rows) AS [Rows]
FROM sys.tables t
JOIN sys.partitions p ON t.object_id = p.object_id
WHERE p.index_id IN (0,1)
GROUP BY t.name
ORDER BY [Rows] DESC;
-- Expected: OrderItems(900), Listings(400), Orders(300), Books(120)...
```

---

## 🔍 Usage Examples

### Common Queries

```sql
-- Search active listings
SELECT * FROM vw_ActiveListings 
WHERE title LIKE '%Harry Potter%' 
ORDER BY price;

-- Top sellers
SELECT TOP 5 * FROM vw_SalesBySeller 
ORDER BY total_sales_amount DESC;

-- User purchase history
SELECT o.order_id, b.title, oi.quantity, oi.unit_price
FROM Orders o
JOIN OrderItems oi ON o.order_id = oi.order_id
JOIN Listings l ON oi.listing_id = l.listing_id
JOIN Books b ON l.book_id = b.book_id
WHERE o.buyer_id = @UserId
ORDER BY o.order_date DESC;

-- Parse JSON activity
SELECT 
    username,
    JSON_VALUE(json_data, '$.last_login') AS last_login,
    JSON_QUERY(json_data, '$.recently_viewed') AS recent_books
FROM UserActivity ua
JOIN Users u ON ua.user_id = u.user_id;
```

### Stored Procedures

```sql
-- Add listing
EXEC sp_AddListing 
    @book_id = 10, @seller_id = 5, @condition_id = 2,
    @price = 12.99, @quantity = 3;

-- Create order with transaction
DECLARE @OrderId INT;
EXEC sp_CreateOrder 
    @buyer_id = 8, @shipping_address_id = 15,
    @total_amount = 45.99, @new_order_id = @OrderId OUTPUT;
```

---

## 📂 Project Structure

```
secondhand-bookstore-db/
├── DATABASE SecondHandBookstore.sql    # DB creation
├── Core Tables.sql                     # Schema definition
├── DATA.sql / DATA1.sql                # Sample data (250 / 3,300 rows)
├── Indexes for Performance.sql         # Query optimization
├── Stored Procedure *.sql              # Business logic (3 files)
├── Trigger *.sql                       # Auto-inventory management
├── Example Views.sql                   # Analytical views
└── Sample Queries/                     # Usage examples (7 files)
    ├── Search Active Listings By Title.sql
    ├── Top 5 Best-Selling Books.sql
    ├── Sales History *.sql
    └── Export/Import XML.sql
```

---

## 🚀 Roadmap

**Phase 1 - Completed ✓**
- [x] Full 3NF schema with 14 tables
- [x] JSON integration for user activity
- [x] Triggers & stored procedures
- [x] Sample data (3,300+ records)

**Phase 2 - Planned**
- [ ] Wishlist & shopping cart
- [ ] Email notification system
- [ ] Advanced search (price/rating filters)
- [ ] Seller analytics dashboard

**Phase 3 - Future**
- [ ] RESTful API layer
- [ ] Full-text search implementation
- [ ] Table partitioning (Orders by year)
- [ ] Recommendation engine

---

## 👤 Project Information

**Author:** [Your Name]  
**Course:** CSE5041 - Database Design & Development  
**Institution:** [Your University]  
**Semester:** [Fall/Spring Year]

**Contact:** [your.email@example.com] | [GitHub](https://github.com/yourusername) | [LinkedIn](https://linkedin.com/in/yourprofile)

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 📚 Resources

**Documentation:** [SQL Server Docs](https://docs.microsoft.com/en-us/sql/) | [Normalization Guide](https://www.studytonight.com/dbms/database-normalization.php)  
**Issues:** [Report bugs](https://github.com/yourusername/secondhand-bookstore-db/issues)  
**Contributions:** PRs welcome! See [CONTRIBUTING.md](CONTRIBUTING.md)

---

<p align="center">
  <strong>⭐ Star this repo if you find it useful!</strong><br/>
  <sub>Built with ❤️ for database enthusiasts</sub>
</p>

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

**Key Relationships:**
- Users (1) ↔ (M) Addresses
- Books (M) ↔ (N) Authors via BookAuthors
- Books (M) ↔ (N) Categories via BookCategories
- Users (1) ↔ (M) Listings (as sellers)
- Listings (M) ↔ (N) Orders via OrderItems
- Orders (1) ↔ (1) Payment
- Orders (1) ↔ (1) Shipment

---

# 🗂 Relational Schema

![Relational Schema](https://github.com/user-attachments/assets/b20f98a9-fa04-4a1a-8f5f-c28367378ab1)

### ✔ Core Tables

- **Users** - User accounts with roles (Customer/Admin)
- **Addresses** - Shipping and billing addresses
- **Books** - Book catalog with ISBN, title, publication details
- **Authors** - Author information and biographies
- **Categories** - Hierarchical book categories
- **Conditions** - Book condition lookup table
- **Listings** - Books for sale with price, quantity, status
- **Orders** - Purchase orders with lifecycle tracking
- **OrderItems** - Line items connecting orders to listings
- **Payments** - Payment transactions and methods
- **Shipments** - Shipping and delivery tracking
- **BookReviews** - User ratings and reviews of books
- **SellerReviews** - User ratings and reviews of sellers
- **UserActivity** - JSON-based user activity logs

All PKs, FKs, and relationships are represented in the schema diagram.

---

# 🧩 Normalization

The Second-Hand Bookstore Database is fully normalized to **Third Normal Form (3NF)** to ensure data integrity, reduce redundancy, and improve performance.

### ✔ First Normal Form (1NF)
- All attributes are atomic  
- No repeating groups  
- Multivalued attributes represented through bridge tables:
  - `BookAuthors` - Resolves M:N between Books and Authors
  - `BookCategories` - Resolves M:N between Books and Categories

### ✔ Second Normal Form (2NF)
- All non-key attributes in composite-key tables depend on the entire primary key  
- `OrderItems(order_id, listing_id)` - All attributes depend on both keys
- No partial dependencies exist in any table

### ✔ Third Normal Form (3NF)
- No transitive dependencies  
- All non-key attributes depend solely on their table's primary key  
- Example: Book pricing is in Listings (not Books) as price depends on seller and condition
- Referential integrity enforced via foreign keys  

---

# 🔗 Functional Dependencies

### Users Table
```
user_id → username, email, password_hash, role, created_at
username → user_id, email, password_hash, role, created_at
email → user_id, username, password_hash, role, created_at
```

### Addresses Table
```
address_id → user_id, line1, line2, city, state, postal_code, country, address_type
```

### Books Table
```
book_id → title, isbn, publication_year, language, edition
isbn → book_id (when not null)
```

### Authors Table
```
author_id → name, biography
```

### Categories Table
```
category_id → name, parent_category_id
```

### Conditions Table
```
condition_id → name, description
```

### Listings Table
```
listing_id → book_id, seller_id, condition_id, price, quantity, status, created_at, updated_at
```

### Orders Table
```
order_id → buyer_id, order_date, status, shipping_address_id, total_amount
```

### OrderItems Table (Composite Key)
```
(order_id, listing_id) → seller_id, quantity, unit_price
```
*Note: This composite key ensures no duplicate listings in the same order*

### Payments Table
```
payment_id → order_id, payment_method, payment_date, amount, status
order_id → payment_id (1:1 relationship)
```

### Shipments Table
```
shipment_id → order_id, shipped_date, carrier, tracking_number, delivery_date
order_id → shipment_id (1:1 relationship)
```

### BookReviews Table
```
review_id → book_id, reviewer_id, rating, comment, created_at
```

### SellerReviews Table
```
review_id → seller_id, reviewer_id, rating, comment, created_at
```

### UserActivity Table
```
activity_id → user_id, json_data, created_at
```

---

# 📜 SQL Scripts

### Database Setup
| File | Description |
|------|-------------|
| `DATABASE SecondHandBookstore.sql` | Creates the database |
| `Core Tables.sql` | Defines all table structures with constraints |
| `Indexes for Performance.sql` | Creates indexes for common queries |

### Data Population
| File | Description | Records |
|------|-------------|---------|
| `DATA.sql` | Small dataset for testing | ~250 rows |
| `DATA1.sql` | Large dataset with cleanup & reset | ~3,300 rows |

### Automation & Business Logic
| File | Description |
|------|-------------|
| `Stored Procedure Add Listing.sql` | Adds new book listings with validation |
| `Stored Procedure Approve Listing.sql` | Approves pending listings (Admin) |
| `Stored Procedure Create Order.sql` | Creates orders with transaction support |
| `Trigger Reduce Stock on OrderItems Insert.sql` | Auto-updates inventory & marks sold-out items |

### Views
| File | Description |
|------|-------------|
| `Example Views.sql` | Pre-built views (Active Listings, Sales, Ratings) |

### Sample Queries & Reports
| File | Description |
|------|-------------|
| `Search Active Listings By Title.sql` | Search available books by title |
| `Get Sales History for a Given Seller.sql` | Seller sales report |
| `Sales History for a Buyer.sql` | Buyer purchase history |
| `Top 5 Best-Selling Books.sql` | Best sellers by quantity and revenue |
| `Export Orders (with Items) to XML.sql` | XML data export for orders |
| `Import Books from XML.sql` | XML data import for books |

---

# ⚙ Setup & Installation

### Prerequisites
- **SQL Server 2016+** (Express, Standard, or Enterprise)
- **SQL Server Management Studio (SSMS)** or **Azure Data Studio**
- Minimum **100MB** disk space

### Installation Steps

#### Option 1: Using SSMS (Recommended)

1. **Clone or download the repository**
```bash
git clone https://github.com/yourusername/secondhand-bookstore-db.git
cd secondhand-bookstore-db
```

2. **Open SSMS** and connect to your SQL Server instance

3. **Execute scripts in order:**

```sql
-- Step 1: Create database
:r "DATABASE SecondHandBookstore.sql"

-- Step 2: Create tables
:r "Core Tables.sql"

-- Step 3: Add indexes
:r "Indexes for Performance.sql"

-- Step 4: Populate data (choose one)
:r "DATA1.sql"  -- Recommended: ~3,300 records

-- Step 5: Create stored procedures
:r "Stored Procedure Add Listing.sql"
:r "Stored Procedure Approve Listing.sql"
:r "Stored Procedure Create Order.sql"

-- Step 6: Create trigger
:r "Trigger Reduce Stock on OrderItems Insert.sql"

-- Step 7: Create views
:r "Example Views.sql"
```

#### Option 2: Quick Setup Script

Create a `setup.sql` file:
```sql
USE master;
GO

-- Create database
IF NOT EXISTS (SELECT * FROM sys.databases WHERE name = 'SecondHandBookstore')
BEGIN
    CREATE DATABASE SecondHandBookstore;
END
GO

USE SecondHandBookstore;
GO

-- Then execute each file in order as shown above
```

### Verification

Check installation success:

```sql
-- Verify all tables exist
SELECT name, type_desc 
FROM sys.objects 
WHERE type IN ('U', 'V', 'P', 'TR')
ORDER BY type_desc, name;

-- Check row counts
SELECT 
    t.name AS TableName,
    SUM(p.rows) AS RowCount
FROM sys.tables t
JOIN sys.partitions p ON t.object_id = p.object_id
WHERE p.index_id IN (0,1)
GROUP BY t.name
ORDER BY t.name;

-- Expected output for DATA1.sql:
-- Users: 60
-- Books: 120
-- Listings: 400
-- Orders: 300
-- OrderItems: 900
-- etc.
```

---

# 🔍 Sample Queries

### Basic Queries

#### 1. Search for books by title
```sql
SELECT *
FROM vw_ActiveListings
WHERE title LIKE '%Harry Potter%'
ORDER BY price;
```

#### 2. Get top sellers by revenue
```sql
SELECT TOP 5 
    seller_username,
    total_sales_amount,
    orders_count
FROM vw_SalesBySeller
ORDER BY total_sales_amount DESC;
```

#### 3. Find highly-rated books
```sql
SELECT 
    title,
    avg_rating,
    review_count
FROM vw_BookRatings
WHERE avg_rating >= 4.0 AND review_count >= 5
ORDER BY avg_rating DESC, review_count DESC;
```

### Intermediate Queries

#### 4. User's purchase history with details
```sql
DECLARE @BuyerId INT = 5;

SELECT 
    o.order_id,
    o.order_date,
    o.status,
    b.title,
    oi.quantity,
    oi.unit_price,
    (oi.quantity * oi.unit_price) AS line_total
FROM Orders o
JOIN OrderItems oi ON o.order_id = oi.order_id
JOIN Listings l ON oi.listing_id = l.listing_id
JOIN Books b ON l.book_id = b.book_id
WHERE o.buyer_id = @BuyerId
ORDER BY o.order_date DESC;
```

#### 5. Books by specific author
```sql
SELECT DISTINCT 
    b.book_id,
    b.title,
    b.publication_year,
    a.name AS author_name
FROM Books b
JOIN BookAuthors ba ON b.book_id = ba.book_id
JOIN Authors a ON ba.author_id = a.author_id
WHERE a.name LIKE '%Rowling%';
```

#### 6. Orders pending shipment
```sql
SELECT 
    o.order_id,
    u.username AS buyer,
    o.order_date,
    o.total_amount,
    p.status AS payment_status
FROM Orders o
JOIN Users u ON o.buyer_id = u.user_id
JOIN Payments p ON o.order_id = p.order_id
WHERE o.status = 'Paid' 
  AND NOT EXISTS (
      SELECT 1 FROM Shipments s WHERE s.order_id = o.order_id
  );
```

### Advanced Queries

#### 7. User activity with JSON parsing
```sql
SELECT 
    u.username,
    u.email,
    JSON_VALUE(ua.json_data, '$.last_login') AS last_login,
    JSON_QUERY(ua.json_data, '$.recently_viewed') AS recently_viewed_books,
    JSON_QUERY(ua.json_data, '$.search_history') AS search_history
FROM UserActivity ua
JOIN Users u ON ua.user_id = u.user_id
WHERE u.user_id = 5;
```

#### 8. Books that have never been sold
```sql
SELECT 
    b.book_id,
    b.title,
    b.isbn,
    a.name AS author
FROM Books b
JOIN BookAuthors ba ON b.book_id = ba.book_id
JOIN Authors a ON ba.author_id = a.author_id
WHERE NOT EXISTS (
    SELECT 1
    FROM Listings l
    JOIN OrderItems oi ON l.listing_id = oi.listing_id
    WHERE l.book_id = b.book_id
)
ORDER BY b.title;
```

#### 9. Sellers with high ratings and sales
```sql
SELECT 
    u.username,
    COUNT(DISTINCT l.listing_id) AS total_listings,
    COALESCE(AVG(CAST(sr.rating AS FLOAT)), 0) AS avg_seller_rating,
    COALESCE(SUM(oi.quantity * oi.unit_price), 0) AS total_revenue
FROM Users u
LEFT JOIN Listings l ON u.user_id = l.seller_id
LEFT JOIN SellerReviews sr ON u.user_id = sr.seller_id
LEFT JOIN OrderItems oi ON u.user_id = oi.seller_id
WHERE u.role = 'Customer'
GROUP BY u.user_id, u.username
HAVING COALESCE(AVG(CAST(sr.rating AS FLOAT)), 0) >= 4.0
ORDER BY total_revenue DESC;
```

### Using Stored Procedures

#### 10. Add a new listing
```sql
EXEC sp_AddListing
    @book_id = 10,
    @seller_id = 5,
    @condition_id = 2,  -- Very Good
    @price = 12.99,
    @quantity = 3;
```

#### 11. Approve a listing (Admin)
```sql
EXEC sp_ApproveListing
    @listing_id = 25;
```

#### 12. Create a new order
```sql
DECLARE @NewOrderId INT;

EXEC sp_CreateOrder
    @buyer_id = 8,
    @shipping_address_id = 15,
    @total_amount = 45.99,
    @new_order_id = @NewOrderId OUTPUT;

SELECT @NewOrderId AS CreatedOrderId;
```

---

# 🔮 Future Improvements

### Planned Features
- [ ] **Wishlist System** - Users can save books for later
- [ ] **Shopping Cart** - Temporary cart storage before checkout
- [ ] **Email Notifications** - Order confirmations and shipping updates
- [ ] **Advanced Search** - Filter by price range, condition, rating, author
- [ ] **Seller Dashboard** - Analytics, inventory management, sales charts
- [ ] **Recommendation Engine** - Based on browsing history and purchases
- [ ] **Bulk Operations** - CSV import/export for books and listings
- [ ] **RESTful API** - JSON API for mobile/web applications
- [ ] **Real-time Alerts** - Low inventory warnings, new listing notifications

### Scalability Enhancements
- [ ] **Table Partitioning** - Partition Orders and OrderItems by year
- [ ] **Archival Strategy** - Move completed orders older than 2 years to archive tables
- [ ] **Full-Text Search** - Implement FTS on book titles and descriptions
- [ ] **Caching Layer** - Redis/Memcached for frequently accessed data
- [ ] **Read Replicas** - Separate replicas for reporting queries
- [ ] **Query Optimization** - Additional composite indexes based on usage patterns

### Advanced Features
- [ ] **Multi-currency Support** - International pricing
- [ ] **Auction System** - Bidding on rare books
- [ ] **Book Exchange** - Trade books without money
- [ ] **Loyalty Program** - Points and rewards system
- [ ] **AI-powered Pricing** - Suggest optimal prices based on market data

---

# 👥 Contributors

### Project Team
- **Your Name** - *Lead Database Designer & Developer*
  - [GitHub](https://github.com/yourusername)
  - [LinkedIn](https://linkedin.com/in/yourprofile)
  - [Email](mailto:your.email@example.com)

### Course Information
- **Course:** CSE5041 - Database Design & Development
- **Institution:** [Your University Name]
- **Semester:** [Fall/Spring Year]
- **Instructor:** [Professor Name]

### Acknowledgments
Special thanks to:
- Professor [Name] for guidance and feedback
- Course TAs for technical support
- SQL Server community for documentation and best practices
- Fellow students for testing and suggestions

---

# 📄 License

This project is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2024 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

See the [LICENSE](LICENSE) file for full details.

---

# 📚 Additional Resources

### Documentation
- [SQL Server Documentation](https://docs.microsoft.com/en-us/sql/)
- [Database Normalization Guide](https://www.studytonight.com/dbms/database-normalization.php)
- [ER Diagram Best Practices](https://www.lucidchart.com/pages/er-diagrams)

### Related Projects
- [Bookstore API Example](https://github.com/examples/bookstore-api)
- [E-commerce Database Design](https://github.com/examples/ecommerce-db)

### Contact
For questions, suggestions, or contributions:
- 📧 Email: [your.email@example.com]
- 💬 GitHub Issues: [Create an issue](https://github.com/yourusername/secondhand-bookstore-db/issues)
- 🐛 Bug Reports: Use the issue tracker with the `bug` label

---

<p align="center">
  <strong>⭐ If you find this project helpful, please give it a star! ⭐</strong>
</p>

<p align="center">
  <sub>Built with passion for database design and development</sub>
</p>

<p align="center">
  Made with ❤️ by database enthusiasts
</p>

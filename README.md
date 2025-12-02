# 📚 Second-Hand Bookstore Database
*A complete, production-style SQL Server marketplace database — beautifully structured, professionally documented.*

---

<p align="center">
  <img src="https://github.com/user-attachments/assets/8e5fc16b-62e5-4a50-99bd-ba0e6b84f2d8" alt="EER Diagram" width="650"/>
</p>
<p align="center"><em>Entity-Relationship Diagram (EER)</em></p>

---

## ✨ Overview
This repository provides a fully implemented **Second-Hand Bookstore Marketplace Database**, featuring a modern design, clean structure, and real-world business logic. It is ideal for:
- Academic and teaching scenarios
- Software engineering and backend development practice
- Marketplace prototyping
- Demonstrations of SQL Server capabilities

---

## 🧱 Core Components
### **1. Schema Architecture**
A well-normalized relational schema including:
- **Users**, **Addresses**, **Books**, **Listings**, **Orders**, **OrderItems**
- Review systems for both books and sellers
- Complete referential integrity (PKs, FKs, cascades)

### **2. Data Initialization**
Rich sample datasets enabling:
- Immediate testing
- Demo flows (search → listing → order → reviews)

### **3. Business Logic Layer**
Includes professionally structured T-SQL features:
- **Stored Procedures** (listing creation, approval, orders)
- **Triggers** (stock reduction & SoldOut automation)
- **Views** for simplified querying
- **Indexes** for optimized performance

### **4. XML Integration**
- Import Books from XML
- Export Orders + Items to XML

---

## 🧩 Relational Schema
<p align="center">
  <img src="https://github.com/user-attachments/assets/b20f98a9-fa04-4a1a-8f5f-c28367378ab1" alt="Relational Schema" width="650"/>
</p>
<p align="center"><em>Logical Relational Schema</em></p>

---

## 🔍 Key Features
### **Marketplace-Ready Design**
- Multi-seller listings
- Variable pricing & book conditions
- Accurate stock tracking

### **Robust Order Workflow**
- Transaction-protected order creation
- Linked OrderItems detail
- Automatic SoldOut state handling

### **Performance and Scalability**
- Tailored indexes for real-world search patterns
- Modular stored procedures
- Efficient view-based querying

### **Extensibility by Design**
Future features can be added cleanly, such as:
- Wishlists
- Shopping cart
- Messaging between buyers and sellers
- Payment flow integration

---

## 🚀 Getting Started
### **1. Create the Database**
Run the core schema scripts to build tables and constraints.

### **2. Load Sample Data**
Execute:
- `DATA.sql`
- `DATA1.sql`

### **3. Add Logic Layer**
Install sequentially:
1. Views
2. Stored Procedures
3. Triggers
4. Indexes

### **4. Execute Sample Queries**
Test end-to-end flows:
- Buyer sales history
- Seller performance
- Title-based listing search
- Best-selling books

---

## 📁 Repository Structure
```
/ ─ Core Tables.sql
  ─ DATABASE SecondHandBookstore.sql
  ─ DATA.sql
  ─ DATA1.sql
  ─ Example Views.sql
  ─ Stored Procedure Add Listing.sql
  ─ Stored Procedure Approve Listing.sql
  ─ Stored Procedure Create Order.sql
  ─ Trigger Reduce Stock on OrderItems Insert.sql
  ─ Search Active Listings By Title.sql
  ─ Get Sales History for a Given Seller.sql
  ─ Sales History for a Buyer.sql
  ─ Top 5 Best-Selling Books.sql
  ─ Import Books from XML.sql
  ─ Export Orders (with Items) to XML.sql
  ─ Indexes for Performance.sql
```

---

## 🛠 Technology Stack
- **Microsoft SQL Server (T-SQL)** — Primary implementation

**Can be adapted to:**
- PostgreSQL
- MySQL
- MariaDB
- SQLite

(Ask if you'd like an automatically generated port!)

---

## 🧾 Summary
This project combines **technical rigor**, **clean design**, and **practical marketplace logic** to form a polished, extensible SQL Server application. Its thoughtful architecture and well-written scripts make it suited for real use cases, education, or portfolio improvement.

If you’d like further enhancements, I can add:
- 🌟 GitHub badges
- 🗺 A Mermaid-style ERD rendered directly in markdown
- 🧪 Testing workflows (sample transactions)
- 📝 Documentation for API integration
- 🔧 Additional procedures (cart system, wishlist system, admin tools)

Just tell me and I’ll upgrade the design even further! 🚀

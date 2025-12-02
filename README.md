# SecondHandBookstore Database 

## Overview
This is a comprehensive SQL Server database system for managing a second-hand bookstore marketplace where users can buy and sell used books.

## Database Schema

### Core Entities

#### Users
Stores user accounts for both customers and administrators.
- **Primary Key**: `user_id` (INT IDENTITY)
- **Key Fields**: `username`, `email`, `password_hash`, `role`
- **Roles**: 'Customer', 'Admin'
- **Constraints**: Unique username and email

#### Books
Contains book catalog information.
- **Primary Key**: `book_id` (INT IDENTITY)
- **Key Fields**: `title`, `isbn`, `publication_year`, `language`, `edition`
- **Constraints**: Unique ISBN

#### Authors
Stores author information with biographical details.
- **Primary Key**: `author_id` (INT IDENTITY)
- **Key Fields**: `name`, `biography`

#### Categories
Hierarchical category structure for book classification.
- **Primary Key**: `category_id` (INT IDENTITY)
- **Key Fields**: `name`, `parent_category_id`
- **Self-referencing**: Supports nested categories

#### Addresses
User addresses for shipping and billing.
- **Primary Key**: `address_id` (INT IDENTITY)
- **Foreign Key**: `user_id` → Users
- **Types**: 'Shipping', 'Billing'

#### Conditions
Lookup table for book condition ratings.
- **Primary Key**: `condition_id` (INT IDENTITY)
- **Values**: 'Like New', 'Very Good', 'Good', 'Acceptable'

### Relationship Tables

#### BookAuthors (Many-to-Many)
Links books to authors (books can have multiple authors).
- **Composite Primary Key**: (`book_id`, `author_id`)
- **Foreign Keys**: Books, Authors

#### BookCategories (Many-to-Many)
Links books to categories (books can belong to multiple categories).
- **Composite Primary Key**: (`book_id`, `category_id`)
- **Foreign Keys**: Books, Categories

### Transaction Tables

#### Listings
Individual book listings from sellers.
- **Primary Key**: `listing_id` (INT IDENTITY)
- **Foreign Keys**: `book_id`, `seller_id`, `condition_id`
- **Key Fields**: `price`, `quantity`, `status`, `created_at`, `updated_at`
- **Status Values**: 'PendingApproval', 'Active', 'SoldOut', 'Cancelled'
- **Business Rules**: Price > 0, Quantity ≥ 0

#### Orders
Customer purchase orders.
- **Primary Key**: `order_id` (INT IDENTITY)
- **Foreign Keys**: `buyer_id` (Users), `shipping_address_id` (Addresses)
- **Status Values**: 'Pending', 'Paid', 'Shipped', 'Completed', 'Cancelled'
- **Key Fields**: `order_date`, `total_amount`

#### OrderItems (Many-to-Many)
Line items within orders linking to specific listings.
- **Composite Primary Key**: (`order_id`, `listing_id`)
- **Foreign Keys**: Orders, Listings, Users (seller)
- **Key Fields**: `quantity`, `unit_price`
- **Triggers**: Automatically reduces listing quantity on insert

#### Payments
Payment records for orders.
- **Primary Key**: `payment_id` (INT IDENTITY)
- **Foreign Key**: `order_id`
- **Status Values**: 'Pending', 'Success', 'Failed'
- **Key Fields**: `payment_method`, `payment_date`, `amount`

#### Shipments
Shipping information for orders.
- **Primary Key**: `shipment_id` (INT IDENTITY)
- **Foreign Key**: `order_id`
- **Key Fields**: `shipped_date`, `carrier`, `tracking_number`, `delivery_date`

### Review Tables

#### BookReviews
Customer reviews and ratings for books.
- **Primary Key**: `review_id` (INT IDENTITY)
- **Foreign Keys**: `book_id`, `reviewer_id` (Users)
- **Rating**: 1-5 scale
- **Key Fields**: `rating`, `comment`, `created_at`

#### SellerReviews
Customer reviews and ratings for sellers.
- **Primary Key**: `review_id` (INT IDENTITY)
- **Foreign Keys**: `seller_id` (Users), `reviewer_id` (Users)
- **Rating**: 1-5 scale
- **Key Fields**: `rating`, `comment`, `created_at`

### Optional Table

#### UserActivity (JSON Storage)
Stores user activity data in JSON format.
- **Primary Key**: `activity_id` (INT IDENTITY)
- **Foreign Key**: `user_id`
- **JSON Fields**: `last_login`, `recently_viewed`, `search_history`

## Database Objects

### Views

#### vw_ActiveListings
Shows all active listings with book and seller information.
```sql
Columns: listing_id, title, isbn, seller_username, price, quantity, created_at
Filter: status = 'Active'
```

#### vw_SalesBySeller
Aggregates sales performance by seller.
```sql
Columns: seller_id, seller_username, total_sales_amount, orders_count
Filter: Paid/Shipped/Completed orders only
```

#### vw_BookRatings
Shows average ratings and review counts for books.
```sql
Columns: book_id, title, avg_rating, review_count
```

### Stored Procedures

#### sp_AddListing
Creates a new book listing in 'PendingApproval' status.
**Parameters**: `@book_id`, `@seller_id`, `@condition_id`, `@price`, `@quantity`

#### sp_ApproveListing
Approves a pending listing, changing status to 'Active'.
**Parameters**: `@listing_id`

#### sp_CreateOrder
Creates a new order with transaction handling.
**Parameters**: `@buyer_id`, `@shipping_address_id`, `@total_amount`
**Output**: `@new_order_id`

### Triggers

#### trg_OrderItems_AfterInsert
Automatically manages listing inventory after order items are inserted.
- Reduces listing quantity by ordered amount
- Sets status to 'SoldOut' when quantity reaches 0

### Indexes

Performance optimization indexes on frequently queried fields:
- `IX_Books_Title` - Search books by title
- `IX_Books_ISBN` - Lookup books by ISBN
- `IX_Listings_Book_Status` - Filter listings by book and status
- `IX_Orders_Buyer_Date` - User order history
- `IX_OrderItems_Seller` - Seller sales queries
- `IX_BookReviews_Book` - Book review aggregation
- `IX_SellerReviews_Seller` - Seller rating aggregation

## Data Files

### DATA.sql (Small Dataset)
Seed data with ~200+ total rows across all tables:
- 20 users (18 customers, 2 admins)
- 20 addresses
- 10 authors
- 8 categories
- 25 books
- 45 listings
- 30 orders
- 60 order items
- 40 book reviews
- 20 seller reviews

### DATA1.sql (Large Dataset)
Comprehensive seed data with 3000+ total rows:
- 60 users (50 customers, 10 admins)
- 80 addresses
- 20 authors
- 10 categories
- 120 books
- 400 listings
- 300 orders
- 900 order items
- 400 book reviews
- 200 seller reviews
- 50 user activity records (JSON)

**Note**: DATA1.sql includes safety features:
- Clears existing data in FK-safe order
- Reseeds identity columns
- Disables/enables OrderItems trigger during seeding

## Common Queries

### Search Active Listings
```sql
SELECT * FROM vw_ActiveListings WHERE title LIKE '%search term%';
```

### Get Sales History for Seller
```sql
-- Uses @SellerId parameter
-- Returns: order_id, order_date, title, quantity, unit_price, line_total
-- Filters: Paid/Shipped/Completed orders
```

### Get Sales History for Buyer
```sql
-- Uses @BuyerId parameter
-- Returns: order_id, order_date, order_total
-- Aggregates order items by order
```

### Top 5 Best-Selling Books
```sql
-- Returns: book_id, title, total_copies_sold, total_revenue
-- Filters: Paid/Shipped/Completed orders
-- Sorted by copies sold descending
```

## XML Operations

### Export Orders to XML
Exports orders with nested order items in hierarchical XML format.
**Structure**: `<Orders><Order><Items><Item/></Item></Items></Order></Orders>`

### Import Books from XML
Parses XML input to insert new books into the database.
**Format**: Expects XML with Book elements containing Title, ISBN, etc.

## Setup Instructions

1. **Create Database**
   ```sql
   CREATE DATABASE SecondHandBookstore;
   USE SecondHandBookstore;
   ```

2. **Create Schema**: Run `Core Tables.sql`

3. **Create Database Objects**: Run stored procedures, views, triggers, and indexes

4. **Seed Data**: Run either `DATA.sql` (small) or `DATA1.sql` (large)

## Business Rules Summary

- All prices and amounts must be positive
- Listings can have zero quantity (marked as SoldOut)
- Orders track buyer and multiple sellers (marketplace model)
- Ratings are constrained to 1-5 scale
- Listings require approval before becoming active
- Order items automatically reduce listing inventory via trigger
- Users can have multiple addresses (shipping/billing)
- Books support multiple authors and categories
- Categories support hierarchical structure

## Key Relationships

```
Users (1) ─── (M) Addresses
Users (1) ─── (M) Listings (as seller)
Users (1) ─── (M) Orders (as buyer)
Books (M) ─── (M) Authors
Books (M) ─── (M) Categories
Books (1) ─── (M) Listings
Listings (1) ─── (M) OrderItems
Orders (1) ─── (M) OrderItems
Orders (1) ─── (1) Payment
Orders (1) ─── (1) Shipment
```

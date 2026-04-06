# 🛍️ Instagram Thrift & Handmade Store Database Schema

## 📖 Overview
This project contains the database design and Entity-Relationship (ER) diagram for a small creator's social commerce business. The store operates primarily via Instagram DMs and WhatsApp, selling a mix of unique thrifted fashion pieces and batch-produced handmade items. 

The database is designed to handle the specific needs of this business, including differentiating unique inventory from reusable stock, tracking the exact social media channel an order came from, and keeping logistics (payment and shipping) cleanly separated from the core order data.

---

## 📋 Table of Contents
1. [Overview](#-overview)
2. [Table Descriptions](#-table-descriptions)
3. [Relationships](#-relationships)

---

## 🗄️ Table Descriptions

This database consists of 7 normalized tables to ensure data integrity and scalable business operations.

### 1. Customer
Stores the identity and contact information of the buyers. 
* **Key Attributes:** `instagram_handle` is critical here since the business operates via social media DMs.

### 2. Product
Manages the store's inventory, handling both unique and batch items.
* **Key Attributes:** `product_type` differentiates between "Thrift" and "Handmade". `stock_quantity` ensures thrifted items are capped at 1 piece, while handmade items can have larger inventories. `condition` tracks the wear-and-tear of vintage pieces.

### 3. Order
Acts as the central hub for a transaction.
* **Key Attributes:** `order_source` tracks whether the sale originated from an InstaDM or WhatsApp, which helps the creator understand their best sales funnels.

### 4. OrderItem (Junction Table)
Resolves the many-to-many relationship between Orders and Products. If a user buys 3 different shirts, this table gets 3 rows tied to 1 order.
* **Key Attributes:** `item_price` records the price of the item *at the exact time of purchase*, ensuring past receipts remain accurate even if the creator raises the price of the product later.

### 5. Payment
Manages the financial status of an order. Separating this from the Order table keeps the schema clean.
* **Key Attributes:** Tracks the `payment_method` (UPI, Card, COD) and the exact `payment_status` (Pending/Completed).

### 6. Shipping
Manages the physical fulfillment and logistics of an order.
* **Key Attributes:** Stores the `courier_partner`, tracking numbers, and delivery status.

### 7. Review
Captures customer feedback on specific products to build trust and social proof.
* **Key Attributes:** Links to both the user who wrote it and the product being reviewed, storing a 1-5 `rating` and text `comment`.

---

## 🔗 Relationships

The database utilizes the following relationships to connect the entities logically:

* **Customer ↔ Order (1 : N)**
  * One customer can place multiple orders over time.
* **Order ↔ OrderItem (1 : N)**
  * One order can contain multiple individual items.
* **Product ↔ OrderItem (1 : N)**
  * One product can be a part of many different order items (especially handmade ones).
* **Order ↔ Payment (1 : 1)**
  * One specific order has exactly one payment record associated with it.
* **Order ↔ Shipping (1 : 1)**
  * One specific order generates exactly one shipping and tracking record.
* **Customer ↔ Review (1 : N)**
  * One customer can write multiple reviews for different products.
* **Product ↔ Review (1 : N)**
  * One product can receive multiple reviews from different customers.

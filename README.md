# Author: Jabulile Gamede

**Section 2: Data Modeling Theory Exercise**
1. **ER Components**

**Entity:** An entity is a thing in the real world that we want to keep information about.
    Example: A Book is an entity. A Customer is also an entity.

**Relationship:** A relationship is how two entities are connected to each other.
    Example: A Customer PLACES an Order. PLACES is the relationship.

**Attribute:** An attribute is a detail that describes an entity.
    Example: For Book, the attributes are BookID, Title, Price.

2. **Data Model Levels**

**Conceptual Data Model:** This is the high-level model. It shows what data we need and how it is related. It is made by business analysts and users. Its focus is on WHAT data is needed, not how it will be stored. It does not have data types or tables.

**Physical Data Model:** This is the low-level model. It shows how the data will actually be stored in the database. It is made by the DBA / developer. Its focus is on HOW it will be stored, like tables, columns, data types, primary keys and foreign keys.

Difference is that Conceptual = business view, Physical = technical view.

**Section 3:** 

1. **Customer places multiple Orders**

Customer and Order have a 1 to Many relationship because one customer can place many orders, but one order belongs to only one customer.

**Customer:**
Attributes: CustomerID, FirstName, LastName, Email, Phone
CustomerID is the Primary Key

**Order:**
Attributes: OrderID, OrderDate, TotalAmount, CustomerID
OrderID is the Primary Key, CustomerID is the Foreign Key

**Tables:**
Customer(CustomerID, FirstName, LastName, Email, Phone)
Order(OrderID, OrderDate, TotalAmount, CustomerID)

The CustomerID in Order is what links the order back to the customer who placed it.

2. **PostgreSQL Implementation**

postgres=# CREATE DATABASE store_modeling_db;
CREATE DATABASE
postgres=# \c store_modeling_db
psql (16.15, server 18.6)
WARNING: psql major version 16, server major version 18.
         Some psql features might not work.
You are now connected to database "store_modeling_db" as user "postgres".
store_modeling_db=# CREATE TABLE Customer (
store_modeling_db(# CustomerID INT PRIMARY KEY,
store_modeling_db(# FirstName VARCHAR(50),
store_modeling_db(# LastName VARCHAR(50),
store_modeling_db(# Email VARCHAR(100),
store_modeling_db(# Phone VARCHAR(20));
CREATE TABLE
store_modeling_db=# \dt
          List of relations
 Schema |   Name   | Type  |  Owner
--------+----------+-------+----------
 public | customer | table | postgres
(1 row)

store_modeling_db=# CREATE TABLE Orders (
store_modeling_db(#     OrderID INT PRIMARY KEY,
store_modeling_db(#     OrderDate DATE,
store_modeling_db(#     TotalAmount DECIMAL(10,2),
store_modeling_db(#     CustomerID INT,
store_modeling_db(#     FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
store_modeling_db(# );
CREATE TABLE
store_modeling_db=# INSERT INTO Orders (OrderID, OrderDate, TotalAmount, CustomerID)
store_modeling_db-# VALUES
store_modeling_db-# (5001, '2026-11-18', 250.00, 101),
store_modeling_db-# (5002, '2026-11-18', 399.99, 101);
INSERT 0 2
store_modeling_db=# SELECT * FROM Customer
store_modeling_db-# SELECT * FROM Customer;
ERROR:  syntax error at or near "SELECT"
LINE 2: SELECT * FROM Customer;
        ^
store_modeling_db=# SELECT * FROM Orders;
 orderid | orderdate  | totalamount | customerid
---------+------------+-------------+------------
    5001 | 2026-11-18 |      250.00 |        101
    5002 | 2026-11-18 |      399.99 |        101
(2 rows)


store_modeling_db=# SELECT * FROM Customer;
 customerid | firstname | lastname |      email       |   phone
------------+-----------+----------+------------------+------------
        101 | Lindo     | Thabede  | lindo@gmail.com  | 0821234784
        102 | Thando    | Ndebe    | thando@gmail.com | 0763456980
(2 rows)

# 📚 Online Bookstore SQL Project

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-025E8C?style=for-the-badge&logo=amazon-dynamodb&logoColor=white)
![pgAdmin](https://img.shields.io/badge/pgAdmin-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)

> A real-world SQL project simulating an **Online Bookstore** — built with PostgreSQL, covering database design, inventory management, customer orders, and sales analytics.

---

## 📌 Project Overview

This project models the complete backend database of an online bookstore. It manages books, customers, orders, and stock — with SQL queries to track sales performance, popular titles, and revenue insights.

---

## 🗂️ Project Structure

```
Online-Bookstore-SQL-Project/
│
├── 📄 bookstore_schema.sql       # Table definitions & constraints
├── 📄 insert_data.sql            # Sample / seed data
├── 📄 queries.sql                # Business queries & analysis
├── 📊 books.csv                  # Books dataset
├── 📊 customers.csv              # Customer dataset
├── 📊 orders.csv                 # Orders dataset
└── 📝 README.md                  # Project documentation
```

---

## 🗃️ Database Schema

### Tables & Description

| Table        | Description                                            |
|--------------|--------------------------------------------------------|
| `books`      | Book catalog with title, author, genre, price & stock  |
| `customers`  | Registered users who purchase books                    |
| `orders`     | Orders placed by customers                             |
| `order_items`| Line items linking orders to specific books            |
| `stock`      | Inventory tracking for each book                       |

### Key Relationships

```
customers ──< orders >── order_items >── books
books ──── stock
```

---

## ⚙️ How to Run This Project

### Prerequisites
- [PostgreSQL](https://www.postgresql.org/download/) installed
- [pgAdmin](https://www.pgadmin.org/download/) or any SQL client

### Steps

**1. Create the database**
```sql
CREATE DATABASE online_bookstore;
```

**2. Run schema file**
```sql
-- Open bookstore_schema.sql in pgAdmin and execute
```

**3. Import CSV data**
```sql
COPY books FROM 'C:/path/to/books.csv' DELIMITER ',' CSV HEADER;
COPY customers FROM 'C:/path/to/customers.csv' DELIMITER ',' CSV HEADER;
COPY orders FROM 'C:/path/to/orders.csv' DELIMITER ',' CSV HEADER;
```

**4. Run queries**
```sql
-- Open queries.sql in pgAdmin and execute
```

---

## 📊 Sample SQL Queries

### 🔹 Top 5 Best-Selling Books
```sql
SELECT b.title, b.author, SUM(oi.quantity) AS total_sold
FROM books b
JOIN order_items oi ON b.book_id = oi.book_id
GROUP BY b.title, b.author
ORDER BY total_sold DESC
LIMIT 5;
```

### 🔹 Total Revenue per Genre
```sql
SELECT b.genre,
       SUM(oi.quantity * b.price) AS total_revenue
FROM books b
JOIN order_items oi ON b.book_id = oi.book_id
GROUP BY b.genre
ORDER BY total_revenue DESC;
```

### 🔹 Top Spending Customers
```sql
SELECT c.customer_name,
       SUM(oi.quantity * b.price) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN books b ON oi.book_id = b.book_id
GROUP BY c.customer_name
ORDER BY total_spent DESC
LIMIT 5;
```

### 🔹 Low Stock Alert (Books running out)
```sql
SELECT b.title, s.stock_quantity
FROM books b
JOIN stock s ON b.book_id = s.book_id
WHERE s.stock_quantity < 10
ORDER BY s.stock_quantity ASC;
```

### 🔹 Monthly Sales Report
```sql
SELECT TO_CHAR(o.order_date, 'YYYY-MM') AS month,
       COUNT(o.order_id)                AS total_orders,
       SUM(oi.quantity * b.price)       AS monthly_revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN books b ON oi.book_id = b.book_id
GROUP BY TO_CHAR(o.order_date, 'YYYY-MM')
ORDER BY month;
```

### 🔹 Sales View (Reusable)
```sql
CREATE VIEW sales_summary AS
SELECT b.title, b.genre, b.author,
       SUM(oi.quantity)              AS units_sold,
       SUM(oi.quantity * b.price)   AS revenue
FROM books b
JOIN order_items oi ON b.book_id = oi.book_id
GROUP BY b.title, b.genre, b.author;

-- Use it anytime:
SELECT * FROM sales_summary ORDER BY revenue DESC;
```

---

## 💡 SQL Concepts Covered

- ✅ Database design & normalization
- ✅ Primary & Foreign Key constraints
- ✅ `JOIN` — INNER, LEFT, RIGHT
- ✅ Aggregate functions — `SUM`, `COUNT`, `AVG`, `MAX`, `MIN`
- ✅ `GROUP BY` & `HAVING`
- ✅ `ORDER BY` & `LIMIT`
- ✅ Subqueries & nested SELECT
- ✅ Views — `CREATE VIEW`
- ✅ Date & time functions
- ✅ Stock / inventory tracking queries

---

## 🛠️ Tools & Technologies

| Tool         | Purpose                          |
|--------------|----------------------------------|
| PostgreSQL   | Relational database engine       |
| pgAdmin      | GUI for writing & running SQL    |
| CSV Dataset  | Raw data for all tables          |
| GitHub       | Version control & project sharing|

---

## 📈 Key Business Insights This Project Answers

- 📖 Which books sell the most?
- 💰 Which genre generates the highest revenue?
- 👤 Who are the top spending customers?
- 📦 Which books are low on stock?
- 📅 What is the monthly sales trend?

---

## 👤 Author

**Amit Kumar**
- GitHub: [@amit8434](https://github.com/amit8434)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

⭐ **If you found this project helpful, please give it a star!**

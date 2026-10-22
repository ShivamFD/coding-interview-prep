# 🎯 PostgreSQL: DISTINCT ON

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-DISTINCT%20ON-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`DISTINCT ON` is a powerful **PostgreSQL-specific** feature.

It allows you to get the first row of each group based on one or more columns.

### Why is it useful?
- Get the latest order of each customer
- Get the highest salary employee in each department
- Get the most recent login of each user

It is cleaner and often faster than using window functions or subqueries for these use cases.

---

## 🧠 Syntax

```sql
SELECT DISTINCT ON (column1) 
    column1, column2, column3
FROM table_name
ORDER BY column1, column2 DESC;
```

**Important Rules:**
1. The expressions in `DISTINCT ON` must match the leftmost expressions in the `ORDER BY` clause.
2. `ORDER BY` decides which row is picked as the “first” one for each group.

---

## 🗄️ Dataset: `orders` Table

| id | customer  | product     | amount | order_date |
|:--:|:----------|:------------|-------:|:-----------|
| 1  | Rahul     | Laptop      | 55000  | 2024-01-05 |
| 2  | Priya     | Mobile      | 25000  | 2024-01-07 |
| 3  | Rahul     | Mouse       | 800    | 2024-01-10 |
| 4  | Amit      | Keyboard    | 1500   | 2024-01-12 |
| 5  | Priya     | Headphones  | 2000   | 2024-01-15 |
| 6  | Rahul     | Monitor     | 12000  | 2024-01-18 |
| 7  | Amit      | Laptop      | 60000  | 2024-01-20 |
| 8  | Sneha     | Tablet       | 15000  | 2024-01-22 |
| 9  | Priya     | Charger     | 500    | 2024-01-25 |
| 10 | Amit      | Mouse       | 900    | 2024-01-28 |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Get the latest order of each customer

```sql
SELECT DISTINCT ON (customer)
    customer,
    product,
    amount,
    order_date
FROM orders
ORDER BY customer, order_date DESC;
```

---

### 🔹 Task 2: Get the first order of each customer

```sql
SELECT DISTINCT ON (customer)
    customer,
    product,
    amount,
    order_date
FROM orders
ORDER BY customer, order_date ASC;
```

---

### 🔹 Task 3: Get the highest amount order of each customer

```sql
SELECT DISTINCT ON (customer)
    customer,
    product,
    amount,
    order_date
FROM orders
ORDER BY customer, amount DESC;
```

---

### 🔹 Task 4: Get the most expensive product ordered by each customer

```sql
SELECT DISTINCT ON (customer)
    customer,
    product,
    amount
FROM orders
ORDER BY customer, amount DESC;
```

---

### 🔹 Task 5: Using DISTINCT ON with multiple columns

```sql
-- Example: Get the latest order for each customer + product combination
SELECT DISTINCT ON (customer, product)
    customer,
    product,
    amount,
    order_date
FROM orders
ORDER BY customer, product, order_date DESC;
```

---

## 💡 Key Takeaways

- `DISTINCT ON` is unique to PostgreSQL.
- It is one of the cleanest ways to get “first row per group”.
- The `ORDER BY` clause is mandatory and extremely important.
- The leftmost `ORDER BY` expressions must match the `DISTINCT ON` expressions.
- In many cases it is simpler than using `ROW_NUMBER()` window function.

---

## 🔥 Bonus Challenge

Write a query using `DISTINCT ON` that returns the most recent order for each customer, but only for those customers who have placed at least 2 orders.
```

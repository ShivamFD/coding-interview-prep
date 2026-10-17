# 📈 Advanced PostgreSQL: Window Functions (Running Total & More)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Advanced%20Window%20Functions-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In this problem we go beyond basic ranking (`ROW_NUMBER`, `RANK`, `DENSE_RANK`) and explore powerful practical uses of Window Functions:

- Running Total (Cumulative Sum)
- Moving Average
- Comparing current row with previous/next row using `LAG` & `LEAD`
- First & Last value in a partition

These are extremely common in real-world reporting and interviews.

---

## 🗄️ Dataset: `sales` Table

| id | employee   | sale_date  | amount |
|:--:|:-----------|:-----------|-------:|
| 1  | Rahul      | 2024-01-05 | 12000  |
| 2  | Priya      | 2024-01-07 | 8000   |
| 3  | Rahul      | 2024-01-10 | 15000  |
| 4  | Amit       | 2024-01-12 | 9500   |
| 5  | Priya      | 2024-01-15 | 11000  |
| 6  | Rahul      | 2024-01-18 | 7000   |
| 7  | Amit       | 2024-01-20 | 13000  |
| 8  | Priya      | 2024-01-22 | 9000   |
| 9  | Rahul      | 2024-01-25 | 16000  |
| 10 | Amit       | 2024-01-28 | 10500  |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Running Total (Cumulative Sales) for each employee

```sql
SELECT 
    employee,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS running_total
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 2: Moving Average of last 2 sales for each employee

```sql
SELECT 
    employee,
    sale_date,
    amount,
    ROUND(
        AVG(amount) OVER (
            PARTITION BY employee 
            ORDER BY sale_date
            ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
        ), 2
    ) AS moving_avg_2
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 3: Show previous sale amount using LAG()

```sql
SELECT 
    employee,
    sale_date,
    amount,
    LAG(amount, 1) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS previous_sale
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 4: Calculate difference from previous sale

```sql
SELECT 
    employee,
    sale_date,
    amount,
    amount - LAG(amount, 1) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS difference_from_previous
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 5: Show next sale amount using LEAD()

```sql
SELECT 
    employee,
    sale_date,
    amount,
    LEAD(amount, 1) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS next_sale
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 6: First and Last sale amount of each employee

```sql
SELECT 
    employee,
    sale_date,
    amount,
    FIRST_VALUE(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS first_sale,
    LAST_VALUE(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_sale
FROM sales
ORDER BY employee, sale_date;
```

---

## 💡 Key Takeaways

- `SUM() OVER (ORDER BY ...)` creates a running total.
- `ROWS BETWEEN` is used to define a sliding window (for moving average).
- `LAG(column, n)` looks at the previous `n` row.
- `LEAD(column, n)` looks at the next `n` row.
- `FIRST_VALUE` and `LAST_VALUE` are very useful for comparison.
- Always use `PARTITION BY` when you want the calculation reset for each group (e.g. each employee).

---

## 🔥 Bonus Challenge

Write a query that shows for each employee:
- Current sale amount
- Running total
- Percentage of current sale compared to their total sales so far
```

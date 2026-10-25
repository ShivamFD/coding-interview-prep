# 📊 Advanced PostgreSQL: Window Frames (ROWS BETWEEN)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Window%20Frames-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

By default, when you use `ORDER BY` inside a Window Function, the frame is:

```sql
RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```

But we can control the exact set of rows using **Window Frames**.

### Common Frame Clauses

| Frame                                      | Meaning                                      |
|--------------------------------------------|----------------------------------------------|
| `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` | From start to current row               |
| `ROWS BETWEEN N PRECEDING AND CURRENT ROW` | Last N rows + current row                    |
| `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` | Current row to the end                 |
| `ROWS BETWEEN N PRECEDING AND N FOLLOWING` | Sliding window                               |
| `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` | Entire partition                |

This is extremely useful for:
- Moving averages
- Running totals with limited lookback
- Comparing with previous N rows
- Sliding window calculations

---

## 🗄️ Dataset: `sales` Table

| id | employee | sale_date  | amount |
|:--:|:---------|:-----------|-------:|
| 1  | Rahul    | 2024-01-01 | 1000   |
| 2  | Rahul    | 2024-01-02 | 1500   |
| 3  | Rahul    | 2024-01-03 | 1200   |
| 4  | Rahul    | 2024-01-04 | 1800   |
| 5  | Rahul    | 2024-01-05 | 2000   |
| 6  | Priya    | 2024-01-01 | 900    |
| 7  | Priya    | 2024-01-02 | 1100   |
| 8  | Priya    | 2024-01-03 | 1300   |
| 9  | Priya    | 2024-01-04 | 800    |
| 10 | Priya    | 2024-01-05 | 1600   |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Running Total (default frame)

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

### 🔹 Task 2: Moving Average of last 3 days (including current)

```sql
SELECT 
    employee,
    sale_date,
    amount,
    ROUND(
        AVG(amount) OVER (
            PARTITION BY employee 
            ORDER BY sale_date
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ), 2
    ) AS moving_avg_3
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 3: Sum of previous 2 rows + current row

```sql
SELECT 
    employee,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS sum_last_3
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 4: Compare current amount with the next day’s amount

```sql
SELECT 
    employee,
    sale_date,
    amount,
    LEAD(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) AS next_day_amount,
    LEAD(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
    ) - amount AS difference
FROM sales
ORDER BY employee, sale_date;
```

---

### 🔹 Task 5: First and Last value of the entire partition

```sql
SELECT 
    employee,
    sale_date,
    amount,
    FIRST_VALUE(amount) OVER (
        PARTITION BY employee 
        ORDER BY sale_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
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

### 🔹 Task 6: Sliding Window – Average of current row + 1 preceding + 1 following

```sql
SELECT 
    employee,
    sale_date,
    amount,
    ROUND(
        AVG(amount) OVER (
            PARTITION BY employee 
            ORDER BY sale_date
            ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
        ), 2
    ) AS sliding_avg
FROM sales
ORDER BY employee, sale_date;
```

---

## 🧠 Detailed Explanation

- `ROWS BETWEEN` works on **physical rows**.
- `RANGE BETWEEN` works on **logical value ranges** (less commonly used).
- The frame defines which rows are included in the calculation relative to the current row.
- Most useful for time-series data, moving averages, and running metrics.

---

## 💡 Key Takeaways for Interviews

- Default frame is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`.
- Use `ROWS BETWEEN` when you want exact control over number of rows.
- Moving average is one of the most common use cases.
- `FIRST_VALUE` and `LAST_VALUE` often need the full frame:  
  `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`
- Very important for analytics and reporting queries.

---

## 🔥 Bonus Challenge

Write a query that shows for each employee:
- Current day amount
- Moving average of last 3 days
- Running total
- Difference from the previous day
```

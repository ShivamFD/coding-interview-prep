# 🏝️ Advanced PostgreSQL: Gaps and Islands Problem

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Gaps%20%26%20Islands-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

The **Gaps and Islands** problem is a classic advanced SQL pattern.

It deals with identifying:
- **Islands** → consecutive groups of related rows
- **Gaps** → missing values or breaks between groups

### Real-world examples:
- Consecutive login days of a user
- Continuous periods of activity
- Finding missing sequence numbers
- Streaks (e.g., consecutive wins, consecutive working days)

This pattern is frequently asked in advanced SQL interviews.

---

## 🧠 Core Technique

We usually solve it using **Window Functions** with this idea:

1. Use `ROW_NUMBER()` or `LAG()` to detect changes
2. Create a group identifier for consecutive rows
3. Aggregate based on that group

---

## 🗄️ Dataset: `attendance` Table

| id | employee | work_date  |
|:--:|:---------|:-----------|
| 1  | Rahul    | 2024-01-01 |
| 2  | Rahul    | 2024-01-02 |
| 3  | Rahul    | 2024-01-03 |
| 4  | Rahul    | 2024-01-05 |
| 5  | Rahul    | 2024-01-06 |
| 6  | Priya    | 2024-01-01 |
| 7  | Priya    | 2024-01-02 |
| 8  | Priya    | 2024-01-04 |
| 9  | Priya    | 2024-01-05 |
| 10 | Priya    | 2024-01-06 |
| 11 | Amit     | 2024-01-03 |
| 12 | Amit     | 2024-01-04 |
| 13 | Amit     | 2024-01-07 |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Find islands of consecutive working days for each employee

```sql
WITH ordered AS (
    SELECT 
        employee,
        work_date,
        ROW_NUMBER() OVER (PARTITION BY employee ORDER BY work_date) AS rn
    FROM attendance
),
grouped AS (
    SELECT 
        employee,
        work_date,
        work_date - (rn || ' days')::INTERVAL AS grp
    FROM ordered
)
SELECT 
    employee,
    MIN(work_date) AS start_date,
    MAX(work_date) AS end_date,
    COUNT(*) AS consecutive_days
FROM grouped
GROUP BY employee, grp
ORDER BY employee, start_date;
```

---

### 🔹 Task 2: Find the longest streak of consecutive days for each employee

```sql
WITH ordered AS (
    SELECT 
        employee,
        work_date,
        ROW_NUMBER() OVER (PARTITION BY employee ORDER BY work_date) AS rn
    FROM attendance
),
grouped AS (
    SELECT 
        employee,
        work_date,
        work_date - (rn || ' days')::INTERVAL AS grp
    FROM ordered
),
streaks AS (
    SELECT 
        employee,
        COUNT(*) AS streak_length
    FROM grouped
    GROUP BY employee, grp
)
SELECT 
    employee,
    MAX(streak_length) AS longest_streak
FROM streaks
GROUP BY employee
ORDER BY longest_streak DESC;
```

---

### 🔹 Task 3: Find gaps (missing dates) in Rahul’s attendance

```sql
WITH rahul_dates AS (
    SELECT work_date
    FROM attendance
    WHERE employee = 'Rahul'
),
date_series AS (
    SELECT generate_series(
        (SELECT MIN(work_date) FROM rahul_dates),
        (SELECT MAX(work_date) FROM rahul_dates),
        '1 day'::interval
    )::date AS expected_date
)
SELECT expected_date AS missing_date
FROM date_series
WHERE expected_date NOT IN (SELECT work_date FROM rahul_dates)
ORDER BY missing_date;
```

---

### 🔹 Task 4: Using LAG() to detect breaks in sequence

```sql
SELECT 
    employee,
    work_date,
    LAG(work_date) OVER (PARTITION BY employee ORDER BY work_date) AS prev_date,
    work_date - LAG(work_date) OVER (PARTITION BY employee ORDER BY work_date) AS gap_days
FROM attendance
ORDER BY employee, work_date;
```

---

## 🧠 Detailed Explanation

### How the Island technique works:

1. We assign a `ROW_NUMBER()` ordered by date for each employee.
2. We subtract this row number (as days) from the actual date.
3. Consecutive dates will produce the **same group key**.
4. We then group by that key to form islands.

**Example for Rahul:**

| work_date  | rn | work_date - rn | Group |
|------------|----|----------------|-------|
| 2024-01-01 | 1  | 2023-12-31     | A     |
| 2024-01-02 | 2  | 2023-12-31     | A     |
| 2024-01-03 | 3  | 2023-12-31     | A     |
| 2024-01-05 | 4  | 2024-01-01     | B     |
| 2024-01-06 | 5  | 2024-01-01     | B     |

---

## 💡 Key Takeaways for Interviews

- Gaps and Islands is a classic advanced SQL pattern.
- The most common solution uses `ROW_NUMBER()` + date arithmetic.
- `LAG()` / `LEAD()` is useful for detecting gaps.
- `generate_series()` is very powerful for finding missing values.
- This pattern appears in attendance systems, streak calculations, sequence analysis, etc.

---

## 🔥 Bonus Challenge

Write a query that shows for each employee:
- Start date of each island
- End date of each island
- Number of consecutive days
- And only show islands that have 3 or more consecutive days
```

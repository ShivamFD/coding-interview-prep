# GROUP BY in PostgreSQL

**Difficulty:** Easy  
**Topic:** GROUP BY, Aggregate Functions  
**Tags:** #postgresql #sql #groupby #aggregate #interview

## Overview

`GROUP BY` is used to arrange rows that have the same values in one or more columns into groups.  
We mainly use it together with aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) to perform calculations on each group separately.

## Why do we use it?

Without `GROUP BY`, aggregate functions calculate a single result for the entire table.  
With `GROUP BY`, we can answer questions like:

- How many employees are there in each department?
- What is the average salary of each department?
- Which department has the highest total salary?

## How it works

1. Rows are divided into groups based on the column(s) mentioned in `GROUP BY`.
2. Aggregate functions are applied to each group independently.
3. One result row is returned for each group.

## Problem Statement

We have the `employees` table:

| id | name     | department | salary |
|----|----------|------------|--------|
| 1  | Rahul    | IT         | 60000  |
| 2  | Priya    | HR         | 45000  |
| 3  | Amit     | IT         | 75000  |
| 4  | Sneha    | Finance    | 52000  |
| 5  | Vikram   | IT         | 68000  |
| 6  | Neha     | HR         | 48000  |

Write SQL queries for the following:

1. Count the number of employees in each department.
2. Find the average salary of each department.
3. Find the total salary paid to each department.
4. Find the highest salary in each department.
5. Show department name and number of employees, but only for departments that have more than 1 employee.

## Solution

```sql
-- 1. Number of employees in each department
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department;

-- 2. Average salary of each department
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department;

-- 3. Total salary of each department
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department;

-- 4. Highest salary in each department
SELECT department, MAX(salary) AS highest_salary
FROM employees
GROUP BY department;

-- 5. Departments that have more than 1 employee
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

Explanation
GROUP BY department creates separate groups for IT, HR, and Finance.
Aggregate functions then run on each group.
HAVING is used to filter groups (just like WHERE filters rows).
Note: WHERE cannot be used with aggregate results — we must use HAVING.

Key Points
Every column in the SELECT clause that is not an aggregate function must appear in the GROUP BY clause.
WHERE filters rows before grouping.
HAVING filters groups after grouping.
GROUP BY is one of the most frequently asked topics in SQL interviews.

Bonus Challenge
Write a query to display:
Department name
Number of employees
Average salary
Total salary
…for each department, sorted by total salary in descending order.

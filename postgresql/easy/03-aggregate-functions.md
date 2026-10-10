# Aggregate Functions in PostgreSQL

**Difficulty:** Easy  
**Topic:** COUNT, SUM, AVG, MAX, MIN  
**Tags:** #postgresql #sql #aggregate #interview

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

1. Find the total number of employees.
2. Find the total salary of all employees.
3. Find the average salary of all employees.
4. Find the highest salary.
5. Find the lowest salary.
6. Find the number of employees in the **IT** department.
7. Find the average salary of employees in the **HR** department.

## Solution

```sql
-- 1. Total number of employees
SELECT COUNT(*) AS total_employees
FROM employees;

-- 2. Total salary of all employees
SELECT SUM(salary) AS total_salary
FROM employees;

-- 3. Average salary
SELECT AVG(salary) AS average_salary
FROM employees;

-- 4. Highest salary
SELECT MAX(salary) AS highest_salary
FROM employees;

-- 5. Lowest salary
SELECT MIN(salary) AS lowest_salary
FROM employees;

-- 6. Number of employees in IT department
SELECT COUNT(*) AS it_employees
FROM employees
WHERE department = 'IT';

-- 7. Average salary of HR department
SELECT AVG(salary) AS hr_average_salary
FROM employees
WHERE department = 'HR';
```

Explanation
COUNT(*) → counts the number of rows
SUM(column) → adds up all values in the column
AVG(column) → calculates the average
MAX(column) → returns the highest value
MIN(column) → returns the lowest value
You can combine aggregate functions with the WHERE clause to calculate values for a specific group of rows.

Key Points
Aggregate functions ignore NULL values (except COUNT(*)).
We usually give a clear alias using AS (example: AS total_employees).
COUNT(*) counts all rows, while COUNT(column) counts only non-NULL values in that column.

Bonus Challenge
Write a single query that shows:
Total number of employees
Highest salary
Lowest salary
Average salary

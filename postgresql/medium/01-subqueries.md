# Subqueries in PostgreSQL

**Difficulty:** Medium  
**Topic:** Subqueries, Nested Queries  
**Tags:** #postgresql #sql #subquery #nestedquery #interview

## Overview

A **Subquery** (also called inner query or nested query) is a query written inside another query.  
The inner query executes first and its result is used by the outer query.

Subqueries can appear in:
- `SELECT` clause
- `FROM` clause
- `WHERE` clause
- `HAVING` clause

## Why do we use it?

Subqueries help us solve problems that require multiple steps of logic in a single query.

**Common use cases:**
- Find employees who earn more than the average salary
- Find employees who work in the same department as a specific person
- Filter data based on the result of another query

## How it works

1. The **inner query** runs first and returns a result.
2. The **outer query** uses that result to filter or calculate the final output.

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
| 7  | Karan    | Finance    | 81000  |

Write SQL queries for the following:

1. Find all employees who earn **more than the average salary**.
2. Find the employee(s) who earn the **highest salary**.
3. Find all employees who work in the same department as **Amit**.
4. Find departments that have more employees than the **HR** department.
5. Show each employee’s name, salary, and the average salary of their department (using subquery).

## Solution

```sql
-- 1. Employees who earn more than average salary
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- 2. Employee(s) with the highest salary
SELECT name, salary
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);

-- 3. Employees who work in the same department as Amit
SELECT name, department, salary
FROM employees
WHERE department = (
  SELECT department 
  FROM employees 
  WHERE name = 'Amit'
);

-- 4. Departments that have more employees than HR department
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > (
  SELECT COUNT(*) 
  FROM employees 
  WHERE department = 'HR'
);

-- 5. Employee name, salary, and average salary of their department
SELECT 
  name,
  salary,
  (SELECT AVG(salary) 
   FROM employees e2 
   WHERE e2.department = e1.department) AS dept_avg_salary
FROM employees e1;
```
Explanation
In query 1 & 2, the subquery returns a single value (scalar subquery).
In query 3, the subquery returns the department of Amit, which is then used for comparison.
In query 4, we use a subquery inside HAVING to compare group results.
In query 5, we use a correlated subquery (the inner query depends on the outer query).

Key Points
A subquery must be enclosed in parentheses ().
Scalar subquery returns a single value.
Correlated subquery runs once for every row of the outer query.
Subqueries can often be rewritten using JOIN, but both approaches are important to know.

Bonus Challenge
Write a query to find the second highest salary in the table using a subquery.

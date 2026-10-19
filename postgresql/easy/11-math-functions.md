# 🔢 PostgreSQL: Math Functions

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Math%20Functions-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

PostgreSQL provides many useful mathematical functions that are commonly used in reports, calculations, and data analysis.

### Most Important Math Functions

| Function              | Description                              |
|-----------------------|------------------------------------------|
| `ABS(x)`              | Absolute value                           |
| `CEIL(x)` / `CEILING` | Round up to nearest integer              |
| `FLOOR(x)`            | Round down to nearest integer            |
| `ROUND(x, n)`         | Round to n decimal places                |
| `TRUNC(x, n)`         | Truncate to n decimal places             |
| `MOD(x, y)`           | Remainder of x divided by y              |
| `POWER(x, y)`         | x raised to the power of y               |
| `SQRT(x)`             | Square root                              |
| `GREATEST(a, b, ...)` | Returns the largest value                |
| `LEAST(a, b, ...)`    | Returns the smallest value               |

---

## 🗄️ Dataset: `products` Table

| id | product_name     | price   | discount | quantity |
|:--:|:-----------------|--------:|---------:|---------:|
| 1  | Laptop           | 55000   | 10       | 5        |
| 2  | Mobile           | 25000.50| 5        | 12       |
| 3  | Headphones       | 1999.99 | 15       | 30       |
| 4  | Keyboard         | 1499    | 0        | 20       |
| 5  | Monitor          | 18500.75| 8        | 8        |

---

## 🎯 Tasks & Solutions

### 🔹 Task 1: Calculate final price after discount

```sql
SELECT 
    product_name,
    price,
    discount,
    ROUND(price - (price * discount / 100), 2) AS final_price
FROM products;
```

---

### 🔹 Task 2: Round the price to nearest integer

```sql
SELECT 
    product_name,
    price,
    ROUND(price) AS rounded_price
FROM products;
```

---

### 🔹 Task 3: Show floor and ceiling values of price

```sql
SELECT 
    product_name,
    price,
    FLOOR(price) AS floor_price,
    CEIL(price) AS ceil_price
FROM products;
```

---

### 🔹 Task 4: Calculate total value (price × quantity)

```sql
SELECT 
    product_name,
    price,
    quantity,
    ROUND(price * quantity, 2) AS total_value
FROM products;
```

---

### 🔹 Task 5: Find the product with highest and lowest price using GREATEST / LEAST (example with two values)

```sql
SELECT 
    product_name,
    price,
    GREATEST(price, 20000) AS at_least_20k,
    LEAST(price, 20000) AS at_most_20k
FROM products;
```

---

### 🔹 Task 6: Calculate square root of quantity and power of discount

```sql
SELECT 
    product_name,
    quantity,
    ROUND(SQRT(quantity), 2) AS sqrt_quantity,
    POWER(discount, 2) AS discount_squared
FROM products;
```

---

### 🔹 Task 7: Find remainder when quantity is divided by 5

```sql
SELECT 
    product_name,
    quantity,
    MOD(quantity, 5) AS remainder
FROM products;
```

---

## 💡 Key Takeaways

- `ROUND(price, 2)` is very commonly used for currency calculations.
- `FLOOR` always rounds down, `CEIL` always rounds up.
- `ABS` is useful when you want only positive differences.
- `GREATEST` and `LEAST` can take multiple arguments.
- Math functions can be combined with aggregate functions.

---

## 🔥 Bonus Challenge

Write a query that shows:
- Product name
- Original price
- Final price after discount
- Total value (final price × quantity)
- And only show products whose final price is more than 10000
```

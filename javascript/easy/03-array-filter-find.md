# 🧹 JavaScript: Array Filter & Find

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array%20Filter%20%26%20Find-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Two of the most commonly used array methods in JavaScript:

| Method     | Purpose                                      | Returns                  |
|------------|----------------------------------------------|--------------------------|
| `filter()` | Creates a new array with elements that pass a test | New array             |
| `find()`   | Returns the **first** element that passes a test | Element or `undefined` |

---

## 🎯 Problem Statement

Given an array of products, perform the following tasks using `filter` and `find`.

```js
const products = [
  { id: 1, name: "Laptop", price: 55000, category: "Electronics", inStock: true },
  { id: 2, name: "Mobile", price: 25000, category: "Electronics", inStock: true },
  { id: 3, name: "Headphones", price: 2000, category: "Electronics", inStock: false },
  { id: 4, name: "Shirt", price: 1200, category: "Fashion", inStock: true },
  { id: 5, name: "Jeans", price: 2500, category: "Fashion", inStock: true },
  { id: 6, name: "Book", price: 500, category: "Education", inStock: true },
  { id: 7, name: "Watch", price: 8000, category: "Electronics", inStock: false }
];
```

---

## 💻 Solutions

### 🔹 Task 1: Get all products that are in stock

```js
const inStockProducts = products.filter((product) => product.inStock);
console.log(inStockProducts);
```

---

### 🔹 Task 2: Get all Electronics products

```js
const electronics = products.filter((product) => product.category === "Electronics");
console.log(electronics);
```

---

### 🔹 Task 3: Get products cheaper than 3000

```js
const cheapProducts = products.filter((product) => product.price < 3000);
console.log(cheapProducts);
```

---

### 🔹 Task 4: Find the product with id = 4

```js
const product = products.find((p) => p.id === 4);
console.log(product);
```

---

### 🔹 Task 5: Find the first out-of-stock product

```js
const outOfStock = products.find((p) => p.inStock === false);
console.log(outOfStock);
```

---

### 🔹 Task 6: Get names of all in-stock Fashion products

```js
const fashionNames = products
  .filter((p) => p.category === "Fashion" && p.inStock)
  .map((p) => p.name);

console.log(fashionNames); // ["Shirt", "Jeans"]
```

---

## 🧠 Explanation

- `filter` → returns a **new array** containing only the elements that satisfy the condition.
- `find` → returns the **first matching element** (or `undefined` if nothing matches).
- Both methods do **not** modify the original array.
- We can chain `filter` + `map` for powerful data transformations.

---

## 💡 Key Interview Points

- Difference between `filter` and `find`
- Difference between `find` and `findIndex`
- Does `filter` mutate the original array? (No)
- What does `filter` return when no element matches? (Empty array `[]`)
- What does `find` return when no element matches? (`undefined`)
- Common chaining patterns (`filter` → `map`)

---

## 🔥 Bonus Challenges

1. Get the most expensive product using `filter` + sorting or other methods
2. Count how many products are in stock
3. Find all products whose name starts with "S"
```

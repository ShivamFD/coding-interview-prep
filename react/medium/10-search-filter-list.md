# ⚛️ React: Search & Filter List

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Search%20%26%20Filter-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Filtering a list based on user input is one of the most common UI patterns in React applications.

This problem teaches you:
- Controlled input
- Derived state (filtering without extra state)
- Case-insensitive search
- Empty state handling
- Clean list rendering with keys

---

## 🎯 Problem Statement

Create a `ProductSearch` component that:

1. Shows a list of products
2. Has a search input
3. Filters the products in real-time as the user types
4. Shows a message when no products match the search
5. Search should be case-insensitive

---

## 💻 Solution

```jsx
import { useState } from "react";

const products = [
  { id: 1, name: "Laptop", category: "Electronics", price: 55000 },
  { id: 2, name: "Mobile", category: "Electronics", price: 25000 },
  { id: 3, name: "Headphones", category: "Electronics", price: 2000 },
  { id: 4, name: "Shirt", category: "Fashion", price: 1200 },
  { id: 5, name: "Jeans", category: "Fashion", price: 2500 },
  { id: 6, name: "Book", category: "Education", price: 500 },
  { id: 7, name: "Watch", category: "Electronics", price: 8000 },
  { id: 8, name: "Shoes", category: "Fashion", price: 3500 }
];

function ProductSearch() {
  const [searchTerm, setSearchTerm] = useState("");

  // Derived state - no need for extra useState
  const filteredProducts = products.filter((product) =>
    product.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
    product.category.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div style={{ maxWidth: "500px", margin: "40px auto", fontFamily: "sans-serif" }}>
      <h2>Product Search</h2>

      <input
        type="text"
        placeholder="Search by name or category..."
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        style={{
          width: "100%",
          padding: "10px",
          fontSize: "16px",
          marginBottom: "20px",
          boxSizing: "border-box"
        }}
      />

      {filteredProducts.length === 0 ? (
        <p style={{ color: "#888" }}>No products found for "{searchTerm}"</p>
      ) : (
        <ul style={{ listStyle: "none", padding: 0 }}>
          {filteredProducts.map((product) => (
            <li
              key={product.id}
              style={{
                padding: "12px",
                border: "1px solid #ddd",
                borderRadius: "6px",
                marginBottom: "8px",
                display: "flex",
                justifyContent: "space-between"
              }}
            >
              <div>
                <strong>{product.name}</strong>
                <div style={{ fontSize: "14px", color: "#666" }}>
                  {product.category}
                </div>
              </div>
              <div style={{ fontWeight: "bold" }}>
                ₹{product.price.toLocaleString()}
              </div>
            </li>
          ))}
        </ul>
      )}

      <p style={{ marginTop: "15px", color: "#555" }}>
        Showing {filteredProducts.length} of {products.length} products
      </p>
    </div>
  );
}

export default ProductSearch;
```

---

## 🧠 Explanation

1. We keep only one state: `searchTerm`.
2. `filteredProducts` is calculated on every render (derived state). This is the recommended pattern for simple filtering.
3. We use `.toLowerCase()` on both sides to make the search case-insensitive.
4. We search in both `name` and `category`.
5. When the filtered list is empty, we show a friendly message.
6. Always use a unique and stable `key` when rendering lists.

---

## 💡 Key Interview Points

- What is derived state? When should you avoid storing filtered data in state?
- Why is using index as `key` a bad idea here?
- How would you optimize this if the list had thousands of items? (`useMemo`)
- Difference between controlled and uncontrolled input
- How would you add debouncing to the search?

---

## 🔥 Bonus Challenges

1. Add a dropdown to filter by category in addition to the search box
2. Highlight the matching text in the product name
3. Use `useMemo` to memoize the filtered list
4. Clear the search input with a small "×" button
```

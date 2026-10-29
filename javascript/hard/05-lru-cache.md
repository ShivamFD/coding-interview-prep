# 🧠 JavaScript: Implement LRU Cache

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-LRU%20Cache-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**LRU (Least Recently Used) Cache** is a data structure that stores a limited number of items.  
When the cache reaches its capacity, it removes the least recently used item before inserting a new one.

### Common Use Cases:
- Browser cache
- Database query caching
- API response caching
- Memory management in applications

This is a very popular hard-level interview question (asked at FAANG companies).

---

## 🎯 Problem Statement

Design and implement an LRU Cache with the following operations:

- `get(key)` → Return the value if the key exists, otherwise return `-1`. Also mark it as recently used.
- `put(key, value)` → Insert or update the value. If the cache is full, remove the least recently used item first.

Both operations should run in **O(1)** time complexity.

---

## 💻 Solution

We use a combination of:

- `Map` (to store key-value pairs with insertion order)
- The fact that JavaScript `Map` maintains insertion order

```js
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }

  get(key) {
    if (!this.cache.has(key)) {
      return -1;
    }

    const value = this.cache.get(key);

    // Move this key to the end (most recently used)
    this.cache.delete(key);
    this.cache.set(key, value);

    return value;
  }

  put(key, value) {
    // If key already exists, delete it so we can re-insert at the end
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }

    this.cache.set(key, value);

    // If over capacity, remove the least recently used item (first item)
    if (this.cache.size > this.capacity) {
      const leastUsedKey = this.cache.keys().next().value;
      this.cache.delete(leastUsedKey);
    }
  }
}
```

---

### Example Usage

```js
const cache = new LRUCache(3);

cache.put(1, "A");
cache.put(2, "B");
cache.put(3, "C");

console.log(cache.get(1)); // "A"  → now order is 2,3,1

cache.put(4, "D");         // removes key 2 (least recently used)

console.log(cache.get(2)); // -1 (not found)
console.log(cache.get(3)); // "C"
console.log(cache.get(4)); // "D"
```

---

## 🧠 Explanation

1. We use a JavaScript `Map` because it remembers the insertion order.
2. The **first** item in the Map is the least recently used.
3. The **last** item is the most recently used.
4. On `get` or `put` of an existing key, we delete and re-insert it so it moves to the end.
5. When capacity is exceeded, we delete the first key.

This gives us **O(1)** time for both operations.

---

## 💡 Key Interview Points

- Why do we use `Map` instead of a plain object?
- How do we achieve O(1) time complexity?
- Difference between LRU and LFU (Least Frequently Used)
- How would you implement this without using `Map`? (Doubly Linked List + HashMap)
- Real-world applications of LRU Cache

---

## 🔥 Bonus Challenges

1. Implement LRU Cache using a Doubly Linked List + Object (classic interview version)
2. Add a `delete(key)` method
3. Implement LFU Cache
```

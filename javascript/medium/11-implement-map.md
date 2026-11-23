# 🗺️ JavaScript: Implement Array.prototype.map (Polyfill)

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array.map%20Polyfill-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`Array.prototype.map` is one of the most frequently used array methods.

It creates a **new array** by calling a function on every element of the original array.

### Key points about map:
- Does **not** mutate the original array
- Returns a new array of the same length
- The callback receives `(currentValue, index, array)`

Writing a polyfill for `map` is a very common interview question.

---

## 🎯 Problem Statement

Implement your own version of `Array.prototype.map` called `myMap`.

Requirements:
1. It should work exactly like the native `map`
2. It should not mutate the original array
3. The callback should receive `currentValue`, `index`, and the original array
4. Support the `thisArg` parameter (optional)

---

## 💻 Solution

```js
Array.prototype.myMap = function (callback, thisArg) {
  if (typeof callback !== "function") {
    throw new TypeError(callback + " is not a function");
  }

  const result = [];
  const array = this;

  for (let i = 0; i < array.length; i++) {
    // Skip empty slots in sparse arrays
    if (i in array) {
      result.push(callback.call(thisArg, array[i], i, array));
    }
  }

  return result;
};
```

---

### Example Usage

```js
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.myMap((num) => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

const withIndex = numbers.myMap((num, index) => `${index}: ${num}`);
console.log(withIndex); // ["0: 1", "1: 2", "2: 3", "3: 4", "4: 5"]

// Original array remains unchanged
console.log(numbers); // [1, 2, 3, 4, 5]
```

---

### Another Example

```js
const users = [
  { name: "Rahul", age: 28 },
  { name: "Priya", age: 25 },
  { name: "Amit", age: 32 }
];

const names = users.myMap((user) => user.name);
console.log(names); // ["Rahul", "Priya", "Amit"]
```

---

## 🧠 Explanation

1. We first check if the callback is actually a function.
2. We create an empty `result` array.
3. We loop through the original array using a `for` loop.
4. `i in array` check is important for sparse arrays (arrays with empty slots).
5. We use `callback.call(thisArg, ...)` so that the correct `this` value is passed.
6. We push the returned value into the `result` array.
7. Finally we return the new array.

---

## 💡 Key Interview Points

- Does `map` mutate the original array? (No)
- What arguments does the callback receive?
- What is the purpose of the second parameter (`thisArg`)?
- Difference between `map`, `forEach`, and `filter`
- How does `map` handle sparse arrays?
- Can you implement `map` using `reduce`?

---

## 🔥 Bonus Challenges

1. Implement `myMap` using `reduce`
2. Implement `myFilter` polyfill
3. Implement `myForEach` polyfill
4. Write a version that works on array-like objects (e.g. `arguments`)
```

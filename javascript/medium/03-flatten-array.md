# 🧱 JavaScript: Flatten Nested Array

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Flatten%20Array-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Flattening an array means converting a nested (multi-dimensional) array into a single flat array.

**Example:**

```js
Input:  [1, [2, [3, 4], 5], 6]
Output: [1, 2, 3, 4, 5, 6]
```

This is a very common interview question.

---

## 🎯 Problem Statement

Write a function `flattenArray` that:

1. Takes a nested array as input
2. Returns a new flat array
3. Works for any level of nesting
4. Should not use the built-in `Array.flat()` method

---

## 💻 Solutions

### Solution 1: Using Recursion (Most Common)

```js
function flattenArray(arr) {
  const result = [];

  for (let item of arr) {
    if (Array.isArray(item)) {
      result.push(...flattenArray(item)); // recursive call
    } else {
      result.push(item);
    }
  }

  return result;
}
```

---

### Solution 2: Using reduce + Recursion

```js
function flattenArray(arr) {
  return arr.reduce((acc, item) => {
    return acc.concat(Array.isArray(item) ? flattenArray(item) : item);
  }, []);
}
```

---

### Solution 3: Using Stack (Iterative - No Recursion)

```js
function flattenArray(arr) {
  const stack = [...arr];
  const result = [];

  while (stack.length) {
    const item = stack.pop();

    if (Array.isArray(item)) {
      stack.push(...item);
    } else {
      result.push(item);
    }
  }

  return result.reverse(); // because we used pop()
}
```

---

### Example Usage

```js
const nested = [1, [2, [3, [4, 5]], 6], 7, [8, 9]];

console.log(flattenArray(nested));
// Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## 🧠 Explanation

### Recursive Approach
- We loop through each element.
- If the element is an array → call the same function again.
- If it is not an array → push it to the result.
- Spread operator (`...`) is used to merge the nested results.

### Iterative Approach (Stack)
- Useful when the nesting is very deep (to avoid stack overflow).
- We use a stack to process elements.
- Arrays are expanded back into the stack, numbers go into the result.

---

## 💡 Key Interview Points

- Difference between shallow flatten and deep flatten
- Why `Array.flat()` is not allowed in interviews (they want to test recursion)
- How to handle very deep nesting (prefer iterative solution)
- Time and Space complexity
- Can you flatten only up to a certain depth? (like `flat(2)`)

---

## 🔥 Bonus Challenges

1. Write a version that flattens only up to a given depth (e.g., `flatten(arr, 2)`)
2. Flatten an array that contains objects as well (keep objects as-is)
3. Implement your own `Array.prototype.myFlat()`
```

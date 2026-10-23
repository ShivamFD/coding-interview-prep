# 🧬 JavaScript: Deep Clone an Object

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Deep%20Clone-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Cloning** means creating a copy of an object.

There are two types:

| Type          | Description                                      | Nested objects |
|---------------|--------------------------------------------------|----------------|
| Shallow Copy  | Copies only the first level                      | Still referenced |
| Deep Copy     | Creates a completely independent copy            | Fully copied   |

### Why Deep Clone is important?
- Prevents accidental mutation of original object
- Needed when working with nested state in React
- Common interview question

---

## 🎯 Problem Statement

Write a function `deepClone` that:

1. Takes an object (or array) as input
2. Returns a deep copy of it
3. Should work with nested objects and arrays
4. Should not use `JSON.parse(JSON.stringify())` as the main solution (interviewers usually want a recursive solution)

---

## 💻 Solutions

### Solution 1: Recursive Deep Clone (Recommended for Interviews)

```js
function deepClone(obj) {
  // Handle null and non-objects
  if (obj === null || typeof obj !== "object") {
    return obj;
  }

  // Handle Date
  if (obj instanceof Date) {
    return new Date(obj.getTime());
  }

  // Handle Array
  if (Array.isArray(obj)) {
    return obj.map((item) => deepClone(item));
  }

  // Handle Object
  const cloned = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }

  return cloned;
}
```

---

### Solution 2: Using JSON methods (Simple but limited)

```js
function deepCloneJSON(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```

**Limitations of JSON method:**
- Cannot clone functions
- Cannot clone `undefined`
- Cannot clone Date properly (becomes string)
- Cannot handle circular references
- Loses `Symbol` keys

---

### Example Usage

```js
const original = {
  name: "Rahul",
  age: 28,
  skills: ["JavaScript", "React"],
  address: {
    city: "Delhi",
    pin: 110001
  },
  hobbies: [
    { name: "Reading", level: "High" },
    { name: "Gaming", level: "Medium" }
  ]
};

const cloned = deepClone(original);

// Modify nested property
cloned.address.city = "Mumbai";
cloned.skills.push("Node.js");

console.log(original.address.city); // "Delhi" (original not affected)
console.log(cloned.address.city);   // "Mumbai"
console.log(original.skills);       // ["JavaScript", "React"]
console.log(cloned.skills);         // ["JavaScript", "React", "Node.js"]
```

---

## 🧠 Explanation

1. If the value is primitive (`number`, `string`, `boolean`, `null`) → return it directly.
2. If it is an Array → create a new array and recursively clone each item.
3. If it is an Object → create a new object and recursively clone each property.
4. Special handling for `Date` objects is added.

This way we get a completely independent copy.

---

## 💡 Key Interview Points

- Difference between Shallow Copy and Deep Copy
- Why `Object.assign()` and spread operator `{...obj}` only do shallow copy
- Limitations of `JSON.parse(JSON.stringify())`
- How to handle circular references (advanced)
- How to clone Map, Set, Function, etc. (advanced)
- Performance comparison between recursive and JSON method

---

## 🔥 Bonus Challenges

1. Handle circular references in your deepClone function
2. Add support for cloning `Map` and `Set`
3. Write a function that checks whether two objects are deeply equal
```

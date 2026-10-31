# 📦 JavaScript: Object Methods

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Object%20Methods-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

JavaScript provides several useful static methods on the `Object` constructor to work with objects.

### Most Important Object Methods

| Method                    | Description                                      |
|---------------------------|--------------------------------------------------|
| `Object.keys()`           | Returns an array of the object's keys            |
| `Object.values()`         | Returns an array of the object's values          |
| `Object.entries()`        | Returns an array of `[key, value]` pairs         |
| `Object.assign()`         | Copies properties from one object to another     |
| `Object.freeze()`         | Freezes an object (makes it immutable)           |
| `Object.hasOwn()`         | Checks if the object has a specific own property |

---

## 🎯 Problem Statement

Perform various operations on the following object:

```js
const user = {
  name: "Rahul",
  age: 28,
  city: "Delhi",
  role: "Developer"
};
```

---

## 💻 Solutions

### 🔹 Task 1: Get all keys of the object

```js
const keys = Object.keys(user);
console.log(keys); // ["name", "age", "city", "role"]
```

---

### 🔹 Task 2: Get all values of the object

```js
const values = Object.values(user);
console.log(values); // ["Rahul", 28, "Delhi", "Developer"]
```

---

### 🔹 Task 3: Get key-value pairs using Object.entries()

```js
const entries = Object.entries(user);
console.log(entries);
// [["name", "Rahul"], ["age", 28], ["city", "Delhi"], ["role", "Developer"]]
```

---

### 🔹 Task 4: Loop through an object using Object.entries()

```js
Object.entries(user).forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});
```

---

### 🔹 Task 5: Convert object to array of keys and then back

```js
const keysArray = Object.keys(user);
console.log(keysArray);

// Create a new object with only selected keys
const selected = Object.fromEntries(
  Object.entries(user).filter(([key]) => key === "name" || key === "role")
);
console.log(selected); // { name: "Rahul", role: "Developer" }
```

---

### 🔹 Task 6: Merge two objects using Object.assign()

```js
const extraInfo = {
  country: "India",
  experience: 4
};

const merged = Object.assign({}, user, extraInfo);
console.log(merged);
```

---

### 🔹 Task 7: Freeze an object

```js
const frozenUser = Object.freeze({ name: "Priya", age: 25 });

frozenUser.age = 30;          // This will not work
console.log(frozenUser.age);  // 25
```

---

## 💡 Key Takeaways

- `Object.keys()`, `Object.values()`, and `Object.entries()` are the most commonly used methods.
- `Object.entries()` + `forEach` is a clean way to loop over objects.
- `Object.assign()` is useful for merging objects (or use the spread operator `{...obj1, ...obj2}`).
- `Object.freeze()` makes an object immutable (shallow freeze).
- These methods are extremely common in interviews and real projects.

---

## 🔥 Bonus Challenges

1. Write a function that counts the number of properties in an object
2. Write a function that checks whether two objects have the same keys
3. Convert an array of entries back into an object using `Object.fromEntries()`
```

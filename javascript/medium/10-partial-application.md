# 🧩 JavaScript: Partial Application

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Partial%20Application-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Partial Application** is a technique where we create a new function by pre-filling some arguments of an existing function.

It is different from Currying:

| Concept              | Description                                      |
|----------------------|--------------------------------------------------|
| Currying             | Converts a function with multiple arguments into a sequence of unary functions |
| Partial Application  | Pre-fills some arguments and returns a function that accepts the rest |

---

## 🎯 Problem Statement

Implement a function `partial(fn, ...args)` that:

1. Takes a function and some initial arguments
2. Returns a new function
3. When the new function is called, it combines the pre-filled arguments with the new arguments and executes the original function

---

## 💻 Solution

```js
function partial(fn, ...presetArgs) {
  return function (...laterArgs) {
    return fn(...presetArgs, ...laterArgs);
  };
}
```

---

### Example Usage

```js
function greet(greeting, name, punctuation) {
  return `${greeting}, ${name}${punctuation}`;
}

// Pre-fill the first argument
const sayHello = partial(greet, "Hello");

console.log(sayHello("Rahul", "!"));     // Hello, Rahul!
console.log(sayHello("Priya", "!!!"));   // Hello, Priya!!!

// Pre-fill first two arguments
const greetRahul = partial(greet, "Hi", "Rahul");
console.log(greetRahul("."));            // Hi, Rahul.
console.log(greetRahul("!"));            // Hi, Rahul!
```

---

### Real-world Example

```js
function calculatePrice(price, tax, discount) {
  return price + (price * tax) - discount;
}

const calculateWithGST = partial(calculatePrice, undefined, 0.18); 
// This version is limited. Better approach below:

function calculatePrice(price, taxRate, discount = 0) {
  return price + price * taxRate - discount;
}

const withGST = partial(calculatePrice, _, 0.18); // using placeholder concept

// Simpler practical version:
const withGST = (price, discount = 0) => calculatePrice(price, 0.18, discount);

console.log(withGST(1000));        // 1180
console.log(withGST(1000, 50));    // 1130
```

---

## 🧠 Explanation

- We use the rest parameter (`...presetArgs`) to collect the pre-filled arguments.
- The returned function collects the remaining arguments (`...laterArgs`).
- We then call the original function by combining both sets of arguments.
- This is a very common functional programming pattern.

---

## 💡 Key Interview Points

- Difference between Partial Application and Currying
- How does partial application help in code reuse?
- Can we skip arguments (placeholders)?
- Real-world libraries that use partial application (Lodash `_.partial`)
- Relationship with `bind` (because `bind` can also do partial application)

---

## 🔥 Bonus Challenges

1. Implement a version that supports placeholders (e.g., `_`) so you can skip arguments
2. Write a `partialRight` function that pre-fills arguments from the right side
3. Show how `Function.prototype.bind` can be used for partial application
```

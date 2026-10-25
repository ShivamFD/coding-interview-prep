# 🔗 JavaScript: Implement Function.prototype.bind

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Function.prototype.bind-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`Function.prototype.bind` creates a new function that, when called, has its `this` keyword set to a provided value, with a given sequence of arguments preceding any provided when the new function is called.

### Why bind is important?
- Controlling the value of `this`
- Partial application of arguments
- Commonly used in event handlers and React class components (in the past)
- Extremely popular interview question

---

## 🎯 Problem Statement

Implement your own version of `Function.prototype.bind` called `myBind`.

Requirements:
1. It should allow setting the `this` context
2. It should support partial application of arguments
3. The bound function should still work with the `new` keyword (advanced)
4. Do not use the native `.bind()` method

---

## 💻 Solution

```js
Function.prototype.myBind = function (context, ...args) {
  // Store the original function
  const originalFunction = this;

  // Return a new function
  const boundFunction = function (...newArgs) {
    // If called with 'new', use the new object as 'this'
    if (new.target) {
      return new originalFunction(...args, ...newArgs);
    }

    // Otherwise use the provided context
    return originalFunction.apply(context, [...args, ...newArgs]);
  };

  // Maintain prototype chain for 'new' usage
  if (originalFunction.prototype) {
    boundFunction.prototype = Object.create(originalFunction.prototype);
  }

  return boundFunction;
};
```

---

### Example Usage

```js
const person = {
  name: "Rahul"
};

function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

// Basic bind
const greetRahul = greet.myBind(person, "Hello");
console.log(greetRahul("!")); // Hello, Rahul!

// Partial application
const greetHello = greet.myBind(person, "Hello");
console.log(greetHello("!!!")); // Hello, Rahul!!!

// Using with new keyword
function User(name, age) {
  this.name = name;
  this.age = age;
}

const BoundUser = User.myBind(null, "Amit");
const user = new BoundUser(28);

console.log(user.name); // Amit
console.log(user.age);  // 28
```

---

## 🧠 Explanation

1. We save the original function using `this` (the function on which `myBind` is called).
2. We return a new function that will execute the original function later.
3. We use `apply` to set the correct `this` context and pass arguments.
4. We support partial application by combining pre-set arguments (`args`) with new arguments (`newArgs`).
5. Special handling is added for the `new` keyword using `new.target`.

---

## 💡 Key Interview Points

- Difference between `call`, `apply`, and `bind`
- What is partial application?
- Does the bound function create a new function? (Yes)
- How does `bind` handle the `new` keyword?
- Can we bind a bound function again?
- Common use cases of `bind` in real applications
- Polyfill vs native implementation differences

---

## 🔥 Bonus Challenges

1. Implement `myCall` and `myApply` as well
2. Create a version of bind that does not support the `new` keyword (simpler version)
3. Explain soft binding vs hard binding
4. Write a function that checks whether a function has already been bound
```

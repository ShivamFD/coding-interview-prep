# 🔤 JavaScript: String Methods

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-String%20Methods-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

Strings are one of the most commonly used data types in JavaScript.  
JavaScript provides many useful built-in methods to work with strings.

### Most Important String Methods

| Method               | Description                              |
|----------------------|------------------------------------------|
| `length`             | Returns the length of the string         |
| `toUpperCase()`      | Converts to uppercase                    |
| `toLowerCase()`      | Converts to lowercase                    |
| `includes()`         | Checks if a substring exists             |
| `startsWith()`       | Checks if string starts with given value |
| `endsWith()`         | Checks if string ends with given value   |
| `slice()`            | Extracts part of a string                |
| `substring()`        | Similar to slice                         |
| `trim()`             | Removes whitespace from both ends        |
| `split()`            | Splits string into an array              |
| `replace()`          | Replaces a substring                     |
| `indexOf()`          | Returns index of first occurrence        |

---

## 🎯 Problem Statement

Perform various operations on the following string:

```js
const message = "  Hello JavaScript World!  ";
```

---

## 💻 Solutions

### 🔹 Task 1: Get the length of the string

```js
console.log(message.length); // 27 (including spaces)
```

---

### 🔹 Task 2: Remove extra spaces using trim()

```js
const cleaned = message.trim();
console.log(cleaned); // "Hello JavaScript World!"
console.log(cleaned.length); // 23
```

---

### 🔹 Task 3: Convert to uppercase and lowercase

```js
console.log(cleaned.toUpperCase()); // HELLO JAVASCRIPT WORLD!
console.log(cleaned.toLowerCase()); // hello javascript world!
```

---

### 🔹 Task 4: Check if the string includes a word

```js
console.log(cleaned.includes("JavaScript")); // true
console.log(cleaned.includes("Python"));     // false
```

---

### 🔹 Task 5: Check start and end

```js
console.log(cleaned.startsWith("Hello"));  // true
console.log(cleaned.endsWith("World!"));   // true
```

---

### 🔹 Task 6: Extract part of the string using slice

```js
console.log(cleaned.slice(0, 5));   // Hello
console.log(cleaned.slice(6, 16));  // JavaScript
console.log(cleaned.slice(-6));     // World!
```

---

### 🔹 Task 7: Split the string into an array of words

```js
const words = cleaned.split(" ");
console.log(words); // ["Hello", "JavaScript", "World!"]
```

---

### 🔹 Task 8: Replace a word

```js
const newMessage = cleaned.replace("JavaScript", "TypeScript");
console.log(newMessage); // Hello TypeScript World!
```

---

### 🔹 Task 9: Find index of a word

```js
console.log(cleaned.indexOf("JavaScript")); // 6
console.log(cleaned.indexOf("Python"));     // -1 (not found)
```

---

## 💡 Key Takeaways

- Strings are immutable — methods return a new string.
- `trim()` is very useful for cleaning user input.
- `includes()`, `startsWith()`, and `endsWith()` are modern and readable.
- `split()` is commonly used to convert a string into an array.
- `slice()` is preferred over `substring()` in most cases.

---

## 🔥 Bonus Challenges

1. Capitalize the first letter of each word in a sentence
2. Count how many times a specific character appears in a string
3. Check if a string is a palindrome
```

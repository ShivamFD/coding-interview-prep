# 🔁 JavaScript: Implement Array.prototype.reduce

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Array.reduce-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

`Array.prototype.reduce` is one of the most powerful array methods in JavaScript.

It executes a reducer function on each element of the array, resulting in a single output value.

### Common use cases:
- Sum of numbers
- Flattening arrays
- Grouping data
- Counting occurrences
- Building objects

---

## 🎯 Problem Statement

Implement your own version of `Array.prototype.reduce` called `myReduce`.

Requirements:
1. It should work with an initial value
2. It should work without an initial value
3. It should handle empty arrays correctly
4. Do not use the native `.reduce()` method

---

## 💻 Solution

```js
Array.prototype.myReduce = function (callback, initialValue) {
  // Check if the array is empty and no initialValue is provided
  if (this.length === 0 && initialValue === undefined) {
    throw new TypeError("Reduce of empty array with no initial value");
  }

  let accumulator;
  let startIndex;

  if (initialValue !== undefined) {
    accumulator = initialValue;
    startIndex = 0;
  } else {
    accumulator = this[0];
    startIndex = 1;
  }

  for (let i = startIndex; i < this.length; i++) {
    accumulator = callback(accumulator, this[i], i, this);
  }

  return accumulator;
};
```

---

### Example Usage

```js
const numbers = [1, 2, 3, 4, 5];

// Sum
const sum = numbers.myReduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15

// Product
const product = numbers.myReduce((acc, curr) => acc * curr);
console.log(product); // 120

// Flatten array
const nested = [[1, 2], [3, 4], [5]];
const flattened = nested.myReduce((acc, curr) => acc.concat(curr), []);
console.log(flattened); // [1, 2, 3, 4, 5]

// Count occurrences
const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];
const count = fruits.myReduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(count); // { apple: 3, banana: 2, orange: 1 }
```

---

## 🧠 Explanation

1. If no `initialValue` is provided, we use the first element of the array as the accumulator and start looping from index 1.
2. If `initialValue` is provided, we start from index 0.
3. On each iteration, we call the callback with:
   - `accumulator`
   - `currentValue`
   - `currentIndex`
   - `originalArray`
4. We throw an error if the array is empty and no initial value is given (same behavior as native reduce).

---

## 💡 Key Interview Points

- Difference between `reduce` and `reduceRight`
- What happens if no initial value is provided?
- Why is `reduce` so powerful?
- Common interview problems solved with reduce (sum, flatten, groupBy, count)
- Can `reduce` replace `map` and `filter`? (Yes, but less readable)

---

## 🔥 Bonus Challenges

1. Implement `myReduceRight`
2. Write a `groupBy` function using your `myReduce`
3. Implement a function that calculates the total price of items in a cart using reduce
```

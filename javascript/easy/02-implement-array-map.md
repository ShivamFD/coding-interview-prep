# Implement Your Own Array Map

**Difficulty:** Easy  
**Topic:** Array Methods, Higher-Order Functions, Callbacks  
**Tags:** #javascript #array #map #interview

## Problem Statement

Write a function `myMap` that works like the built-in `Array.prototype.map`.

It should:
- Accept an array and a callback function
- Call the callback on every element of the array
- Return a **new array** with the results
- Not modify the original array

Do **not** use the built-in `map` method.

## Example
```

const numbers = [1, 2, 3, 4];

const doubled = myMap(numbers, (num) => num * 2);
console.log(doubled); // [2, 4, 6, 8]

const withIndex = myMap(numbers, (num, index) => num + index);
console.log(withIndex); // [1, 3, 5, 7]

console.log(numbers); // [1, 2, 3, 4] (original array remains unchanged)
```

Approach
Create an empty result array.
Loop through the original array using a for loop.
For each element, call the callback function and pass:
current element
current index
the original array (optional but good to support)
Push the returned value from the callback into the result array.
Return the result array.


Solution
function myMap(array, callback) {
  const result = [];

  for (let i = 0; i < array.length; i++) {
    const value = callback(array[i], i, array);
    result.push(value);
  }

  return result;
}


Explanation
We create a new empty array result so that the original array is not modified.
We use a classic for loop to iterate over every element.
The callback receives three arguments (just like the real map):
current element
current index
the original array
Whatever the callback returns is pushed into the new array.
Finally, we return the new array.
This is exactly how the built-in map works under the hood.
Time & Space Complexity
Time Complexity: O(n) where n is the length of the array
Space Complexity: O(n) because we create a new array of the same size
Edge Cases
Empty array should return an empty array
Callback can return any type of value (number, string, object, etc.)
Original array must remain unchanged
Bonus Challenge (Optional)
Can you also implement myFilter using the same style?

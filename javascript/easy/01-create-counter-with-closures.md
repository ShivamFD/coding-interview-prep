# Create a Counter using Closures

**Difficulty:** Easy  
**Topic:** Closures, Scope, Encapsulation  
**Tags:** #javascript #closures #interview

## Problem Statement

Write a function `createCounter` that takes an optional `initialValue` (default is `0`) and returns an object with the following methods:

- `increment()` → increases the count by 1 and returns the new value
- `decrement()` → decreases the count by 1 and returns the new value
- `reset()` → resets the count to the initial value and returns it
- `getValue()` → returns the current count

The internal `count` variable must be **private** (not accessible from outside). Use **closures** to achieve this.

## Example

```js
const counter = createCounter(5);

console.log(counter.getValue());   // 5
console.log(counter.increment());  // 6
console.log(counter.increment());  // 7
console.log(counter.decrement());  // 6
console.log(counter.reset());      // 5
console.log(counter.getValue());   // 5

Approach
Inside the createCounter function, create a let count variable.
This count variable lives in the outer function's scope.
Return an object that contains 4 methods.
These methods remember the count variable through closure (lexical scope).
Because of this, count cannot be accessed directly from outside — this is a good example of encapsulation.

Solution
function createCounter(initialValue = 0) {
  let count = initialValue;

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    reset() {
      count = initialValue;
      return count;
    },
    getValue() {
      return count;
    }
  };
}

Explanation
let count = initialValue is a private variable.
The methods in the returned object can access this count because they were created in the same lexical environment.
Every time you call createCounter, a new independent closure is created. So multiple counters work separately.
Time & Space Complexity
Time Complexity: O(1) for all operations
Space Complexity: O(1)

Edge Cases
Initial value can also be negative.
You should be able to create multiple counters and use them independently.
reset() always goes back to the original initial value.

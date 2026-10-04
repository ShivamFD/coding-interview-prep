# Create a Counter Component using useState

**Difficulty:** Easy  
**Topic:** React Hooks, useState, Event Handling  
**Tags:** #react #usestate #hooks #interview

## Problem Statement

Create a functional React component called `Counter` that:

- Displays the current count (starting from 0)
- Has three buttons:
  - **Increment** → increases the count by 1
  - **Decrement** → decreases the count by 1
  - **Reset** → sets the count back to 0

Use the `useState` hook. Do not use class components.

## Example

```jsx
// When the component renders:

Count: 0

[Increment]  [Decrement]  [Reset]

After clicking Increment twice and Decrement once:
Count: 1
```


Approach
Import useState from React.
Create a state variable count with initial value 0.
Create three functions (or inline handlers) for increment, decrement, and reset.
Render the current count and the three buttons.
Attach the handlers to the buttons using onClick.


Solution
```
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  const reset = () => {
    setCount(0);
  };

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

export default Counter;

Alternative (using functional updates – recommended)
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(prev => prev + 1)}>Increment</button>
      <button onClick={() => setCount(prev => prev - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

export default Counter;

```
Explanation

useState(0) creates a state variable count and a setter function setCount.
When we call setCount, React re-renders the component with the new value.
Using the functional form setCount(prev => prev + 1) is safer when the new state depends on the previous state.
The component is fully controlled by React state.
Key Points Interviewers Look For
Correct usage of useState
Understanding that state updates trigger re-render
Preference for functional updates (prev => ...)
Clean and readable JSX
Bonus Challenge (Optional)
Add a prop initialValue so the counter can start from any number.
Disable the Decrement button when count is 0.


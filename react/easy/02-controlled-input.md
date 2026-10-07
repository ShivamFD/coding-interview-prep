# Create a Controlled Input Component

**Difficulty:** Easy  
**Topic:** Controlled Components, useState, Forms  
**Tags:** #react #usestate #controlled-component #forms #interview

## Problem Statement

Create a functional React component called `NameInput` that:

- Has an input field where the user can type their name
- Displays the typed name live below the input in this format:  
  **Hello, {name}!**
- Has a **Clear** button that empties the input field
- The input must be a **controlled component** (value is controlled by React state)

## Example


[ Input field ]
Hello, Rahul!
[Clear]
When the user types "Rahul", it should immediately show “Hello, Rahul!”  
When Clear is clicked, the input becomes empty and the greeting disappears or shows “Hello, !”

## Approach

1. Create a state variable to store the input value (using `useState`).
2. Bind the input’s `value` to that state.
3. Use `onChange` to update the state whenever the user types.
4. Create a clear function that sets the state back to an empty string.
5. Conditionally show the greeting only when there is some text.

## Solution

```jsx
import { useState } from "react";

function NameInput() {
  const [name, setName] = useState("");

  const handleChange = (e) => {
    setName(e.target.value);
  };

  const handleClear = () => {
    setName("");
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Enter your name"
        value={name}
        onChange={handleChange}
      />

      <button onClick={handleClear}>Clear</button>

      {name && <h2>Hello, {name}!</h2>}
    </div>
  );
}

export default NameInput;
```


Alternative (shorter version)
```

import { useState } from "react";

function NameInput() {
  const [name, setName] = useState("");

  return (
    <div>
      <input
        type="text"
        placeholder="Enter your name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />

      <button onClick={() => setName("")}>Clear</button>

      {name && <h2>Hello, {name}!</h2>}
    </div>
  );
}

export default NameInput;


```


Explanation
This is a classic controlled component. The value of the input is fully controlled by React state.
Every keystroke updates the state via onChange, which causes a re-render.
Using {name && <h2>...} is conditional rendering — the greeting only appears when name is not empty.
The Clear button simply resets the state to an empty string.

Key Points Interviewers Look For
Understanding of controlled vs uncontrolled components
Correct use of value + onChange
Clean state management
Conditional rendering

Bonus Challenge (Optional)
Show a character count below the input (e.g. “Characters: 5”)
Disable the Clear button when the input is already empty
Convert this into a reusable component that accepts a label prop




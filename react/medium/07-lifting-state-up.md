# ⚛️ React: Lifting State Up

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Lifting%20State%20Up-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

**Lifting State Up** means moving state to the closest common ancestor of components that need to share that state.

Instead of each child managing its own state, the parent manages it and passes data + updater functions down as props.

### When do we lift state up?
- Two or more components need to share the same data
- Sibling components need to communicate
- You want a single source of truth

---

## 🎯 Problem Statement

Create a temperature converter with two inputs:

1. Celsius input
2. Fahrenheit input

Both inputs should stay in sync.  
When one changes, the other should update automatically.

This is a classic example of lifting state up.

---

## 💻 Solution

```jsx
import { useState } from "react";

// Child Component 1
function CelsiusInput({ temperature, onTemperatureChange }) {
  return (
    <div>
      <label>Celsius: </label>
      <input
        type="number"
        value={temperature}
        onChange={(e) => onTemperatureChange(e.target.value)}
      />
    </div>
  );
}

// Child Component 2
function FahrenheitInput({ temperature, onTemperatureChange }) {
  return (
    <div>
      <label>Fahrenheit: </label>
      <input
        type="number"
        value={temperature}
        onChange={(e) => onTemperatureChange(e.target.value)}
      />
    </div>
  );
}

// Parent Component
function TemperatureConverter() {
  const [temperature, setTemperature] = useState("");
  const [scale, setScale] = useState("c"); // 'c' or 'f'

  const toCelsius = (fahrenheit) => ((fahrenheit - 32) * 5) / 9;
  const toFahrenheit = (celsius) => (celsius * 9) / 5 + 32;

  const handleCelsiusChange = (value) => {
    setScale("c");
    setTemperature(value);
  };

  const handleFahrenheitChange = (value) => {
    setScale("f");
    setTemperature(value);
  };

  const celsius =
    scale === "f" ? (temperature === "" ? "" : toCelsius(parseFloat(temperature))) : temperature;

  const fahrenheit =
    scale === "c" ? (temperature === "" ? "" : toFahrenheit(parseFloat(temperature))) : temperature;

  return (
    <div>
      <h2>Temperature Converter</h2>

      <CelsiusInput
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />

      <br />

      <FahrenheitInput
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />

      <p>
        {temperature !== "" && (
          scale === "c"
            ? `${temperature}°C = ${parseFloat(fahrenheit).toFixed(2)}°F`
            : `${temperature}°F = ${parseFloat(celsius).toFixed(2)}°C`
        )}
      </p>
    </div>
  );
}

export default TemperatureConverter;
```

---

## 🧠 Explanation

1. Both inputs need to stay in sync → state cannot live inside individual inputs.
2. We lift the state up to the parent (`TemperatureConverter`).
3. Parent keeps:
   - `temperature` (the value)
   - `scale` (which input was last changed)
4. Parent passes the current value and change handlers down as props.
5. This creates a **single source of truth**.

---

## 💡 Key Interview Points

- What is “single source of truth”?
- Why should we avoid duplicating state?
- When should we lift state up vs using Context?
- Difference between controlled and uncontrolled components
- How does data flow in React? (One-way data flow)

---

## 🔥 Bonus Challenges

1. Add a Kelvin input as well
2. Create a color picker with Hex and RGB inputs that stay in sync
3. Build two sibling counters that can reset each other by lifting state up
```

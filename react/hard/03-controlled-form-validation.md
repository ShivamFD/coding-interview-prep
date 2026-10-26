# ⚛️ React: Controlled Form with Validation

![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Form%20Validation-blue?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Interview%20Prep-success?style=for-the-badge)

---

## 📌 Overview

In React, **Controlled Components** mean that form data is handled by React state.

When we add validation, we usually track:
- Form values
- Errors
- Touched fields
- Form validity

This is a very common real-world interview pattern.

---

## 🎯 Problem Statement

Create a complete **Registration Form** with the following fields:

- Name
- Email
- Password
- Confirm Password

**Validation Rules:**
- Name → required, minimum 3 characters
- Email → required, valid email format
- Password → required, minimum 6 characters
- Confirm Password → must match Password

Show error messages only after the field is touched or form is submitted.

---

## 💻 Solution

```jsx
import { useState } from "react";

function RegistrationForm() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    password: "",
    confirmPassword: ""
  });

  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  const [submitted, setSubmitted] = useState(false);

  const validate = (name, value, allValues = formData) => {
    switch (name) {
      case "name":
        if (!value.trim()) return "Name is required";
        if (value.trim().length < 3) return "Name must be at least 3 characters";
        return "";
      case "email":
        if (!value) return "Email is required";
        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return "Invalid email format";
        return "";
      case "password":
        if (!value) return "Password is required";
        if (value.length < 6) return "Password must be at least 6 characters";
        return "";
      case "confirmPassword":
        if (!value) return "Please confirm your password";
        if (value !== allValues.password) return "Passwords do not match";
        return "";
      default:
        return "";
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;

    const updatedFormData = { ...formData, [name]: value };
    setFormData(updatedFormData);

    // Live validation after the field has been touched
    if (touched[name] || submitted) {
      setErrors((prev) => ({
        ...prev,
        [name]: validate(name, value, updatedFormData)
      }));
    }
  };

  const handleBlur = (e) => {
    const { name, value } = e.target;
    setTouched((prev) => ({ ...prev, [name]: true }));
    setErrors((prev) => ({
      ...prev,
      [name]: validate(name, value)
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setSubmitted(true);

    // Validate all fields
    const newErrors = {};
    Object.keys(formData).forEach((key) => {
      newErrors[key] = validate(key, formData[key]);
    });

    setErrors(newErrors);

    const isValid = Object.values(newErrors).every((err) => err === "");

    if (isValid) {
      alert("Form submitted successfully!\n" + JSON.stringify(formData, null, 2));
      // Reset form if needed
      // setFormData({ name: "", email: "", password: "", confirmPassword: "" });
    }
  };

  return (
    <div style={{ maxWidth: "400px", margin: "40px auto" }}>
      <h2>Registration Form</h2>

      <form onSubmit={handleSubmit}>
        {/* Name */}
        <div style={{ marginBottom: "15px" }}>
          <label>Name</label>
          <input
            type="text"
            name="name"
            value={formData.name}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
          {errors.name && <p style={{ color: "red", margin: "5px 0" }}>{errors.name}</p>}
        </div>

        {/* Email */}
        <div style={{ marginBottom: "15px" }}>
          <label>Email</label>
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
          {errors.email && <p style={{ color: "red", margin: "5px 0" }}>{errors.email}</p>}
        </div>

        {/* Password */}
        <div style={{ marginBottom: "15px" }}>
          <label>Password</label>
          <input
            type="password"
            name="password"
            value={formData.password}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
          {errors.password && <p style={{ color: "red", margin: "5px 0" }}>{errors.password}</p>}
        </div>

        {/* Confirm Password */}
        <div style={{ marginBottom: "15px" }}>
          <label>Confirm Password</label>
          <input
            type="password"
            name="confirmPassword"
            value={formData.confirmPassword}
            onChange={handleChange}
            onBlur={handleBlur}
            style={{ display: "block", width: "100%", padding: "8px" }}
          />
          {errors.confirmPassword && (
            <p style={{ color: "red", margin: "5px 0" }}>{errors.confirmPassword}</p>
          )}
        </div>

        <button type="submit">Register</button>
      </form>
    </div>
  );
}

export default RegistrationForm;
```

---

## 🧠 Explanation

- All inputs are **controlled** via React state.
- We track three things:
  - `formData` → actual values
  - `errors` → validation messages
  - `touched` → whether the user has interacted with the field
- Validation runs on `onBlur` and on submit.
- This pattern is the foundation of most form libraries (Formik, React Hook Form, etc.).

---

## 💡 Key Interview Points

- What are controlled components?
- Difference between controlled and uncontrolled components
- Why do we use `touched` state?
- How would you scale this pattern for large forms?
- Pros & cons of building validation yourself vs using a library
- How does React Hook Form improve this pattern?

---

## 🔥 Bonus Challenges

1. Add a “Show Password” toggle
2. Disable the submit button until the form is valid
3. Add real-time password strength indicator
4. Extract the validation logic into a custom hook (`useForm`)
```


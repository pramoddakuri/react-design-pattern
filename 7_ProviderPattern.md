<h2>Provider Pattern</h2>

Prop drilling.
<img height="700" alt="image" src="https://github.com/user-attachments/assets/8a1b81b2-5f24-407c-9783-da1f7e77d063" />

<h3>Provider & Context</h3>

Steps to create a context
<ul>
  <li>Create a context</li>
  <li>Create a provider</li>
  <li>Wrap the component hierarchy withe provider.</li>
  <li>Create a hook to make the context available.</li>
  <li>Use the data from Context wherever needed.</li>
</ul>

<h4>Create a context</h4>

```jsx
import { createContext } from "react";

const ThemeContext = createContext();
const BrandContext = createContext();

export { ThemeContext, BrandContext };
```

<h4>Create a Provider</h4>

```jsx
import { useState } from 'react';
import PropTypes from 'prop-types';

import { ThemeContext } from '../context';

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState(false);

  const toggleTheme = () => {
    setTheme((prev) => !prev);
    document.body.classList.toggle("dark");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
ThemeProvider.propTypes = {
  children: PropTypes.node.isRequired,
};

export default ThemeProvider;
```

<h4>Wrap the component hierarchy withe provider</h4>

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./index.css";
import BrandProvider from "./provider/BrandProvider.jsx";
import ThemeProvider from "./provider/ThemeProvider.jsx";
ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <ThemeProvider>
            <BrandProvider>
                <App />
            </BrandProvider>
        </ThemeProvider>
    </React.StrictMode>
);
```

<h4>Custom hook to get the values from the context</h4>

```jsx
import { useContext } from "react";
import { ThemeContext } from "../context";

const useTheme = () => {
    const { theme, toggleTheme } = useContext(ThemeContext);

    return { theme, toggleTheme };
};

export { useTheme };
```

<h4>Using the custom hook in application</h4>

```jsx
import { useTheme } from "./hook/useTheme";
import { useBrand } from "./hook/useBrand";

function App() {
    const { theme, toggleTheme } = useTheme();
    const brand = useBrand();

    console.log(brand);

    return (
        <div
            style={{
                backgroundColor: theme ? "#fff" : "#222",
                color: theme ? "#000" : "#fff",
                height: "100vh",
            }}
        >
            <nav className="flex justify-between bg-slate-500 p-1">
                <h1 className="text-3xl">My App</h1>
                <button onClick={toggleTheme}>Toggle Theme</button>
            </nav>
            <main className="p-4 text-center">
                <p className="text-xl m-3">
                    {theme ? "☀️ Light Mode" : "🌙 Dark Mode"}
                </p>
                <div>
                  {brand && <p>{brand.name}</p>}
                </div>
            </main>
        </div>
    );
}

export default App;
```

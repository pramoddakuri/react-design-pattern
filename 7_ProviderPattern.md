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

<h4>Pattern 3</h4>
<h5>Compound Components Pattern</h5>

<h5>What we learn</h5>
<ol>
  <li>Bloated Components with Prop Soup</li>
  <li>The Compound Component Pattern</li>
  <li>Two Exampls</li>
  <li>Use cases</li>
  <li>Pitfall and best practices</li>
</ol>

<h5>Prop Soup</h5>

Below is the code of a dialog.

```jsx

  function Modal({ title, body, primaryAction, secondaryAction }) {
    return (
        <div className="modal-backdrop">
            <div className="modal-container">
                <h2 className="modal-header">{title}</h2>
                <p className="modal-body">{body}</p>
                <div className="modal-footer">
                    {secondaryAction}
                    {primaryAction}
                </div>
            </div>
        </div>
    );
}

export default Modal;

```

in the above component, props are passed and they are rendered. What if a new content or button needs to be show.
Simple answer pass the props from parent and render that.

<h5>Compound Components</h5>

<img width="1500" height="728" alt="image" src="https://github.com/user-attachments/assets/01d1e315-95b6-4037-9128-845d0445e0ee" />

Breaking the above model into blocks

```jsx



function Modal({ children, isOpen, onClose }) {
  if (!isOpen) return null;

  return (
    <div className="modal-backdrop">
      <div className="modal-container">
        {children}
        <button className="modal-close" onClick={onClose}>
          ✖
        </button>
      </div>
    </div>
  );
}

function ModalHeader({ children }) {
  return <div className="modal-header">{children}</div>;
}

function ModalBody({ children }) {
  return <div className="modal-body">{children}</div>;
}

function ModalFooter({ children }) {
  return <div className="modal-footer">{children}</div>;
}

// Attach subcomponents
Modal.Header = ModalHeader;
Modal.Body = ModalBody;
Modal.Footer = ModalFooter;

export default Modal;
```

<h3>React treats functions as Objects</h3>

**If a function is an object, how can it be rendered to the DOM?**

Because **JavaScript functions are both:**

1.  **Callable** (you can execute them)
2.  **Objects** (you can attach properties to them)

React uses the **callable** behavior, not the "object with properties" behavior **when rendering**.
**Key Idea: In React, a component is just a function.**

When React sees:

```jsx
<Modal />
```

it does **not** treat it as an object.

React treats it as a **component** and literally calls it like:

```js
Modal(props);
```

So React doesn’t render the function itself —  
React renders **what the function returns** (React elements).

***

# ✔️ So how can the same function also have properties?

Because functions in JavaScript are special:  
They are **first-class objects with callable behavior**.

Example:

```js
function sayHello() {
  return "Hello!";
}

sayHello.message = "Hi!"; // Adding a property to the function-object
```

Both things are true at the same time:

*   You can call it: `sayHello()`
*   You can read/write properties: `sayHello.message`

Same for your React component:

```js
Modal.Header = ModalHeader;
```

You're adding a property.

But when React renders:

```jsx
<Modal />
```

React does **only this**:

```js
Modal({ isOpen, onClose, children });
```

It doesn’t care about any attached properties like `Modal.Header`.  
Those are just for developer convenience — a pattern called **Compound Components**.

***

# 🔍 **Why React treats a component function differently?**

Because React detects **capitalized function names** as components.

This:

```jsx
<Modal />
```

is compiled by Babel into:

```js
React.createElement(Modal, props);
```

Thus React won't treat it as a plain object — it always calls it.

***

# 🏗️ DOM Rendering Chain (Simple Version)

    <Modal>
       <Modal.Header />
    </Modal>

    ↓ JSX compiled ↓
    React.createElement(Modal)
    React.createElement(Modal.Header)

    ↓ React calls the functions ↓
    Modal(props)
    ModalHeader(props)

    ↓ Functions return React elements ↓
    { type: "div", props: {…} }

    ↓ React reconciler updates virtual DOM ↓

    ↓ React DOM converts to real DOM ↓
    <div class="modal-header">...

At no point does React "render a function object" —  
React renders **the return value of the function.**

***

# 🎉 Summary (Easy Version)

### ✔ JavaScript functions = callable + objects

You can **call** them AND **add properties** to them.

### ✔ React component = just a function

React **calls** the function to get JSX.

### ✔ Compound components

You **attach subcomponents** as properties:

```js
Modal.Header = ModalHeader;
```

Totally allowed, totally normal.

### ✔ When rendering

React ignores the properties and **calls the component function** to get the UI.




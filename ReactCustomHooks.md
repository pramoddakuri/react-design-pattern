<h2>React Custom Hooks</h2>

<h4>What will learn</h4>
<ol>
  <li>Why Hooks</li>
  <li>What is a Hook in React</li>
  <li>Built in Hooks Overview</li>
  <li>Rules of Hook</li>
  <li>Custom Hokoks Pattern</li>
  <li>Real-world Examples</li>
  <li>Use cases and ideas</li>
  <li>Pitfalls</li>
</ol>

<h3>Why Hooks</h3>

<p>React Hooks were introduced in React 16.8 (February 2019). Before that, React did not have Hooks. The primary way to share reusable logic across components was through Higher-Order Components (HOCs), Render Props, and earlier, Mixins (in createClass, later deprecated).</p>
<h5>What existed before Hooks?</h5>
<p>1) Class components + lifecycle methods
Reusable logic was often implemented inside class components using lifecycle methods (componentDidMount, componentDidUpdate, componentWillUnmount) and then abstracted via patterns like HOCs or Render Props.</p>

```jsx

class UserList extends React.Component {
  state = { users: [] };

  componentDidMount() {
    fetch("/api/users").then(res => res.json()).then(users => this.setState({ users }));
  }

  render() {
    return <List data={this.state.users} />;
  }
}
```

<p>2) Higher-Order Components (HOCs)
A HOC is a function that takes a component and returns a new component, injecting props or behavior.</p>

```jsx


function withUserData(Wrapped) {
  return class extends React.Component {
    state = { users: [] };
    componentDidMount() {
      fetch("/api/users").then(res => res.json()).then(users => this.setState({ users }));
    }
    render() {
      return <Wrapped {...this.props} users={this.state.users} />;
    }
  };
}

```

<p>Trade-offs: Can lead to “wrapper hell” (deeply nested component wrappers), name collisions, and harder debugging</p>
<p>3) Render Props
A component that takes a function as a child and calls it to render, passing data in.</p>

```javascript
class UserData extends React.Component {
  state = { users: [] };
  componentDidMount() {
    fetch("/api/users").then(res => res.json()).then(users => this.setState({ users }));
  }
  render() {
    return this.props.children(this.state.users);
  }
}
// Usage
<UserData>
  {users => <List data={users} />}
</UserData>
```
<p>Why Hooks? What problems do they solve?
Hooks provide a first-class way to reuse stateful logic without changing your component hierarchy. Instead of HOCs/render props, you write custom hooks:</p>

```javascript

import { useEffect, useState } from "react";

function useUsers() {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    let isMounted = true;
    fetch("/api/users")
      .then(res => res.json())
      .then(u => isMounted && setUsers(u));
    return () => { isMounted = false; };
  }, []);
  return users;
}

function UserList() {
  const users = useUsers();
  return <List data={users} />;
}
```

<h3>What is Hooks in React</h3>
Hooks look like a simple javascript functions, but they are not normal utility funcitons, they are deeply integrated with Reacts rendering engine.

A normal js funciton are not aware of
<ul>
  <li>Has no knowledge about react</li>
  <li>Cannot store react state</li>
  <li>Cannot trigger re-renders</li>
  <li>Cannot hook into React life cycle</li>
</ul>

A hook like useState() talks directly to React’s internal engine.
It tells React:
<ul>
<li>Hey React, store this state for this component instance.</li>
<li>React, re-render this component when the state changes.</li>
<li>React, run this side-effect after rendering.</li>
</ul>

<h5> Why are they called Hooks?</h5>
<p>Because they “hook into” React’s internal system.
useState() hooks into:<br/>
➡ the component’s state storage<br/>
useEffect() hooks into:<br/>
➡ React’s commit phase (after DOM updates)<br/>
useContext() hooks into:<br/>
➡ the context subscription system<br/>
useMemo() hooks into:<br/>
➡ memoization during re-renders<br/>
Each Hook is a “hook point” into React.</p>


<h3>Built in Hooks</h3>

[Link to react cheat sheet:] (https://www.tapascript.io/books/react-hooks-cheatsheet)

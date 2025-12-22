<h3>Higher Order Component & Function</h3>

<h5>What we learn</h5>
<ol>
  <li>HOF & HOC</li>
  <li>How it help in sharing Logic</li>
  <li>A few real world examples</li>
  <li>Use cases</li>
  <li>Pitfalls and Modern Alternaties</li>
</ol>

<h3>Higher order Function(HOF)</h3>
A higher order function is the one which takes a fucntion as props or returns a function as props, and also can do the both at a time.

```jsx
function withLoggine(fn){
  return function(...args){
    console.log('Calling function with args', args);
    const result = fn(...args);
    console.log('Result', result);
    return result;
  }
}

```

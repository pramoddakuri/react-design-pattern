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
function withLogging(fn){
  return function(...args){
    console.log('Calling function with args', args);
    const result = fn(...args);
    console.log('Result', result);
    return result;
  }
}

```

What This Function Does
It is a higher‑order function that:

Takes a function fn as an argument.
Returns a new function that:

logs the arguments before calling fn
calls the original function
logs the result after calling fn
returns the original result

This is essentially a function wrapper / decorator.

```jsx
function add(a, b) {
  return a + b;
}

const loggedAdd = withLoggine(add);

loggedAdd(2, 3);
// Console:
// Calling function with args [2, 3]
// Result 5

```

<h3>Higher order Components(HOC)</h3>

Lets assume we have a rectangle of color blue, star of color green and cylinder of color yellow, now if we want to transform all of the color to red, what we should do ?

Then we have some set of input components and we need to return some set of output components, but with some enchancements.

<img width="1352" height="853" alt="image" src="https://github.com/user-attachments/assets/7e655e4f-e3b4-4752-a4b2-affcf762158a" />

<h5>Movie App</h5>

Lets assume we have all the data related movies was stored in a database.

<img width="1820" height="925" alt="image" src="https://github.com/user-attachments/assets/7f0f6218-c683-49dd-950b-b87c8ffb8d19" />

lets assume the first api call is to fetch the list of available movies, that are present inside the database. and other api to show movie analytics. While fetching the data we show loading state. If we observe we have some common things in the above process. They are fetching data form the api and showing loading state while fetching the data.





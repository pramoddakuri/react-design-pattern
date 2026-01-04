<h1>Render Props Pattern</h1>

<h5>What we learn</h5>
<ol>
  <li>What render props and why they exist</li>
  <li>How it help in Sharing logic</li>
  <li>A few real world examples</li>
  <li>Use cases</li>
  <li>Pitfalls and modren alternatives</li>
</ol>

<h5>The Problem found</h5>

Imagine we need to build up a system to track bikes and cars, do we need to write tracking mechanism for both bike and car seperate or to some extent same. The answer is to some extent, we can implement the logic that is suitable for both bike and car. Some duplicates the logic, this is the code smell and the problem.

<h5>Messy car Tracker Component</h5>

```jsx

import { useState } from "react";

const CarTracker = () => {
    const [pos, setPos] = useState({ x: 0, y: 0 });

    function handleMouseMove(e) {
        setPos({ x: e.clientX, y: e.clientY });
    }
    return (
        <div
            className="border p-2 w-full h-48 my-2"
            onMouseMove={handleMouseMove}
        >
            <p>
                🚗 Car is at ({pos.x}, {pos.y})
            </p>
        </div>
    );
};

export default CarTracker;
```

Messy Bike Pattern also looks the same.

<h4>The Render Props Pattern</h4>

**A render prop is a prop, that expects a function, that return a JSX.**

Code implentation with pattern

**Parent**
```jsx
import MouseTrackerWithChildren from "./with-pattern/using-children/MouseTrackerWithChildren";

function App() {
    return (
        <div className="flex flex-col items-center m-2">
            {/*<CarTracker />
      <BikeTracker />

            <MouseTracker
                render={(pos) => (
                    <p>
                        🚗 Car is at ({pos.x}, {pos.y})
                    </p>
                )}
            />

             <MouseTracker
                render={({x, y}) => (
                    <p>
                         🏍️ Bike is at ({x}, {y})
                    </p>
                )}
            />*/}

            <MouseTrackerWithChildren>
                {({ x, y }) => (
                    <p>
                        🚗 Car is at ({x}, {y})
                    </p>
                )}
            </MouseTrackerWithChildren>
            
            <MouseTrackerWithChildren>
                {({ x, y }) => (
                    <p>
                        🏍️ Bike is at ({x}, {y})
                    </p>
                )}
            </MouseTrackerWithChildren>
        </div>
    );
}

export default App;
```
**Child**

```jsx
import { useState } from "react";
function MouseTracker({ render }) {
    const [pos, setPos] = useState({ x: 0, y: 0 });

    function handleMouseMove(e) {
        setPos({ x: e.clientX, y: e.clientY });
    }

    return (
        <div
            className="border p-2 w-full h-48 my-2"
            onMouseMove={handleMouseMove}
        >
            {render(pos)}
        </div>
    );
}

export default MouseTracker;
```

<h5>Use Cases</h5>

To expose interanl state to consumer, where they can pass jsx part

Pitfall

It creates new inline function as the rerender happens.

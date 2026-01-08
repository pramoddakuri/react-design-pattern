<h2>UseOptimistic Hook</h2>

<ol>
  <li>What is an Optimistic UI</li>
  <li>React: useOptimistic() Hook</li>
  <li>Anatomy</li>
  <li>A mini porject using useOptimistic</li>
  <li>Error handling and rollback</li>
  <li>Use cases& ideas</li>
  <li>Pit falls and anti patterns</li>
  <li>Tasks</li>
</ol>

<h3>Optimistic UI</h3>

<p>This approach is used to give instant feedback to the user with likely outcome, server request and response will be taken after when the call is done.</p>
<img width="1122" height="317" alt="image" src="https://github.com/user-attachments/assets/65e5ce69-26ad-471f-a72f-8e59ea3b4319" />

<h3>useOptimistic</h3>

```jsx
const [optimisticState, addOptimistic] = useOptimistic(currentState, updateFn)
```

<img width="1241" height="553" alt="image" src="https://github.com/user-attachments/assets/544a62bd-d00c-466f-b89c-2918c2d03e17" />


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

```jsx


import { startTransition, useOptimistic, useState } from "react";

async function sendLikeToServer(postId) {
    // simulate network
    await new Promise((r) => setTimeout(r, 700));

    if (Math.random() < 0.2) throw new Error("Network failed");
    console.log(`Sent a like for the post id ${postId}`);
    return { success: true };
}

export default function LikeButton({ postId, initialLikes = 0 }) {
    // the "real" source of truth for likes (committed)
    const [likes, setLikes] = useState(initialLikes);
    // optimistic state + updater function
    const [optimisticLikes, addOptimisticLike] = useOptimistic(
        likes,
        (currentLikes, delta) => currentLikes + delta
    );

    const handleLike = async () => {
        // 1) Apply optimistic change *immediately*
        addOptimisticLike(1);

        // 2) Start server call in low priority to avoid blocking UI

        try {
            await sendLikeToServer(postId);
            // On success, commit the real state update:
            // IMPORTANT: update the real state so optimistic snapshot eventually matches
            setLikes((prev) => prev + 1);
        } catch (err) {
            // On error, rollback the real state (or trigger a refetch)
            // Because we never incremented likes (real), just leave likes unchanged
            // But we should show an error to user:
            console.error("Like failed:", err);
            // Optionally: show toast or set an error state
            // And — to force the optimistic view to refresh and reflect real state — call setLikes to current value
            setLikes((s) => s); // no-op but will cause optimistic to reflect the committed value
            // Or you can trigger a re-fetch of the post state
        }
    };

    return (
        <div className="flex">
            <button onClick={handleLike}>❤️ {optimisticLikes}</button>
            <button onClick={() => startTransition(async () => handleLike())}>
                ❤️ {optimisticLikes}
            </button>
        </div>
    );
}

```

**What is startTransition in React?**<br>
startTransition is a React 18 API that lets you mark certain state updates as non-urgent (a.k.a. transitions). React will prioritize urgent updates (like typing, clicking a button, focusing an input) and delay or interrupt the transition updates if needed to keep the UI feeling responsive.<br>

Think of it as telling React: “This update can be done in the background. Don’t block the immediate interaction.”

**Why does startTransition exist?**
Before React 18, all state updates were treated equally. If a user typed into an input and that triggered a heavy re-render (e.g., filtering a big list, recalculating derived UI, updating a complex tree), the UI could stutter or lag, because React had to finish rendering before reflecting the keystroke. This led to:

Janky typing experiences
Slow route transitions on large pages
Blocked interactions during heavy computations

React 18 introduced Concurrent Rendering and update priorities. With startTransition, you can mark expensive, non-urgent updates so React can:

Keep urgent updates snappy,
Interrupt long renders when newer urgent updates arrive,
Defer non-urgent work without freezing the UI.

```jsx


function SearchBox({ items }) {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState(items);
  const [isPending, startTransition] = useTransition();

  function onChange(e) {
    const q = e.target.value;
    // Urgent: reflect keystroke immediately
    setQuery(q);

    // Non-urgent: filter results
    startTransition(() => {
      const filtered = items.filter(item => item.includes(q));
      setResults(filtered);
    });
  }

  return (
    <>
      <input value={query} onChange={onChange} />
      {isPending && <small>Updating results…</small>}
      <List items={results} />
    </>
  );
}

```

<h3>Error Handling & Rollback</h3>

```jsx

catch (err) {
            // On error, rollback the real state (or trigger a refetch)
            // Because we never incremented likes (real), just leave likes unchanged
            // But we should show an error to user:
            console.error("Like failed:", err);
            // Optionally: show toast or set an error state
            // And — to force the optimistic view to refresh and reflect real state — call setLikes to current value
            setLikes((s) => s); // no-op but will cause optimistic to reflect the committed value
            // Or you can trigger a re-fetch of the post state
        }
```

<h3>Use Cases</h3>

-Real time interactions likes likes
-posting a msg and sending
-shopping cards add and remove instantli

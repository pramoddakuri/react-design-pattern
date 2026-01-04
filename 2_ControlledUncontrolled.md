<h1>Design Patter 2</h1>
<h3>Controlled vs Uncontrolled Components Pattern</h3>

<h5>What we learn</h5>
  1. Differnce between states and refs in React.</br>
  2. Why not to mixing them in forms.</br>
  3. Controlled and Uncontrolled form.</br>
  4. Pitfalls and best practices.</br>


<h5>State Vs Ref</h5>
<h6>State</h6>
<ol>
  <li>State is React's way of managing dynamic data that drives rendering</li>
  <li>Think state as a source of truth</li>
  <li>When state changes the component rerenders</li>
  <li>State is reactive - the UI always reflects the current state.</li>
</ol>

<h6>Refs</h6>
<ol>
    <li>Refs are used to access DOM elements or React components.</li>
    <li>It allows you to access persistant values without retriggering re-render.</li>
</ol>

<h5>A Messy Feedback Form</h5>

Form which contains both state and refs together is called messy feedback form. Such feedback forms can be braken down into controlled and uncontrolled components.
State is value, where as ref is managing dom level interaction if its required.

In controlled forms, all the data are managed by states, where interactions was managed by ref.

```Javascript

import { useState, useRef } from "react";

export default function ControlledFeedbackForm() {
    const [form, setForm] = useState({ name: "", email: "", message: "" });
    const nameRef = useRef();
    const emailRef = useRef();
    const messageRef = useRef();

    const handleChange = (e) => {
        const { name, value } = e.target;
        setForm({ ...form, [name]: value });
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        if (!form.name) {
            nameRef.current.focus();
            return;
        }
        if (!form.email.includes("@")) {
            emailRef.current.focus();
            return;
        }
        if (!form.message) {
            messageRef.current.focus();
            return;
        }
        console.log("Form submitted:", form);
    };

    return (
        <form className="flex flex-col" onSubmit={handleSubmit}>
            <input
                className="border rounded-2xl p-2 my-3"
                name="name"
                type="text"
                ref={nameRef}
                value={form.name}
                onChange={handleChange}
                placeholder="Name"
            />
            <input
                className="border rounded-2xl p-2 my-3"
                name="email"
                type="email"
                ref={emailRef}
                value={form.email}
                onChange={handleChange}
                placeholder="Email"
            />
            <textarea
                className="border rounded-2xl p-2 my-3"
                name="message"
                ref={messageRef}
                value={form.message}
                onChange={handleChange}
                placeholder="Your message"
            />
            <button
                className="bg-purple-500 text-white p-1 rounded"
                type="submit"
            >
                Send Feedback
            </button>
        </form>
    );
}
```

Above form represents controlled form.

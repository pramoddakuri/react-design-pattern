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

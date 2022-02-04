# crbug-talkback-nested-focusable

## TalkBack: should not read content if focusables are nested

Tested on Chrome 97.0.4692.98 + TalkBack 12.1 and Chrome 100.0.4867.0 + TalkBack 12.1.

### Background

When selecting a focusable element, it should only read its accessible name (such as `aria-label`), but not its content.

### Findings

With the following element:

```html
<div aria-label="Title" tabindex="0">
  <p>First paragraph</p>
  <p>Last paragraph</p>
</div>
```

From the top of the document, swiping right, it will read:

- "Title"
- "First paragraph"
- "Last paragraph"

This is correct behavior.

However, when a focusable element is nested inside:

```html
<div aria-label="Title" tabindex="0">
  <p>First paragraph</p>
  <p>Last paragraph</p>
  <button>Hello</button>
</div>
```

From the top of the document, swiping right, it will read:

- "Title, **First paragraph, Last paragraph**" (it should not narrate the content)
- "First paragraph"
- "Last paragraph"
- "Hello button, double tap to active"

### Additional information

The only way to workaround it is to make it not focusable by any means, such as `<button disabled>`, `<button hidden>`, `<button style="display: none;">`. However, in many cases, this is not a feasible workaround.

Things tested **not** working as a workaround:

```html
<button
  aria-hidden="true"
  role="presentation none"
  style="pointer-events: none;"
  tabindex="-1"
>
```

And

```html
<div
  aria-hidden="true"
  role="presentation none"
>
  <button>Hello</button>
</div>
```

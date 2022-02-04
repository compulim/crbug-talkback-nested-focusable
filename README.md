# crbug-talkback-nested-focusable

## Chrome + TalkBack is inconsistent when reading nested focusables and read the content twice

Tested on Chrome 97.0.4692.98 + TalkBack 12.1 and Chrome 100.0.4867.0 + TalkBack 12.1.

You can test this on our [GitHub Pages](https://compulim.github.io/crbug-talkback-nested-focusable/).

With this HTML code:

```html
<article aria-label="Title" tabindex="0">
  <p>First paragraph</p>
  <p>Last paragraph</p>
</article>
```

On every swipe right gesture on TalkBack, it will read each bullet:

1. "Title", article

This is our observation and serve as our baseline.

With similar HTML code with a nested focusable:

```html
<article aria-label="Title" tabindex="0">
  <p>First paragraph</p>
  <p>Last paragraph</p>
  <button type="button">Click me</button>
</article>
```

On every swipe right gesture on TalkBack, it will read each bullet:

1. "Title", article, <ins>"First paragraph", "Last paragraph"</ins></li>
1. <ins>"First paragraph"</ins></li>
1. <ins>"Last paragraph"</ins></li>
1. "Click me", button, double tap to activate</li>

Note:

- 1st swipe will read the element content, along with the `aria-label`
   - Since `aria-label` present, TalkBack should not read the element content
- 2nd and 3rd swipe will read the element content
   - This is inconsistent with the former case

With nested focusable, TalkBack will read the content twice (1st point, and then, 2nd/3rd points).

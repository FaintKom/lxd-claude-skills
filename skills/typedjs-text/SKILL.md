---
name: typedjs-text
description: Typed.js is a typewriter/typing text animation library. Use when building animated hero headlines, rotating phrases, or terminal-style typing effects that type and backspace strings.
---

# Typed.js — Typewriter Text Animation

You are integrating Typed.js, a small (~14KB) dependency-free library that animates text as if it's being typed and backspaced, character by character. It powers hero headlines, rotating taglines, fake terminal output, and animated input placeholders.

## When to use
- Animated hero headline that types "We build **apps**" then swaps "apps" for "websites", "tools"…
- Rotating list of phrases that type/backspace in a loop.
- Terminal/CLI typing effect with pauses (`^1000`) and inline HTML.
- Animated placeholder/value on an `<input>` via the `attr` option.

## When NOT to use
- General text reveal, per-letter stagger, or word-by-word entrance animations → use **Splitting.js + GSAP** (or SplitText). Typed.js only does the typewriter metaphor.
- Static text styled to *look* typed → just use CSS.
- Large bodies of body copy — typing is slow and hurts UX/accessibility.

## Quick Start

```bash
npm i typed.js
```

```js
import Typed from 'typed.js';

const typed = new Typed('#el', {
  strings: ['First sentence.', 'Second sentence.'],
  typeSpeed: 50,
  backSpeed: 30,
  loop: true,
});
```

```html
<span id="el"></span>
```

CDN alternative (no build step):

```html
<script src="https://unpkg.com/typed.js@3.0.0/dist/typed.umd.js"></script>
```

## Core Patterns

### 1. Common options
```js
new Typed('#el', {
  strings: ['Design.', 'Develop.', 'Deploy.'],
  typeSpeed: 50,      // ms per char typed
  backSpeed: 25,      // ms per char deleted
  backDelay: 1500,    // pause (ms) before backspacing a finished string
  startDelay: 300,    // delay (ms) before the first character
  loop: true,         // repeat forever (loopCount: 3 to cap it)
  smartBackspace: true, // only delete the part that differs between strings
  showCursor: true,
  cursorChar: '|',    // try '_' or '▋'
  shuffle: false,     // randomize string order
});
```

### 2. Inline HTML + mid-string pauses
Strings accept HTML, and `^ms` inserts a pause anywhere in a string.
```js
new Typed('#el', {
  strings: [
    'We build <strong>apps</strong>.',
    'Loading ^1000 done.',          // waits 1000ms mid-string
    'Tiny pause ^500 here.',        // also works: ^500
    '<i>type</i> &amp; backspace',  // escape & as &amp;
  ],
  typeSpeed: 40,
  contentType: 'html', // default; set to null for plain text (no HTML parsing)
});
```
Use `\n` for newlines (e.g. fake terminals). Escape literal `<`, `>`, `&`, and quotes in your source so they don't break the markup.

### 3. SEO/accessible strings from hidden HTML (`stringsElement`)
Keep the real text in the DOM (crawlable, readable) instead of a JS array.
```html
<div id="typed-strings" style="display:none">
  <p>Typed.js is a <strong>JavaScript</strong> library.</p>
  <p>It <em>types</em> out sentences.</p>
</div>
<span id="el"></span>
```
```js
new Typed('#el', { stringsElement: '#typed-strings', typeSpeed: 50 });
```

### 4. Callbacks
```js
new Typed('#el', {
  strings: ['One', 'Two', 'Three'],
  onBegin: (self) => console.log('starting'),
  preStringTyped: (arrayPos, self) => {},   // before each string
  onStringTyped: (arrayPos, self) => {},    // after each string
  onComplete: (self) => self.cursor.remove(), // all strings done (non-loop)
});
```
Other hooks: `onTypingPaused`, `onTypingResumed`, `onReset`, `onStop`, `onStart`, `onLastStringBackspaced`, `onDestroy`.

### 5. Runtime control
```js
const typed = new Typed('#el', { strings: ['Hello'], typeSpeed: 50 });

typed.toggle();  // pause / resume
typed.stop();    // halt
typed.start();   // resume after stop
typed.reset();   // back to the first string, restart
typed.destroy(); // tear down + remove listeners (call on unmount)
```

### 6. Fade out instead of backspacing
```js
new Typed('#el', {
  strings: ['First', 'Second'],
  loop: true,
  fadeOut: true,
  fadeOutClass: 'typed-fade-out', // CSS class applied during fade
  fadeOutDelay: 500,
});
```

### 7. Animate an input's placeholder/value
```js
new Typed('#search', {
  strings: ['Search docs…', 'Search components…'],
  attr: 'placeholder',         // or 'value'
  bindInputFocusEvents: true,  // pause typing while the user focuses the input
  typeSpeed: 60,
  loop: true,
});
```

### 8. Custom cursor via CSS
Typed.js auto-injects default cursor CSS (`autoInsertCss: true`). Override the class:
```css
.typed-cursor {
  color: #06f;
  font-weight: 100;
  opacity: 1;
  animation: typedBlink 0.7s infinite;
}
@keyframes typedBlink { 50% { opacity: 0; } }
.typed-fade-out { opacity: 0; transition: opacity 0.25s; }
```

## Gotchas & Perf
- **Escape HTML and quotes** inside `strings`. Use `&amp;` for `&`, and watch nested quotes — they silently break the markup or the JS array.
- **Always `destroy()` on teardown.** In React, return the cleanup from `useEffect`:
  ```jsx
  useEffect(() => {
    const typed = new Typed(elRef.current, { strings: ['Hi'], typeSpeed: 50 });
    return () => typed.destroy(); // prevents duplicate cursors / memory leaks
  }, []);
  ```
  Run the effect once (empty deps); re-running without `destroy()` stacks multiple instances on the same node.
- **Cursor styling is CSS-only** (`.typed-cursor`). Don't try to style it inline via options beyond `cursorChar`.
- **Accessibility:** screen readers announce every keystroke mutation, which is noisy. Provide a static, readable fallback and hide the animated node from assistive tech:
  ```html
  <span class="sr-only">We build apps, websites, and tools.</span>
  <span id="el" aria-hidden="true"></span>
  ```
- **Respect `prefers-reduced-motion`:** skip the animation and render final text for users who opt out.
  ```js
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelector('#el').textContent = 'We build apps.';
  } else {
    new Typed('#el', { strings: ['We build apps.'], typeSpeed: 50 });
  }
  ```
- **Don't over-loop long strings** — `typeSpeed`/`backSpeed` of 0 is instant; keep individual strings short for snappy hero text.

## Resources
- Repo: https://github.com/mattboldt/typed.js
- Customization / full options docs: https://github.com/mattboldt/typed.js#customization

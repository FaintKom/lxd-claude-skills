---
name: splittingjs-text
description: Splitting.js splits text into chars/words/lines exposing index CSS vars for staggered animation. Use for per-letter/word reveals, staggered char animations driven by CSS or GSAP.
---

# Splitting.js — Text Splitting for Staggered Animation

## Role

You are wiring up **Splitting.js** to break text (or grids/images) into individually addressable spans, each tagged with index CSS custom properties (`--char-index`, `--word-index`, `--line-index`, totals, and percents). You then drive the actual motion with CSS transitions/keyframes or a JS animation engine (GSAP, anime.js, Motion).

**Splitting only splits + exposes data.** It writes `<span>` wrappers and CSS vars. It does NOT animate anything. You own the animation layer.

### When to use
- Per-letter or per-word text reveal / entrance effects.
- Staggered character animations where each char's delay derives from its index.
- Line-by-line reveals, grid/cell image effects, shuffle/wave text.
- Pairs perfectly with **GSAP** `stagger` — split with Splitting, animate `result.chars` with GSAP.

### When NOT to use
- You need an animation engine — Splitting is not one. Use GSAP/anime/Motion (or pure CSS) on top.
- Plain text that doesn't animate per-glyph — don't pay the DOM cost.
- Long body copy / paragraphs of text — hundreds of spans hurt layout and a11y. Reserve for headings and short hero lines.
- Frequently-changing live text (counters, input mirrors) — re-splitting on every change is expensive.

## Quick Start

```bash
npm install splitting
```

```js
import "splitting/dist/splitting.css"; // base styles: positions spans, sets vars
import Splitting from "splitting";

// HTML: <h1 data-splitting>Hello</h1>
Splitting(); // splits every [data-splitting] element by chars (default)
```

`Splitting()` finds `[data-splitting]`, wraps each char in `<span class="char">` (and each word in `<span class="word">`, since `chars` runs `words` as a dependency to prevent wrap glitches), and sets per-element vars like `--char-total`/`--word-total` plus per-span `--char-index`/`--word-index`. It returns an array of result objects.

```html
<!-- CDN alternative -->
<link rel="stylesheet" href="https://unpkg.com/splitting/dist/splitting.css" />
<link rel="stylesheet" href="https://unpkg.com/splitting/dist/splitting-cells.css" />
<script src="https://unpkg.com/splitting/dist/splitting.min.js"></script>
```

## Core Patterns

### 1. Basic chars split + CSS stagger
Use `calc()` against `--char-index` to derive each char's `transition-delay`.

```html
<h1 data-splitting class="reveal">Splitting</h1>
```
```css
.reveal .char {
  display: inline-block;
  opacity: 0;
  transform: translateY(0.5em);
  transition: opacity 0.4s, transform 0.4s;
  /* each char fires 40ms after the previous */
  transition-delay: calc(var(--char-index) * 40ms);
}
.reveal.is-in .char { opacity: 1; transform: none; }
```
```js
Splitting();
document.querySelector(".reveal").classList.add("is-in");
```

### 2. Split by words or lines
`chars` is the default; pass `by` for other granularities. `lines` depends on `words`.

```js
Splitting({ target: "[data-words]", by: "words" }); // .word spans + --word-index
Splitting({ target: "[data-lines]", by: "lines" }); // result.lines is a 2D array of words per line
```
```css
[data-lines] .word { transition-delay: calc(var(--line-index) * 120ms); }
```

### 3. Programmatic / targeted split
Pass explicit `target` (selector or element) and `by`. Returns results array.

```js
const [result] = Splitting({ target: document.querySelector(".title"), by: "chars" });
// result.el, result.chars (Array<span>), result.words, result.lines
```

### 4. Animate with GSAP (recommended for control)
Split first, then hand the span array to GSAP's `stagger`.

```js
import { gsap } from "gsap";
const [result] = Splitting({ target: ".hero h1", by: "chars" });

gsap.from(result.chars, {
  yPercent: 100,
  opacity: 0,
  duration: 0.6,
  ease: "back.out(1.7)",
  stagger: 0.03,            // per-char delay; or stagger: { from: "center", each: 0.03 }
});
```
GSAP needs the `.char` spans to wrap motion cleanly — set `.char { display: inline-block; }` and often `overflow: hidden` on the word/line for a mask reveal.

### 5. Reveal on scroll (IntersectionObserver or ScrollTrigger)
```js
Splitting();
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) {
      e.target.classList.add("is-in"); // CSS handles the staggered transition
      io.unobserve(e.target);
    }
  });
}, { threshold: 0.4 });
document.querySelectorAll(".reveal").forEach((el) => io.observe(el));
```
With GSAP: `ScrollTrigger.create({ trigger: el, onEnter: () => gsap.from(result.chars, { ... }) })`.

### 6. Grid / cells split for image effects
Requires the cells stylesheet. Splits an element into a grid for tile/reveal effects.

```js
import "splitting/dist/splitting-cells.css";
// <div data-splitting="cells" data-rows="4" data-cols="6" style="background-image:url(...)"></div>
Splitting({ target: "[data-splitting='cells']", by: "cells", rows: 4, cols: 6 });
// result.cells = flat array; result.rows / result.cols = 2D arrays
```
```css
.cell { transition-delay: calc((var(--row-index) + var(--col-index)) * 60ms); }
```

### 7. Multiple plugins on one element (key)
Use `key` to namespace vars and avoid collisions when splitting the same element more than once.

```js
Splitting({ target: ".title", by: "chars", key: "a" }); // vars become --a-char-index, etc.
```
Custom plugin note: Splitting exposes `Splitting.add({...})` to register your own splitter plugin — rarely needed; prefer composing existing `by` values.

### 8. Re-split on content change
Splitting wraps the existing DOM; it does NOT track changes. If text changes, restore originals first, then re-run.

```js
// Splitting.html() returns split markup as a string without touching the live DOM tree:
el.innerHTML = Splitting.html({ content: newText, by: "chars" });
// Or: cache original text, reset el.textContent = original, then Splitting() again.
```

## Gotchas & Perf

- **Run after fonts + content are ready.** Splitting measures and wraps the current DOM. Call inside `document.fonts.ready.then(Splitting)` or after the text is in place — otherwise web-font reflow can break line splits.
- **DOM cost is real.** Each char becomes a `<span>` (plus a word wrapper). A 40-char heading is ~80 nodes. Splitting `lines` re-measures. Keep it to headings/short lines; never split paragraphs.
- **Accessibility:** the original text content is preserved as the spans' text, and Splitting sets `aria` so screen readers still read the whole word/phrase, not letter-by-letter. Don't strip the wrappers' text. Avoid splitting interactive/labelled content.
- **`prefers-reduced-motion` guard** — always provide a no-motion path:
  ```css
  @media (prefers-reduced-motion: reduce) {
    .reveal .char { transition: none; opacity: 1; transform: none; }
  }
  ```
  For GSAP, gate the tween: `if (!matchMedia("(prefers-reduced-motion: reduce)").matches) gsap.from(...)`.
- **Re-running duplicates spans.** Calling `Splitting()` twice on the same element nests `.char` inside `.char`. Reset to the original text (or use `Splitting.html()` off-DOM) before re-splitting.
- **`whitespace` option** (chars plugin) controls whether spaces count toward `--char-index`; enable it if your stagger math expects spaces to occupy an index.
- **`display: inline-block`** on `.char`/`.word` is almost always required for `transform` to apply — the base CSS provides positioning but verify against your reset/normalize.

## Resources

- Repo: https://github.com/shshaw/Splitting
- Docs: https://splitting.js.org/
- Guide (options, plugins, vars): https://splitting.js.org/guide.html

---
name: vivus-svg-drawing
description: Vivus animates SVG stroke paths to create a self-drawing/handwriting reveal effect. Use when building animated logos, signatures, line-art reveals, or icon draw-on intros.
---

# Vivus — SVG Line-Drawing Animation

You are integrating **Vivus**, a tiny dependency-free library that "draws" SVG paths by animating their `stroke-dasharray`/`stroke-dashoffset`, producing a handwriting / self-drawing reveal.

## When to use
- Logo or signature that draws itself on load or on scroll.
- Line-art / icon "draw-on" intros.
- Sequential stroke reveals (one path after another).

## When NOT to use
- You need **fills, color, opacity, scale, or position** animated → use GSAP, anime.js, or Motion. Vivus only animates strokes drawing in.
- Your art has no stroked outline paths (e.g. flat filled shapes, `<text>`, raster `<image>`) → Vivus cannot draw it. Convert text to outlined paths first.
- The asset is loaded as `<img src="...svg">` → Vivus needs DOM access to the paths (see Gotchas).

## Quick Start

```bash
npm i vivus
```

```html
<svg id="my-svg" viewBox="0 0 200 100">
  <path d="M10 50 H190" fill="none" stroke="#000" stroke-width="2"/>
</svg>
```

```js
import Vivus from 'vivus';

new Vivus('my-svg', { type: 'delayed', duration: 200 });
```

`duration` is measured in **frames** (~60fps), not milliseconds. `200` ≈ 3.3s.

## Core Patterns

### 1. Options & draw styles
```js
new Vivus('my-svg', {
  type: 'oneByOne',          // delayed | sync | oneByOne | scenario | scenario-sync
  duration: 200,             // total frames
  delay: 20,                 // frames between first/last path (delayed/oneByOne)
  animTimingFunction: Vivus.EASE_OUT,   // easing across whole SVG
  pathTimingFunction: Vivus.LINEAR,     // easing within each path
  start: 'inViewport'        // inViewport | manual | autostart
});
```
- `delayed` (default): all paths draw together, slightly staggered.
- `sync`: all paths draw simultaneously.
- `oneByOne`: each path completes before the next starts.
- `scenario` / `scenario-sync`: per-path timing from `data-*` attributes (pattern 5).

### 2. Finish callback
Third constructor arg fires when the drawing completes.
```js
new Vivus('my-svg', { duration: 150 }, (obj) => {
  console.log('done', obj.getStatus()); // 'end'
  obj.el.classList.add('drawn');        // e.g. now fade in fills via CSS
});
```

### 3. Manual control: play / stop / reset / finish
```js
const v = new Vivus('my-svg', { start: 'manual', duration: 200 });

v.play();        // play forward at normal speed
v.play(2);       // play at 2x speed
v.stop();        // pause at current frame
v.reset();       // jump back to empty (nothing drawn)
v.finish();      // jump to fully drawn
v.getStatus();   // 'start' | 'progress' | 'end'
```
Methods chain: `v.stop().reset().play(2);`

### 4. Reverse (erase) with play(-1)
A negative speed plays backwards, "un-drawing" the strokes.
```js
const v = new Vivus('my-svg', { start: 'manual', duration: 120 });
v.finish();      // start fully drawn
button.onclick = () => v.play(-1);   // erase
// re-draw: v.play(1)
```

### 5. Scenario — custom per-path timing
Use `type: 'scenario'` (async) or `'scenario-sync'` and annotate paths with
`data-start` / `data-duration` / `data-delay` (in frames).
```html
<svg id="story">
  <path d="..." data-start="0"   data-duration="20" fill="none" stroke="#000"/>
  <path d="..." data-start="20"  data-duration="40" fill="none" stroke="#000"/>
  <path d="..." data-delay="10"  data-async         fill="none" stroke="#000"/>
</svg>
```
```js
new Vivus('story', { type: 'scenario-sync', start: 'autostart' });
```

### 6. Inline SVG vs external file
Inline SVG works directly. To load an external file dynamically, use the `file`
option (Vivus injects an `<object>` so it can reach the paths):
```js
new Vivus('container-id', { file: '/assets/logo.svg', duration: 200 });
```
Or wrap an `<object>` yourself — never use `<img>` (see Gotchas).

### 7. Trigger on scroll
`start: 'inViewport'` (the default) auto-plays when the SVG scrolls into view —
no scroll listener needed.
```js
new Vivus('my-svg', { start: 'inViewport', duration: 200 });
```
For finer control (e.g. IntersectionObserver), use `start: 'manual'` and call
`v.play()` yourself.

## Gotchas & Performance

- **Strokes only.** Each animated path needs `fill: none` and a visible `stroke`.
  If a shape is filled during the draw, the fill shows instantly and ruins the
  effect. Animate fills in **afterward** via the finish callback + a CSS transition.
- **No `<img>`.** Vivus must touch the path DOM. `<img src="x.svg">` exposes
  nothing — use **inline `<svg>`** or an `<object>` / the `file:` option.
- **`<text>` / raster won't draw.** Convert text to outlined vector paths
  (e.g. in Illustrator/Inkscape) before animating.
- **`duration` is frames, not ms.** Roughly `frames / 60 = seconds`.
- **Resized SVG?** Call `v.recalc()` after the element's dimensions change so
  dash lengths stay correct.
- **Respect `prefers-reduced-motion`.** Skip the animation and show the final
  drawing instead:
  ```js
  const v = new Vivus('my-svg', { start: 'manual', duration: 200 });
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    v.finish();    // show fully drawn, no motion
  } else {
    v.play();
  }
  ```
- **Tune `dashGap`** (default 2) if you see faint stroke remnants or gaps at the
  start of a path.

## Resources
- Repo & full docs: https://github.com/maxwellito/vivus
- Easing constants: `Vivus.LINEAR`, `Vivus.EASE`, `Vivus.EASE_IN`, `Vivus.EASE_OUT`, `Vivus.EASE_OUT_BOUNCE`

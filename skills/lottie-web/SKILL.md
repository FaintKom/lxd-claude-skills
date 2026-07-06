---
name: lottie-web
description: Lottie renders After Effects / JSON vector animations on the web via lottie-web or the modern dotLottie player. Use when playing designer animations, animated icons, loaders, or scroll/hover/cursor-interactive Lottie.
---

# Lottie on the Web

You help developers embed and control **Lottie** animations on the web. Lottie plays
**pre-made** vector animations exported from After Effects (via Bodymovin) or LottieFiles
as `.json` or compressed `.lottie` files. Two runtimes matter:

- **lottie-web** (`lottie-web`, by Airbnb) — the original, battle-tested SVG/canvas/HTML player.
- **dotLottie** (`@lottiefiles/dotlottie-web`) — the modern WASM/canvas player from LottieFiles
  with smaller `.lottie` files, built-in **state machines**, and first-class framework wrappers.

## When to use
- Playing a designer's exported animation (hero, splash, success checkmark).
- Animated icons, micro-interactions, loaders/spinners.
- Scroll-, hover-, or cursor-synced playback of a vector animation.
- You need the same animation across web / iOS / Android / React Native / Flutter (Lottie is cross-platform).

## When NOT to use
- You need **interactive state machines with complex logic / game-like input** → consider **Rive** (or dotLottie state machines for simpler cases).
- The animation must be **generated/edited programmatically** from data → use a code-driven lib (GSAP, Motion, Framer Motion, anime.js, CSS).
- It's a single simple transition → plain CSS/Web Animations is lighter than shipping a player.

---

## Quick Start

### Option A — lottie-web (classic, most compatible)
```bash
npm i lottie-web
```
```js
import lottie from 'lottie-web';

const anim = lottie.loadAnimation({
  container: document.getElementById('lottie'), // required DOM element
  renderer: 'svg',        // 'svg' | 'canvas' | 'html'
  loop: true,
  autoplay: true,
  path: '/animations/a.json', // or use `animationData: importedJson`
});
```
Import only what you need to shrink the bundle:
`import lottie from 'lottie-web/build/player/lottie_light';` (SVG-only, no expressions).

### Option B — dotLottie (modern, recommended for new projects)
```bash
npm i @lottiefiles/dotlottie-web
```
```js
import { DotLottie } from '@lottiefiles/dotlottie-web';

const dotLottie = new DotLottie({
  canvas: document.querySelector('#canvas'), // canvas element (not a div)
  src: '/animations/a.lottie',  // .lottie or .json URL; or `data: jsonStringOrObject`
  loop: true,
  autoplay: true,
  // speed: 1, segment: [0, 60], backgroundColor: '#fff',
  // renderConfig: { autoResize: true, devicePixelRatio: window.devicePixelRatio },
  // layout: { fit: 'contain', align: [0.5, 0.5] },
});
```

### Option C — Web component (no framework, drop-in markup)
```bash
npm i @lottiefiles/dotlottie-wc
```
```html
<script type="module" src="https://unpkg.com/@lottiefiles/dotlottie-wc@latest/dist/index.js"></script>
<dotlottie-wc src="/a.lottie" autoplay loop style="width:300px;height:300px"></dotlottie-wc>
```
lottie-web also ships a player web component (`@lottiefiles/lottie-player` → `<lottie-player>`),
but the dotLottie component is the actively developed one.

---

## Core Patterns

### 1. Playback control
```js
// lottie-web
anim.play(); anim.pause(); anim.stop();
anim.setSpeed(1.5);
anim.setDirection(-1);          // reverse
anim.goToAndStop(30, true);     // frame 30 (true = frames, false = ms)
anim.goToAndPlay(0, true);

// dotLottie
dotLottie.play(); dotLottie.pause(); dotLottie.stop();
dotLottie.setSpeed(1.5);
dotLottie.setMode('reverse');   // 'forward' | 'reverse' | 'bounce' | 'reverse-bounce'
dotLottie.setFrame(30);
```

### 2. Segments (play part of the timeline)
```js
// lottie-web — play frames 30→60, then loop that range
anim.playSegments([30, 60], true);

// dotLottie
dotLottie.setSegment(30, 60);   // constrain to a frame range
dotLottie.play();
```

### 3. Events
```js
// lottie-web
anim.addEventListener('complete', () => {});
anim.addEventListener('loopComplete', () => {});
anim.addEventListener('DOMLoaded', () => {});      // ready to control
anim.addEventListener('enterFrame', (e) => {});    // e.currentTime

// dotLottie
dotLottie.addEventListener('load', () => {});       // animation loaded
dotLottie.addEventListener('complete', () => {});
dotLottie.addEventListener('loop', () => {});
dotLottie.addEventListener('frame', (e) => {});     // e.currentFrame
dotLottie.addEventListener('play', () => {});
dotLottie.addEventListener('pause', () => {});
```

### 4. Renderer choice (perf)
- **SVG** (lottie-web default): crisp at any scale, CSS-styleable/inspectable, but slow with many layers/shapes.
- **Canvas** (lottie-web `renderer:'canvas'`, and dotLottie always canvas): far better for large, complex, or many simultaneous animations; not DOM-inspectable.
- Rule of thumb: small icon → SVG. Full-screen / dozens on screen → canvas (or dotLottie).

### 5. Interactivity (scroll / hover / cursor sync)
```bash
npm i @lottiefiles/lottie-interactivity   # works with lottie-web
```
```js
import { create } from '@lottiefiles/lottie-interactivity';
create({
  player: anim,            // a lottie-web instance (give it an id if using <lottie-player>)
  mode: 'scroll',          // 'scroll' | 'cursor' | 'hover' | 'chain'
  actions: [{ visibility: [0, 1], type: 'seek', frames: [0, 120] }],
});
```
For dotLottie, prefer **state machines** (load a `.lottie` that embeds a state machine):
```js
dotLottie.loadStateMachine('my-state-machine');
dotLottie.startStateMachine();
// drive transitions: dotLottie.postPointerDownEvent(x, y) / postPointerEnterEvent(...) etc.
```

### 6. React
```bash
npm i lottie-react          # classic
npm i @lottiefiles/dotlottie-react   # modern
```
```jsx
// lottie-react
import { useLottie } from 'lottie-react';
import data from './a.json';
function Icon() {
  const { View, play, pause } = useLottie({ animationData: data, loop: true });
  return View;
}

// dotlottie-react
import { DotLottieReact } from '@lottiefiles/dotlottie-react';
function Hero() {
  return <DotLottieReact src="/a.lottie" loop autoplay />;
}
// Grab the instance: <DotLottieReact dotLottieRefCallback={(dl) => (ref.current = dl)} ... />
```

### 7. Dynamic color / properties
- **lottie-web**: edit the parsed `animationData` JSON before load (color arrays are normalized `[r,g,b]` 0–1),
  or use the expressions interpreter (full `lottie.min.js`, not `lottie_light`) for runtime-driven values.
- **dotLottie**: use **theming** — load a `.lottie` with theme slots and call
  `dotLottie.setTheme('dark')` / `dotLottie.loadTheme(name)` to swap colors without touching JSON.

### 8. Reduced-motion guard (always do this)
```js
const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
const anim = lottie.loadAnimation({ /* ... */, autoplay: !reduce });
if (reduce) anim.goToAndStop(anim.totalFrames - 1, true); // show final frame, no motion
```

---

## Generating Lottie with AI

Need to **create** a Lottie animation, not just play one? Use the
**diffusionstudio/lottie** skill — a coding-agent harness that **generates production-ready
Lottie JSON from natural-language prompts** (for Claude Code, Codex, or any skill-capable agent).
It complements this skill: diffusionstudio/lottie produces the `.json`/`.lottie`, then you render
and control it with the patterns above.

```bash
npx skills add diffusionstudio/lottie
# then prompt your agent: "create a Lottie of a bouncing checkmark, green, 2s loop"
```
Repo: https://github.com/diffusionstudio/lottie

---

## Gotchas & Perf
- **Prefer `.lottie` over `.json`** — dotLottie's compressed format is dramatically smaller (often 5–10×) and bundles assets/themes.
- **Use the canvas renderer** for many or large/complex animations; SVG with hundreds of layers will jank.
- **Always clean up** to avoid leaks: `anim.destroy()` (lottie-web) / `dotLottie.destroy()` — on React unmount, in `useEffect` cleanup.
- **Lazy-load** offscreen animations (IntersectionObserver) and only `play()` when visible; pause when scrolled away.
- **Keep source files lean**: ask the designer to reduce layers, avoid heavy masks/mattes and expressions, rasterize where possible.
- **Honor `prefers-reduced-motion`** — pause/skip or show a static frame.
- **dotLottie needs a `<canvas>`**, lottie-web needs a container `<div>`; mixing them up is a common first error.
- **Sizing**: set explicit width/height on the container/canvas, or motion may render at 0×0.

## Resources
- lottie-web (Airbnb): https://github.com/airbnb/lottie-web
- dotLottie player docs: https://developers.lottiefiles.com/docs/dotlottie-player/
- AI Lottie generator (skill): https://github.com/diffusionstudio/lottie

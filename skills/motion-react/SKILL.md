---
name: motion-react
description: Motion (ex-Framer Motion) is a React animation and gesture library shipped as the `motion` package. Use when adding declarative React animations, layout/shared-element transitions, gestures, page transitions, scroll effects, or springs.
---

# Motion for React

Declarative, hardware-accelerated animations for React via the `motion` package (formerly `framer-motion`; the old package is deprecated in favor of `motion/react`). You get animated DOM/SVG components, gestures, layout animations, `AnimatePresence` exit animations, and motion-value/scroll hooks.

## When to use
- Component enter/exit, hover/tap/drag gestures, page and route transitions.
- Layout animations and shared-element (`layoutId`) transitions — Motion's standout feature.
- Spring physics, scroll-linked effects, scroll-triggered reveals (`whileInView`).

## When NOT to use
- Complex, precisely-sequenced timelines across many independent elements with scrubbing/labels — reach for **GSAP** (gsap + ScrollTrigger).
- Pure non-React projects — use the vanilla `animate()` (see bottom), or CSS for trivial transitions.

## Quick Start

```bash
npm install motion
```

```jsx
import { motion } from "motion/react";

export function Box() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.4, ease: "easeOut" }}
    />
  );
}
```

Any prop in `animate` not present in `initial` animates from its rendered/style value. Every HTML/SVG tag has a `motion.*` equivalent (`motion.button`, `motion.path`, ...).

## Core Patterns

### 1. Variants + propagation (stagger)
Parent variants cascade to children automatically; `staggerChildren` offsets them.

```jsx
const list = { show: { transition: { staggerChildren: 0.08 } } };
const item = { hidden: { opacity: 0, x: -20 }, show: { opacity: 1, x: 0 } };

<motion.ul variants={list} initial="hidden" animate="show">
  {data.map((d) => (
    <motion.li key={d.id} variants={item}>{d.label}</motion.li>
  ))}
</motion.ul>
```

### 2. Gestures: whileHover / whileTap / drag

```jsx
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
/>

<motion.div
  drag
  dragConstraints={{ left: 0, right: 300, top: 0, bottom: 0 }}
  dragElastic={0.2}
/>
```

### 3. AnimatePresence (exit + page transitions)
Enables `exit` animations when a component unmounts. Children need a stable `key`.

```jsx
import { AnimatePresence, motion } from "motion/react";

<AnimatePresence mode="wait">
  <motion.section
    key={route}                       // changing key triggers exit→enter
    initial={{ opacity: 0, y: 12 }}
    animate={{ opacity: 1, y: 0 }}
    exit={{ opacity: 0, y: -12 }}
  >
    {page}
  </motion.section>
</AnimatePresence>
```

### 4. Layout & shared-element transitions
`layout` animates size/position changes; `layoutId` morphs between two elements (e.g. a tab indicator or expanding card).

```jsx
<motion.div layout>{expanded ? "tall" : "short"}</motion.div>

{tabs.map((t) => (
  <button key={t} onClick={() => setActive(t)}>
    {t}
    {active === t && <motion.div layoutId="underline" className="bar" />}
  </button>
))}
```

### 5. Spring vs tween
`type: "spring"` is physics-based (interruptible, natural); tween is duration/easing-based.

```jsx
transition={{ type: "spring", stiffness: 300, damping: 30 }}     // springy
transition={{ type: "tween", duration: 0.3, ease: "easeInOut" }} // timed
```

### 6. Scroll-linked: useScroll + useTransform

```jsx
import { motion, useScroll, useTransform } from "motion/react";

function Progress() {
  const { scrollYProgress } = useScroll();           // 0→1 down the page
  const scaleX = scrollYProgress;                     // bind directly
  const opacity = useTransform(scrollYProgress, [0, 0.5], [1, 0]);
  return <motion.div style={{ scaleX, opacity, transformOrigin: "left" }} />;
}
```

### 7. Imperative: useAnimate

```jsx
import { useAnimate, stagger } from "motion/react";

function Shaker() {
  const [scope, animate] = useAnimate();             // scope = ref boundary
  async function run() {
    await animate(scope.current, { x: [0, -8, 8, 0] }, { duration: 0.3 });
    animate("li", { opacity: 1 }, { delay: stagger(0.05) }); // scoped selector
  }
  return <ul ref={scope} onClick={run}>{/* ... */}</ul>;
}
```

### 8. Motion values: useMotionValue / useSpring

```jsx
import { motion, useMotionValue, useSpring, useTransform } from "motion/react";

const x = useMotionValue(0);
const smoothX = useSpring(x, { stiffness: 200, damping: 25 });
const rotate = useTransform(smoothX, [-100, 100], [-15, 15]);
// updating x via drag won't re-render React — values flow straight to the DOM
<motion.div style={{ x: smoothX, rotate }} />
```

### 9. Scroll-triggered reveal: whileInView

```jsx
<motion.div
  initial={{ opacity: 0 }}
  whileInView={{ opacity: 1 }}
  viewport={{ once: true, amount: 0.3 }}   // fire once, when 30% visible
/>
```

## Gotchas & Performance
- **Animate transforms, not layout.** `x`/`y`/`scale`/`rotate`/`opacity` are GPU-composited and cheap. Animating `width`/`height`/`top`/`left` forces layout/paint — prefer `layout` or `scale`.
- **`layout` animations have a cost.** Each measures the DOM (read/write). Avoid hundreds at once; scope them and consider `layout="position"` when only position changes.
- **AnimatePresence rules:** direct children must have a unique, stable `key`; a conditionally-rendered single child must be the *direct* child of `AnimatePresence`. Use `mode="wait"` to fully exit before the next enters.
- **Motion values bypass React state** — updating a `useMotionValue` does not re-render. Use them for high-frequency updates (drag, scroll) to stay performant.
- **Respect reduced motion:** gate non-essential motion behind `useReducedMotion()`, or wrap the app in `<MotionConfig reducedMotion="user">`.

```jsx
import { useReducedMotion } from "motion/react";
const reduce = useReducedMotion();
const animate = reduce ? { opacity: 1 } : { opacity: 1, y: 0 };
```

## Vanilla Motion (non-React)
The same engine ships a framework-agnostic `animate()` from `"motion"` — no React needed:

```js
import { animate, scroll, stagger } from "motion";

animate("div", { opacity: 1, y: 0 }, { duration: 0.5, delay: stagger(0.1) });
scroll(animate(".bar", { scaleX: [0, 1] }));   // scroll-linked
```

There's also a 2.3kb mini build: `import { animate } from "motion/mini"` (styles only).

## Resources
- Repo: https://github.com/motiondivision/motion
- Docs: https://motion.dev/docs

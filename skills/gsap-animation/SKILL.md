---
name: gsap-animation
description: GSAP is a high-performance JS animation library for the web. Use when building timelines, ScrollTrigger scroll effects, complex sequencing, SVG morphing/drawing, or text reveals beyond what CSS handles.
---

# GSAP Animation Skill

[GSAP](https://gsap.com/) (GreenSock Animation Platform) is the industry-standard JavaScript animation engine for awards-caliber, high-performance web motion. As of 2025 GSAP is **100% free**, including every former "Club" plugin: ScrollTrigger, SplitText, MorphSVG, DrawSVG, ScrollSmoother, Flip, Observer, and more.

## When to use / When NOT to use

- **Use GSAP** for: orchestrated multi-step timelines, scroll-driven animation (scrub/pin), staggered reveals, SVG path morphing/drawing, physics-feel eases, and anything needing precise playback control (pause/reverse/seek/timeScale).
- **Use CSS** instead for: simple hover states, one-shot transitions, and loading spinners. No JS payload needed.
- **Use Motion (Framer Motion / motion.dev)** instead when: you want a declarative React-first API tightly bound to component lifecycle and layout animations. GSAP wins on complex imperative timelines, scroll choreography, and SVG plugins.

## Quick Start

```bash
npm install gsap
```

```js
import gsap from "gsap";

// Minimal tween: animate to a state over 1s
gsap.to(".box", { x: 200, rotation: 45, duration: 1, ease: "power2.out" });
```

## Core Patterns

### 1. to / from / fromTo

```js
gsap.to(".box",   { x: 300, opacity: 1, duration: 1 });           // current -> given
gsap.from(".box", { y: 50, opacity: 0, duration: 1 });            // given -> current (reveal)
gsap.fromTo(".box",
  { scale: 0, autoAlpha: 0 },                                     // from
  { scale: 1, autoAlpha: 1, duration: 0.8, ease: "back.out(1.7)" } // to
);
```

### 2. Timelines — sequencing, position params, labels

```js
const tl = gsap.timeline({ defaults: { duration: 0.6, ease: "power3.out" } });

tl.to(".title", { y: 0, opacity: 1 })
  .to(".subtitle", { y: 0, opacity: 1 }, "-=0.3")  // start 0.3s before prev ends
  .add("reveal")                                   // a named label
  .to(".cta", { scale: 1 }, "reveal")              // start AT label
  .to(".img", { opacity: 1 }, "<")                 // "<" = start of prev tween
  .to(".bg", { opacity: 1 }, 1.5);                 // absolute time = 1.5s

tl.pause(); tl.play(); tl.reverse(); tl.timeScale(2); // full control
```

### 3. Stagger

```js
gsap.to(".card", {
  y: 0, opacity: 1, duration: 0.5,
  stagger: 0.1,                                    // 0.1s between each
});

gsap.to(".grid-item", {
  scale: 1,
  stagger: { each: 0.05, from: "center", grid: "auto", ease: "power2.inOut" },
});
```

### 4. ScrollTrigger — scrub, pin, toggleActions

```js
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

// Scrub: animation progress tied to scroll position
gsap.to(".panel", {
  xPercent: -100,
  ease: "none",
  scrollTrigger: {
    trigger: ".panel",
    start: "top top",      // when trigger top hits viewport top
    end: "+=1500",         // 1500px of scroll
    scrub: 1,              // 1s catch-up smoothing (true = instant)
    pin: true,             // freeze element while scrubbing
    markers: false,        // true to debug start/end
  },
});

// Toggle-on-enter reveal (not scrubbed)
gsap.from(".feature", {
  y: 60, opacity: 0, duration: 0.8,
  scrollTrigger: {
    trigger: ".feature",
    start: "top 80%",
    toggleActions: "play none none reverse", // onEnter onLeave onEnterBack onLeaveBack
  },
});
```

### 5. Eases

```js
gsap.to(".a", { x: 200, ease: "power4.out" });        // strong decel
gsap.to(".b", { y: 100, ease: "elastic.out(1, 0.3)" });
gsap.to(".c", { scale: 1, ease: "back.out(1.7)" });   // slight overshoot
gsap.to(".d", { rotation: 360, ease: "steps(12)" });  // stepped
// Preview/cheatsheet: https://gsap.com/docs/v3/Eases
```

### 6. matchMedia() — responsive animations

```js
const mm = gsap.matchMedia();

mm.add("(min-width: 768px)", () => {
  // Desktop-only tweens; auto-reverted when the query stops matching
  gsap.to(".hero", { x: 400, scrollTrigger: { trigger: ".hero", scrub: true } });
});

mm.add("(max-width: 767px)", () => {
  gsap.to(".hero", { y: 100 });
});

mm.add("(prefers-reduced-motion: reduce)", () => {
  gsap.set(".hero", { opacity: 1 }); // respect a11y: no motion
});
```

### 7. context() — scoping & cleanup for SPA/React

```js
// Vanilla / framework-agnostic cleanup
const ctx = gsap.context(() => {
  gsap.to(".box", { x: 100 });          // selectors scoped to `scopeEl`
  gsap.timeline().to(".item", { y: 0, stagger: 0.1 });
}, scopeEl);                             // scope element (e.g. a ref.current)

// On unmount / route change:
ctx.revert();                            // kills every tween/ScrollTrigger created inside
```

### 8. ScrollTrigger + Lenis smooth scroll

```js
import Lenis from "lenis";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

const lenis = new Lenis();
// Drive Lenis from GSAP's ticker (single RAF loop, no jitter)
lenis.on("scroll", ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

### 9. SVG — DrawSVG & MorphSVG (now free)

```js
import gsap from "gsap";
import { DrawSVGPlugin } from "gsap/DrawSVGPlugin";
import { MorphSVGPlugin } from "gsap/MorphSVGPlugin";
gsap.registerPlugin(DrawSVGPlugin, MorphSVGPlugin);

gsap.fromTo("#path", { drawSVG: "0%" }, { drawSVG: "100%", duration: 2 }); // line draw
gsap.to("#circle", { morphSVG: "#star", duration: 1 });                    // shape morph
```

### 10. Text reveal with SplitText (now free)

```js
import gsap from "gsap";
import { SplitText } from "gsap/SplitText";
gsap.registerPlugin(SplitText);

// v3.13+ factory API. `mask` clips for a clean reveal; `autoSplit` re-splits on font load/resize.
const split = SplitText.create(".headline", {
  type: "words, chars",
  mask: "chars",
  autoSplit: true,
  onSplit(self) {
    return gsap.from(self.chars, {
      yPercent: 100, opacity: 0, duration: 0.6,
      ease: "power3.out", stagger: 0.03,
    });
  },
});
// split.revert(); to restore original markup
```

## Gotchas & Perf

- **Animate transforms, not layout props.** Prefer `x`/`y`/`scale`/`rotation`/`opacity` (compositor-friendly) over `left`/`top`/`width`/`margin` (trigger layout/paint).
- **`will-change`**: GSAP usually handles GPU layering; add `will-change: transform` sparingly for known-heavy elements and remove it after — leaving it on everything hurts memory.
- **`force3D`**: defaults to `"auto"`; set `force3D: true` to force GPU layers, or `false` to avoid blurry text on subpixel transforms.
- **Kill tweens** to avoid leaks/conflicts: `gsap.killTweensOf(".box")`, `tween.kill()`, or `ctx.revert()`. Store timeline refs and kill on unmount.
- **React StrictMode double-fires** effects in dev — without cleanup you get duplicate/ghost animations. Use `useGSAP` or `gsap.context().revert()` so the second mount cleans up the first.
- **`ScrollTrigger.refresh()`** after DOM/layout changes (images load, fonts swap, content toggles) so start/end positions recalc. It auto-fires on resize, but call manually after async content.

## React usage — useGSAP

```jsx
import { useRef } from "react";
import gsap from "gsap";
import { useGSAP } from "@gsap/react";
gsap.registerPlugin(useGSAP);

export default function Hero() {
  const container = useRef(null);

  // Auto-cleanup (reverts on unmount) + scoped selectors + StrictMode-safe.
  const { contextSafe } = useGSAP(() => {
    gsap.from(".box", { y: 50, opacity: 0, stagger: 0.1 });
  }, { scope: container });              // pass `dependencies: [dep]` to re-run

  // Event handlers must be wrapped so their tweens are tracked for cleanup:
  const onClick = contextSafe(() => {
    gsap.to(".box", { rotation: "+=90" });
  });

  return (
    <div ref={container}>
      <div className="box" onClick={onClick}>Hello</div>
    </div>
  );
}
```

Install: `npm install gsap @gsap/react`.

## Resources

- Repo: https://github.com/greensock/GSAP
- Docs: https://gsap.com/docs/v3/
- Eases visualizer: https://gsap.com/docs/v3/Eases
- ScrollTrigger: https://gsap.com/docs/v3/Plugins/ScrollTrigger/
- React (useGSAP): https://gsap.com/resources/React/

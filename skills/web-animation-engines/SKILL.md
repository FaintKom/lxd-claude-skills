---
name: web-animation-engines
description: Router/decision-guide for picking the right web animation library and the matching skill. Use when a task needs motion/animation on the web but the engine is unspecified — "animate this", "make it move", "smooth scroll", "3D scene", "animated text", "play a Lottie", "canvas editor" — and you need to choose between GSAP, Motion, three.js, Pixi, Lottie, Rive, etc.
---

# Web Animation Engines — Router

**Role**: Pick the right tool, then load its skill. Don't hand-roll motion when a focused skill exists.

This skill is an index. It maps a task to ONE recommended engine and the skill that
covers it in depth. Read the table, pick, then invoke that skill (e.g. `gsap-animation`).
Each linked skill has Quick Start + real patterns + gotchas.

## How to choose (fast path)

```
DOM elements, complex sequencing/timeline?      → gsap-animation
DOM in React, declarative + gestures?           → motion-react
React, physics-feel springs/lists?              → react-spring-animation
Smooth/inertia page scroll?                     → lenis-smooth-scroll
Simple reveal-on-scroll (landing page)?         → aos-scroll / scrollreveal-scroll
Scrubbed/pinned scroll scenes?                  → gsap-animation (ScrollTrigger) [legacy: scrollmagic-scroll]
3D scene / WebGL?                               → threejs-* skills (already installed)
2D WebGL at scale (particles, sprites, games)?  → pixijs-animation
Tiny custom GLSL / fullscreen shader?           → ogl-webgl
Shader effects on existing <img>/DOM?           → curtainsjs-webgl
Programmatic SVG draw + animate?                → svgjs-animation
SVG line "self-drawing" (logo/signature)?       → vivus-svg-drawing
Heavy SVG: masking, path morph, load .svg?      → snapsvg-animation
Canvas editor: drag/resize shapes, diagrams?    → konva-canvas
Canvas design tool: select/serialize/export?    → fabricjs-canvas
Generative vector art, path/boolean geometry?   → paperjs-canvas
Renderer-agnostic 2D (svg/canvas/webgl)?        → twojs-animation
Visual keyframe editor driving any JS values?   → theatrejs-editor
Interactive animation w/ state machines?        → rive-animation
Play designer-made AE/JSON animation?           → lottie-web
Typewriter / typing text?                       → typedjs-text
Per-letter/word staggered text reveal?          → splittingjs-text + gsap-animation
Distorted/liquid/glitch shader text?            → blotter-text
Letter-level keyframe text (rotate/scale chars)? → animejs-animation (installed) or gsap-animation + splittingjs-text
```

## By category

### Timeline / general motion
- **gsap-animation** — industry standard. Timelines, ScrollTrigger, SplitText, eases, SVG plugins (all free since 2025). Default for anything non-trivial, framework-agnostic. **When in doubt, start here.**
- **motion-react** (ex-Framer Motion) — declarative React: `initial/animate`, variants, gestures, `AnimatePresence`, layout/shared-element transitions. Best DX inside React.
- **react-spring-animation** — physics springs (`useSpring`/`useTrail`/`useTransition`), pairs with `@use-gesture`. Pick when you want natural feel over keyframes.
- **animejs-animation** *(already installed)* — lightweight standalone timeline/stagger engine.

### Scroll
- **lenis-smooth-scroll** — smooth/inertia scroll. The glue layer; syncs with GSAP ScrollTrigger & three.js. (Respect `prefers-reduced-motion`.)
- **aos-scroll** — dead-simple `data-aos` reveal, zero JS per element. Landing pages.
- **scrollreveal-scroll** — JS-config reveal per selector.
- **scrollmagic-scroll** — *legacy/unmaintained*. Pinning/scenes. For new work prefer GSAP ScrollTrigger.

### 3D / WebGL
- **threejs-*** *(already installed: fundamentals, materials, shaders, loaders, postprocessing, animation, interaction, geometry, lighting, textures)* + **3d-web-experience** — full 3D. Use these for any 3D scene.
- **pixijs-animation** — fast 2D WebGL/WebGPU. Particles, sprites, 2D games, data-viz at scale.
- **ogl-webgl** — minimal (~5kb) low-level WebGL. Fullscreen shader quads, raw control, you write the GLSL.
- **curtainsjs-webgl** — binds WebGL planes to DOM `<img>`/`<video>`. Image hover-distortion, scroll shader effects, keeps DOM/SEO.

### SVG
- **svgjs-animation** — modern create+manipulate+animate SVG (runner API).
- **vivus-svg-drawing** — stroke "self-drawing" effect (logos, signatures, line-art).
- **snapsvg-animation** — heavyweight SVG: masking/clipping, path morph, load external SVG. (Older.)

### Canvas 2D (scene graphs / editors)
- **konva-canvas** — scene graph + events + drag + Transformer handles. Diagrams, whiteboards. (`react-konva`.)
- **fabricjs-canvas** — object model with built-in selection/transform + JSON/SVG serialization. Design/annotation tools.
- **paperjs-canvas** — richest path/segment/curve + boolean geometry. Generative vector art, path math.
- **twojs-animation** — one API across SVG/Canvas/WebGL backends. Flat 2D vector motion.

### Declarative / designer-driven
- **theatrejs-editor** — visual keyframe editor that animates arbitrary JS values; drives three.js/R3F/DOM. (Ship `@theatre/core` only, not studio.)
- **rive-animation** — interactive vector animation with **state machines + runtime inputs**. `.riv` files from the Rive editor. Interactive icons/characters/onboarding.
- **lottie-web** — plays After Effects / JSON / `.lottie` animations. Loaders, animated icons. Prefer dotLottie player. *(Generating Lottie JSON from a prompt: the `diffusionstudio/lottie` generator skill.)*

### Text effects
- **typedjs-text** — typewriter/typing, rotating phrases.
- **splittingjs-text** — splits text into chars/words/lines with CSS index vars; animate with CSS or GSAP. The standard for staggered letter reveals.
- **blotter-text** — WebGL shader materials on text (liquid/glitch/distort).

## Decision tips / common combos
- **Creative agency site**: `lenis-smooth-scroll` + `gsap-animation` (ScrollTrigger) + `splittingjs-text` for headlines; add `curtainsjs-webgl` or `ogl-webgl` for image shader effects.
- **3D product/portfolio**: `threejs-*` (+ `theatrejs-editor` to author the camera/object motion visually).
- **Animated UI micro-interactions in React**: `motion-react` (or `react-spring-animation` for spring feel).
- **Animated icons/loaders shipped by designers**: `lottie-web`; if they must react to state/input → `rive-animation`.
- **Whiteboard / design editor**: `fabricjs-canvas` (serialization) or `konva-canvas` (events/perf).
- **Accessibility**: every motion path must honor `prefers-reduced-motion` — each skill notes how.

## Not sure? Default ladder
1. It's React UI motion → `motion-react`.
2. It's anything else non-trivial on the DOM/SVG → `gsap-animation`.
3. It's 3D → `threejs-*`.
4. It's a designer file → `lottie-web` (static) / `rive-animation` (interactive).

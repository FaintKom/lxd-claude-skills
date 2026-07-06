---
name: rive-animation
description: Rive interactive vector animation with runtime-controllable State Machines, inputs, and events. Use when building interactive animated icons, characters, onboarding, or data-driven UI animation — a lighter interactive alternative to Lottie.
---

# Rive (Web Runtime)

Role: implement and control **Rive** animations on the web. Rive plays `.riv` files (vector
animations authored in the Rive editor) and exposes **State Machines** — graphs of states wired
to **inputs** (boolean / number / trigger) that you drive at runtime. This makes animations
interactive and stateful rather than fixed playback.

## When to use
- Interactive animated icons, buttons, toggles, characters, mascots.
- Onboarding / hero animations that react to user input, scroll, or app state.
- Data-driven UI animation (gauges, progress, avatars) updated from JS.
- You want runtime control: change state, fire events, swap text, two-way data binding.

## When NOT to use (vs Lottie)
- **Lottie** = play an After Effects export; great for fire-and-forget decorative loops, huge
  free asset libraries. No native interaction model.
- **Rive** = State Machines + inputs + events + data binding + runtime API; designed in the
  Rive editor (not hand-coded). Choose Rive when the animation must *respond*; choose Lottie
  for static playback or when you only have AE assets.
- `.riv` files are produced in the Rive editor — you do not write them by hand.

## Quick Start

```bash
npm i @rive-app/canvas
# alternatives: @rive-app/webgl2 (advanced rendering), @rive-app/canvas-lite (smaller)
# React: npm i @rive-app/react-canvas
```

```js
import { Rive, Layout, Fit, Alignment, EventType } from '@rive-app/canvas';

const canvas = document.getElementById('canvas');
const rive = new Rive({
  src: '/animations/character.riv', // or a hosted https URL
  canvas,
  autoplay: true,
  stateMachines: 'State Machine 1', // name(s) from the editor
  artboard: 'Main',                 // optional; defaults to default artboard
  layout: new Layout({ fit: Fit.Contain, alignment: Alignment.Center }),
  autoBind: true,                   // bind default ViewModelInstance (data binding)
  onLoad: () => rive.resizeDrawingSurfaceToCanvas(),
});
```

Loading is **async** — only touch inputs/text/state inside (or after) `onLoad`.

## Core Patterns

### 1. Load a `.riv` and play an animation
```js
const rive = new Rive({
  src: '/loader.riv',
  canvas: document.getElementById('canvas'),
  autoplay: true,
  // play a linear animation instead of a state machine:
  animations: 'spin',
  onLoad: () => rive.resizeDrawingSurfaceToCanvas(),
});
// rive.play('spin'); rive.pause(); rive.stop();
```

### 2. State Machine inputs (boolean / number / trigger)
Fetch inputs by state-machine name, then set `.value` or call `.fire()`.
```js
const inputs = rive.stateMachineInputs('State Machine 1'); // SMIInput[]
const hover  = inputs.find(i => i.name === 'hover');   // boolean -> SMIBoolean
const level  = inputs.find(i => i.name === 'level');   // number  -> SMINumber
const bump   = inputs.find(i => i.name === 'bump');    // trigger -> SMITrigger

canvas.addEventListener('mouseenter', () => { hover.value = true;  });
canvas.addEventListener('mouseleave', () => { hover.value = false; });
level.value = 75;   // drive a number input (e.g. a gauge)
bump.fire();        // fire a one-shot trigger
```

### 3. Responsive layout
Set a `Layout` (Fit + Alignment), and re-fit on every resize / DPR change.
```js
const rive = new Rive({
  src: '/hero.riv', canvas,
  autoplay: true, stateMachines: 'SM',
  layout: new Layout({ fit: Fit.Cover, alignment: Alignment.Center }),
  onLoad: () => rive.resizeDrawingSurfaceToCanvas(),
});
window.addEventListener('resize', () => rive.resizeDrawingSurfaceToCanvas());
// Fit.Layout makes the artboard itself responsive (use with autoBind/data binding).
```

### 4. Listen to Rive events (fired from the State Machine)
```js
rive.on(EventType.RiveEvent, (event) => {
  const { name, properties } = event.data; // e.g. 'buttonClicked', { url: '...' }
  if (name === 'openLink' && properties?.url) window.open(properties.url);
});
```

### 5. Text runs (swap text at runtime)
```js
rive.setTextRunValue('scoreText', '1,024');
// scoped to a nested artboard path:
// rive.setTextRunValueAtPath('label', 'Hi', 'nested/path');
```

### 6. Data binding / nested inputs
`autoBind: true` binds the default `ViewModelInstance` so editor-bound properties
(text, color, number) update live. Access view-model properties for two-way binding:
```js
const vmi = rive.viewModelInstance;            // requires autoBind (or bind manually)
vmi?.number('progress')?.value = 0.5;          // push a value into the artboard
vmi?.string('username')?.value = 'Ada';
const trigger = vmi?.trigger('celebrate'); trigger?.trigger();
// Nested state machines: target nested artboard inputs via the view model / path APIs.
```

### 7. Cleanup (free WASM memory)
```js
rive.cleanup(); // call on unmount / when the animation is gone
```

## React (@rive-app/react-canvas)

```jsx
import { useRive, useStateMachineInput } from '@rive-app/react-canvas';

function LikeButton() {
  const { rive, RiveComponent } = useRive({
    src: '/like.riv',
    stateMachines: 'State Machine 1',
    autoplay: true,
    layout: new Layout({ fit: Fit.Contain, alignment: Alignment.Center }),
    autoBind: true,
  });

  // grab a specific input by state-machine + input name
  const liked = useStateMachineInput(rive, 'State Machine 1', 'liked');
  const bump  = useStateMachineInput(rive, 'State Machine 1', 'bump');

  return (
    <RiveComponent
      style={{ width: 120, height: 120 }}
      onClick={() => { if (liked) liked.value = !liked.value; bump?.fire(); }}
    />
  );
}
```

The hook handles canvas mounting, resize, and `cleanup()` automatically on unmount.
Use `rive.on(EventType.RiveEvent, ...)` inside a `useEffect` once `rive` is ready.

## Gotchas & Perf
- **Always `resizeDrawingSurfaceToCanvas()`** in `onLoad` and on `resize` / DPR change —
  skipping it gives blurry or mis-scaled output. The vanilla runtime does NOT auto-resize.
- **Load is async**: inputs, text runs, and view models are only valid after `onLoad`.
- **`.riv` files come from the Rive editor**, not hand-coding — request the state-machine and
  input/text-run names from the designer; they are case-sensitive strings.
- **Call `cleanup()`** (vanilla) to free the underlying WASM/C++ objects and avoid leaks; the
  React hook does this for you.
- **Pause offscreen**: stop or pause animations not in view (IntersectionObserver) to save CPU/GPU.
- Prefer `@rive-app/canvas` for broad support; use `@rive-app/webgl2` only when you need its
  advanced rendering (meshes, advanced blend) and `canvas-lite` to shrink bundle size.

## Resources
- Runtime repo (WASM + JS): https://github.com/rive-app/rive-wasm
- Web JS docs: https://rive.app/docs/runtimes/web/web-js

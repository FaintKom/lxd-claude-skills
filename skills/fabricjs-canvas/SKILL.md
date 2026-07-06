---
name: fabricjs-canvas
description: Fabric.js is an interactive HTML5 canvas library with an object model, selection/transform controls, free drawing, and JSON/SVG serialization. Use when building canvas editors, image annotation, design tools, whiteboards, or anything needing drag/resize objects and export.
---

# Fabric.js Canvas (v6)

You build interactive canvas applications with Fabric.js v6. Fabric wraps the
`<canvas>` element in a retained-mode object model: every shape, image, and text
is a stateful object you can select, drag, scale, rotate, group, serialize, and
restore. v6 is ESM-only with named imports (`import { Canvas, Rect } from 'fabric'`).

## When to use

- Design tools, whiteboards, diagram editors, meme/poster makers.
- Image annotation: bounding boxes, labels, freehand markup over a photo.
- Anything where the user must **select, move, resize, rotate** objects with
  built-in transform handles, then **export to JSON/SVG/PNG**.

## When NOT to use (vs Konva / plain canvas)

- **Konva** is leaner and faster for pure rendering / games / many thousands of
  nodes with custom interaction. Fabric is heavier but ships selection,
  transform controls, and serialization out of the box.
- For one-off static drawing or charts, use the raw Canvas 2D API or a chart lib.
- For GPU-heavy / 3D scenes, use WebGL (Three.js / PixiJS).

Fabric's edge is editor-focused features: control handles, active selection,
free drawing, and round-trip `toJSON`/`loadFromJSON`. Reach for it when the app
is an **editor**, not a renderer.

## Quick Start

```bash
npm i fabric
```

```html
<canvas id="c" width="800" height="600"></canvas>
```

```js
import { Canvas, Rect } from 'fabric';

const canvas = new Canvas('c', { backgroundColor: '#f3f3f3' });

canvas.add(new Rect({
  left: 100, top: 100, width: 120, height: 80,
  fill: '#3b82f6', rx: 8, ry: 8,
}));
// add() auto-renders; for manual batches call canvas.requestRenderAll()
```

## Core Patterns

### 1. Creating objects

```js
import { Canvas, Rect, Circle, Textbox } from 'fabric';

canvas.add(
  new Rect({ left: 20, top: 20, width: 100, height: 60, fill: '#ef4444' }),
  new Circle({ left: 160, top: 20, radius: 40, fill: '#22c55e' }),
  new Textbox('Edit me', { left: 20, top: 120, width: 200, fontSize: 24 }),
);
```

### 2. Images (async — returns a Promise)

```js
import { FabricImage } from 'fabric';

const img = await FabricImage.fromURL(
  'https://example.com/photo.jpg',
  { crossOrigin: 'anonymous' },          // needed for later toDataURL export
  { left: 0, top: 0, scaleX: 0.5, scaleY: 0.5 },
);
canvas.add(img);
canvas.requestRenderAll();
```

### 3. Selection & control locking

```js
const r = new Rect({ left: 50, top: 50, width: 80, height: 80, fill: '#888' });
r.set({
  selectable: true,        // can be selected at all
  lockMovementX: true,     // pin horizontally
  lockRotation: true,      // hide/disable rotation
  hasControls: true,       // show resize/rotate handles
});
canvas.add(r);

// Global selection behaviour
canvas.selection = true;            // allow rubber-band multi-select
canvas.set('hoverCursor', 'move');
```

### 4. Events

```js
canvas.on('object:modified', (e) => {
  console.log('moved/scaled/rotated:', e.target);
});
canvas.on('object:moving', (e) => {           // live drag — snap to grid
  const o = e.target;
  o.set({ left: Math.round(o.left / 20) * 20, top: Math.round(o.top / 20) * 20 });
});
canvas.on('selection:created', (e) => console.log('selected', e.selected));
canvas.on('selection:cleared', () => console.log('nothing selected'));
canvas.on('mouse:down', (opt) => console.log('pointer', canvas.getPointer(opt.e)));
```

### 5. Free drawing (PencilBrush)

```js
import { PencilBrush } from 'fabric';

canvas.isDrawingMode = true;
canvas.freeDrawingBrush = new PencilBrush(canvas);
canvas.freeDrawingBrush.color = '#1e293b';
canvas.freeDrawingBrush.width = 4;
// each stroke becomes a Path object you can later select/serialize.
// Toggle off: canvas.isDrawingMode = false;
```

### 6. Animation

```js
import { util } from 'fabric';

// Per-object convenience animate()
rect.animate({ angle: 360, left: 300 }, {
  duration: 800,
  onChange: () => canvas.requestRenderAll(),
  easing: util.ease.easeInOutQuad,
});

// Low-level util.animate for custom values
util.animate({
  startValue: 0, endValue: 1, duration: 500,
  onChange: (opacity) => { rect.set({ opacity }); canvas.requestRenderAll(); },
});
```

### 7. Groups & ActiveSelection

```js
import { Group, ActiveSelection } from 'fabric';

// Permanent group (acts as one object)
const group = new Group([rectA, rectB], { left: 100, top: 100 });
canvas.add(group);

// Programmatically multi-select existing objects
const sel = new ActiveSelection(canvas.getObjects(), { canvas });
canvas.setActiveObject(sel);
canvas.requestRenderAll();
```

### 8. Serialization & export

```js
// Save -> JSON (include any custom props you set)
const json = canvas.toJSON(['myCustomId']);
localStorage.setItem('scene', JSON.stringify(json));

// Restore (async in v6 — returns a Promise)
await canvas.loadFromJSON(json);
canvas.requestRenderAll();

// Export raster
const png = canvas.toDataURL({ format: 'png', multiplier: 2 }); // 2x for retina

// Export vector
const svg = canvas.toSVG();
```

### 9. Image filters

```js
import { FabricImage, filters } from 'fabric';

const img = await FabricImage.fromURL('photo.jpg', { crossOrigin: 'anonymous' });
img.filters.push(
  new filters.Grayscale(),
  new filters.Brightness({ brightness: 0.1 }),
);
img.applyFilters();          // must call after changing filters
canvas.add(img);
canvas.requestRenderAll();
```

## Gotchas & Performance

- **ESM named imports only.** v6 has no default `fabric` export and no global
  `window.fabric`. Use `import { Canvas, Rect, FabricImage } from 'fabric'`.
- **`Image` was renamed `FabricImage`** in v6 to avoid clashing with the DOM
  `Image`. Its loader is `FabricImage.fromURL(url, opts, objOpts)` and it
  **returns a Promise** — always `await` it.
- **`loadFromJSON` is async in v6** (returns a Promise). Await it before
  rendering, then call `requestRenderAll()`.
- **Render explicitly.** `add()`/`remove()` auto-render, but bulk mutations
  (looping `set()`, filter changes) do not. Use **`requestRenderAll()`**
  (batches to the next animation frame) instead of `renderAll()` (synchronous,
  forces an immediate redraw — only for one-off final renders).
- **`crossOrigin: 'anonymous'`** is required when loading remote images you later
  export with `toDataURL`/`toSVG`, otherwise the canvas is tainted and export throws.
- **Object caching** (`objectCaching`, on by default) renders each object to an
  offscreen cache for speed. Disable per-object (`obj.objectCaching = false`)
  only if an object's appearance updates every frame (e.g. live data).
- **Large images / many objects:** pre-scale big images before adding, cap
  `multiplier` on exports, and avoid per-frame `renderAll()`. For very large
  scenes set `canvas.renderOnAddRemove = false` while batch-adding, then render once.
- **Retina:** `enableRetinaScaling` (default `true`) sharpens output on HiDPI
  displays but increases the backing-store size — disable it for huge canvases
  if memory/perf matters.
- **Dispose** the canvas (`canvas.dispose()`) on teardown (e.g. React unmount)
  to remove listeners and free the context.

## Resources

- Repo: https://github.com/fabricjs/fabric.js
- Docs (verify current v6 API here): http://fabricjs.com/
- API reference: http://fabricjs.com/docs/

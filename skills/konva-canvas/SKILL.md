---
name: konva-canvas
description: Konva is a 2D canvas framework with a scene graph, event handling, drag-drop, and animation. Use when building interactive canvas apps, draggable shapes, diagram/whiteboard editors, or hit detection on canvas.
---

# Konva.js — Interactive 2D Canvas Scene Graph

## Role & When to Use

Konva renders to HTML5 `<canvas>` but gives you a retained **scene graph** (Stage → Layer → Group → Shape) with per-node events, dragging, tweening, and pixel-accurate hit detection. You manipulate objects, Konva redraws.

**Use Konva when:**
- Building diagram / whiteboard / flowchart / floor-plan editors
- You need draggable, clickable, resizable shapes with selection handles
- Hundreds–low-thousands of interactive shapes where you want canvas perf + object events
- Image annotation, cropping, design tools, simple 2D games

**Use NOT Konva when:**
- **vs PixiJS:** Pixi is WebGL-first, built for high-FPS games / particle-heavy / tens of thousands of sprites. Konva is canvas2d and shines for *editors and tools* with rich event/drag semantics, not bullet-hell games.
- **vs SVG/DOM:** For a handful of shapes that need CSS/DOM/accessibility, plain SVG is simpler. Konva wins once you have many interactive shapes (SVG DOM gets slow) or need canvas export/filters.
- **vs Three.js:** Anything genuinely 3D.

## Quick Start

```bash
npm install konva
```

```js
import Konva from 'konva';

const stage = new Konva.Stage({
  container: 'container', // id of a <div> OR a DOM element
  width: 800,
  height: 600,
});

const layer = new Konva.Layer();
stage.add(layer);

const rect = new Konva.Rect({
  x: 50, y: 50, width: 120, height: 80,
  fill: '#4f46e5', cornerRadius: 8,
  draggable: true,
});
layer.add(rect);
// In Konva 8+ adding a node auto-draws; call layer.draw() if needed.
```

## Core Patterns

### 1. Hierarchy: Stage → Layer → Group → Shape
```js
const stage = new Konva.Stage({ container: 'app', width: 800, height: 600 });
const layer = new Konva.Layer();
const group = new Konva.Group({ x: 100, y: 100, draggable: true }); // moves children together
group.add(new Konva.Circle({ radius: 30, fill: 'red' }));
group.add(new Konva.Text({ text: 'node', y: 40 }));
layer.add(group);
stage.add(layer);
```
Each `Layer` is its own canvas element. Groups are logical containers (transforms cascade to children).

### 2. Shapes
```js
new Konva.Rect({ x, y, width, height, fill, stroke, strokeWidth, cornerRadius });
new Konva.Circle({ x, y, radius, fill });
new Konva.Ellipse({ x, y, radiusX, radiusY });
new Konva.Text({ x, y, text: 'Hi', fontSize: 18, fontFamily: 'Arial', fill: '#111' });
new Konva.Line({ points: [0,0, 100,50, 200,0], stroke: 'black', strokeWidth: 2, lineCap: 'round', tension: 0.5 });
new Konva.Line({ points: [0,0, 80,0, 80,80, 0,80], closed: true, fill: '#eee' }); // polygon
new Konva.Path({ data: 'M10 10 H 90 V 90 H 10 Z', fill: 'green' }); // SVG path data
// Image needs a loaded HTMLImageElement:
const img = new Image();
img.onload = () => { layer.add(new Konva.Image({ image: img, x: 0, y: 0, width: 200, height: 150 })); };
img.src = 'photo.png';
// Or the helper:
Konva.Image.fromURL('photo.png', (node) => { node.setAttrs({ x: 10, y: 10 }); layer.add(node); });
```

### 3. Dragging + dragmove
```js
const node = new Konva.Rect({ x: 20, y: 20, width: 60, height: 60, fill: 'teal', draggable: true });
node.on('dragstart', () => node.opacity(0.6));
node.on('dragmove', () => console.log(node.x(), node.y())); // live position; snap here
node.on('dragend', () => node.opacity(1));
// Constrain movement:
node.dragBoundFunc((pos) => ({ x: pos.x, y: 50 })); // lock Y axis
```

### 4. Events: on() / off()
```js
shape.on('click tap', (e) => { /* e.target, e.evt (native) */ });
shape.on('mouseenter', () => { stage.container().style.cursor = 'pointer'; });
shape.on('mouseleave', () => { stage.container().style.cursor = 'default'; });
shape.on('dblclick dbltap', () => {});
// Namespaced + delegated:
shape.on('click.menu', handler);
shape.off('click.menu');
stage.on('click', (e) => { if (e.target === stage) clearSelection(); }); // click empty area
```

### 5. Konva.Animation (per-frame loop)
```js
const hexagon = new Konva.RegularPolygon({ x: 100, y: 100, sides: 6, radius: 40, fill: 'orange' });
layer.add(hexagon);
const anim = new Konva.Animation((frame) => {
  // frame.time (ms since start), frame.timeDiff, frame.frameRate
  hexagon.rotation((frame.time * 0.1) % 360);
}, layer); // pass layer(s) so they auto-redraw
anim.start();
// anim.stop();
```

### 6. Tweens: node.to() and Konva.Tween
```js
// Quick one-off:
node.to({ x: 300, scaleX: 1.5, duration: 0.4, easing: Konva.Easings.EaseInOut,
          onFinish: () => console.log('done') });
// Reusable / controllable:
const tween = new Konva.Tween({ node, duration: 1, x: 200, opacity: 0.5,
                                easing: Konva.Easings.BounceEaseOut });
tween.play();   // tween.reverse(); tween.pause(); tween.reset();
```

### 7. Transformer (resize / rotate handles)
```js
const tr = new Konva.Transformer({
  rotateEnabled: true,
  keepRatio: false,
  boundBoxFunc: (oldBox, newBox) => (newBox.width < 20 ? oldBox : newBox), // min size
});
layer.add(tr);
tr.nodes([rect]); // attach to one or many selected nodes
stage.on('click tap', (e) => {
  if (e.target === stage) { tr.nodes([]); return; }
  if (e.target.draggable()) tr.nodes([e.target]);
});
// Transformer scales the node; on 'transformend' you often bake scale into width/height:
rect.on('transformend', () => {
  rect.width(rect.width() * rect.scaleX());
  rect.height(rect.height() * rect.scaleY());
  rect.scaleX(1); rect.scaleY(1);
});
```

### 8. Redraw, caching, layering
```js
layer.draw();        // full redraw
layer.batchDraw();   // coalesce many redraws into next animation frame
heavyGroup.cache();  // render subtree to an offscreen bitmap (big win for complex/static nodes)
heavyGroup.clearCache();
node.moveToTop();    // z-order: moveToBottom, moveUp, moveDown, zIndex(n)
```

## React — react-konva

```bash
npm install react-konva konva
```
```jsx
import { Stage, Layer, Rect, Circle, Transformer } from 'react-konva';

function Canvas() {
  const [pos, setPos] = React.useState({ x: 40, y: 40 });
  return (
    <Stage width={800} height={600}>
      <Layer>
        <Rect
          x={pos.x} y={pos.y} width={100} height={60} fill="indigo"
          draggable
          onDragEnd={(e) => setPos({ x: e.target.x(), y: e.target.y() })}
          onClick={() => console.log('clicked')}
        />
        <Circle x={300} y={120} radius={40} fill="crimson" />
      </Layer>
    </Stage>
  );
}
```
Props mirror Konva attrs; events are `onClick`, `onDragMove`, `onTransformEnd`, etc. For Transformer, use a `ref` and call `transformerRef.current.nodes([shapeNode])` in an effect after selection changes.

## Gotchas & Performance

- **Minimize layers.** Each `Layer` is a separate canvas (GPU/memory cost). 3–5 layers max; group static vs dynamic content rather than spawning layers.
- **`listening(false)`** on non-interactive nodes removes them from the hit graph — large win for backgrounds/decorations: `bg.listening(false)`.
- **`cache()` complex/static nodes** (text-heavy groups, many sub-shapes, filtered nodes). Re-`cache()` after you change them.
- **`perfectDrawEnabled(false)`** skips an extra buffer pass used to fix stroke+fill+opacity edge artifacts — turn off when you don't need pixel-perfect strokes.
- **Use `batchDraw()`** inside drag/animation handlers instead of `draw()` to avoid redrawing every event.
- **Retina/HiDPI:** Konva auto-uses `window.devicePixelRatio`. Override globally with `Konva.pixelRatio = 1` (faster, blurrier) or per stage. Sizes stay in CSS pixels.
- **Filters require `cache()`** first: `node.cache(); node.filters([Konva.Filters.Blur]); node.blurRadius(8);`
- **`shadow*` is expensive** — avoid shadows on many moving nodes; cache instead.
- **`hitStrokeWidth`** widens the clickable area of thin lines without changing visuals.
- **Memory:** call `node.destroy()` (not just `remove()`) when permanently discarding nodes; `stage.destroy()` on unmount.

## Resources

- Repo: https://github.com/konvajs/konva
- Docs & tutorials: https://konvajs.org/docs/
- Full API reference: https://konvajs.org/api/Konva.html
- React bindings: https://github.com/konvajs/react-konva

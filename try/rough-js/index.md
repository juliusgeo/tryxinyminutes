---
x: Rough.js
title: Try Rough.js in Y minutes
image: /try/rough-js/cover.png
lastmod: 2024-02-24
original: https://roughjs.com/
license: MIT
contributors:
    - ["Preet Shihn", "https://github.com/pshihn"]
    - ["Anton Zhiyanov", "https://antonz.org"]
---

[Rough.js](https://roughjs.com/) is a small (<9KB gzipped) graphics library that lets you draw in a _sketchy_, _hand-drawn_ style. The library defines primitives for lines, curves, arcs, polygons, circles, and ellipses. Rough.js works with both Canvas and SVG.

Let's see Rough.js in action! You can edit the code at any time and click the Run button to see the updated drawing.

[Basic usage](#basic-usage) ·
[Shapes](#shapes) ·
[Filling](#filling) ·
[Sketching style](#sketching-style) ·
[Further reading](#further-reading)

<div class="tryx__panel">
<p>✨ This is an open source guide. Feel free to <a href="https://github.com/nalgeon/tryxinyminutes/blob/main/try/rough-js/index.md">improve it</a>!</p>
</div>

## Basic usage

You can draw on a `canvas`:

```js
// `out-canvas` is a `canvas` tag
// defined below this code snippet
const canvas = document.getElementById("out-canvas");

// clear the canvas for redrawing
const context = canvas.getContext("2d");
context.clearRect(0, 0, canvas.width, canvas.height);

// draw a rectangle
const rc = rough.canvas(canvas);
rc.rectangle(10, 10, 200, 100); // x, y, width, height

// show the drawing
canvas.style.display = "unset";
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" output-mode="hidden">
</codapi-snippet>

<canvas id="out-canvas" width="250" height="120" style="display: none"></canvas>

Or generate an `svg`:

```js
// `out-svg` is an `svg` tag
// defined below this code snippet
const svg = document.getElementById("out-svg");

// clear the svg for redrawing
svg.innerHTML = "";

// draw a rectangle
const rs = rough.svg(svg);
const node = rs.rectangle(10, 10, 200, 100); // x, y, width, height

// show the drawing
svg.appendChild(node);
svg.style.display = "unset";
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" output-mode="hidden">
</codapi-snippet>

<svg id="out-svg" width="250" height="120" style="display: none"></svg>

Both `RoughCanvas` (an object returned by `rough.canvas`) and RoughSVG (an object returned by `rough.svg`) provide the same methods. The difference is that the `RoughSVG` methods return a node (`g`) that can be inserted into the SVG DOM.

I will be using `RoughCanvas` from now on.

## Shapes

Draw a **line** from `(x1, y1)` to `(x2, y2)`:

```js
// `init` is a helper function I'm using in the examples.
// It initializes a RoughCanvas on the element with the given ID
// just like we did above.
rc = init("c01");

rc.line(10, 10, 160, 60);
rc.line(10, 40, 160, 90, { strokeWidth: 5 });
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c01" width="250" height="110" style="display: none"></canvas>

Draw a **rectangle** with the top-left corner at `(x, y)` and the specified `width` and `height`:

```js
rc = init("c02");

rc.rectangle(10, 10, 100, 50);
rc.rectangle(130, 10, 100, 50, { fill: "red" });
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c02" width="250" height="80" style="display: none"></canvas>

Draw a **circle** with the center at `(x, y)` and the specified `diameter`:

```js
rc = init("c03");

rc.circle(60, 50, 80);
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c03" width="250" height="110" style="display: none"></canvas>

Draw an **ellipse** with the center at `(x, y)` and the specified `width` and `height`:

```js
rc = init("c04");

rc.ellipse(60, 40, 100, 60);
rc.ellipse(180, 40, 100, 60, { fill: "blue", stroke: "red" });
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c04" width="250" height="90" style="display: none"></canvas>

Draw a **set of lines** connecting the specified `points`:

```js
rc = init("c05");

// linearPath accepts an array of points.
// Each point is an array with values [x, y].
rc.linearPath([
    [10, 60],
    [60, 10],
    [110, 60],
    [160, 10],
    [210, 60],
]);
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c05" width="250" height="80" style="display: none"></canvas>

Draw a **polygon** with the specified `vertices`:

```js
rc = init("c06");

// polygon accepts an array of points.
// Each point is an array with values [x, y].
rc.polygon([
    [10, 60],
    [60, 10],
    [110, 40],
    [160, 10],
    [210, 60],
]);
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c06" width="250" height="80" style="display: none"></canvas>

Draw an **arc**:

```js
// An arc is described as a section of an ellipse.
arc(x, y, width, height, start, stop, closed [, options]) {}
// x, y          - the center of the ellipse.
// width, height - the dimensions of the ellipse.
// start, stop   - the start and stop angles for the arc.
//
// closed is a boolean.
// If true, the end points of the arc will be connected to the center.

```

```js
rc = init("c07");

// upper left segment
rc.arc(60, 50, 100, 80, Math.PI, Math.PI * 1.6, true);

// lower right segment
rc.arc(60, 50, 100, 80, 0, Math.PI / 2, true, {
    stroke: "red",
    strokeWidth: 4,
    fill: "rgba(255,255,0,0.4)",
    fillStyle: "solid",
});

// lower left segment
rc.arc(60, 50, 100, 80, Math.PI / 2, Math.PI, true, {
    stroke: "blue",
    strokeWidth: 2,
    fill: "rgba(255,0,255,0.4)",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c07" width="250" height="110" style="display: none"></canvas>

Draw a **curve** passing through the points:

```js
rc = init("c08");

// curve accepts an array of points.
// Each point is an array with values [x, y].
rc.curve([
    [10, 60],
    [60, 10],
    [110, 60],
    [160, 10],
    [210, 60],
]);
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c08" width="250" height="80" style="display: none"></canvas>

Draw a **path** described by an [SVG path](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths) data string:

```js
rc = init("c09");

rc.path("M 10 80 C 40 10, 65 10, 95 80 S 150 150, 180 80", {
    stroke: "blue",
    fill: "green",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c09" width="250" height="150" style="display: none"></canvas>

## Filling

The `fill` property specifies the color used to fill a shape. Uses hachure by default:

```js
rc = init("c11");

rc.circle(50, 50, 80, { fill: "red" });
rc.rectangle(110, 10, 80, 80, { fill: "red" });
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c11" width="210" height="110" style="display: none"></canvas>

You can change the hatch angle and line thickness, or choose a different fill style altogether:

```js
rc = init("c12");

// hatch angle
rc.rectangle(10, 10, 80, 80, {
    fill: "red",
    hachureAngle: 60,
    hachureGap: 8,
});

// thicker lines
rc.circle(150, 50, 80, {
    fill: "rgb(10,150,10)",
    fillWeight: 3,
});

// solid fill instead of hachure
rc.rectangle(210, 10, 80, 80, {
    fill: "rgba(255,0,200,0.2)",
    fillStyle: "solid",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c12" width="310" height="110" style="display: none"></canvas>

You can use the following fill styles:

```js
rc = init("c13");

// hachure (default) draws sketchy parallel lines.
rc.rectangle(10, 10, 80, 80, {
    fill: "blue",
});

// solid is more like a conventional fill.
rc.rectangle(110, 10, 80, 80, {
    fillStyle: "solid",
    fill: "blue",
});

// zigzag draws zigzag lines that fill the shape.
rc.rectangle(210, 10, 80, 80, {
    fillStyle: "zigzag",
    fill: "blue",
});

// cross-hatch is similar to hachure, but draws cross-hatched lines.
rc.rectangle(310, 10, 80, 80, {
    fillStyle: "cross-hatch",
    fill: "blue",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c13" width="410" height="110" style="display: none"></canvas>

And even more:

```js
rc = init("c14");

// dots fills the shape with sketchy dots.
rc.rectangle(10, 10, 80, 80, {
    fillStyle: "dots",
    fill: "blue",
});

// dashed is similar to hachure, but the lines are dashed.
rc.rectangle(110, 10, 80, 80, {
    fillStyle: "dashed",
    fill: "blue",
    dashOffset: 5,
    dashGap: 5,
});

// zigzag-line is similar to hachure,
// but the lines are drawn in a zigzag pattern.
rc.rectangle(210, 10, 80, 80, {
    fillStyle: "zigzag-line",
    fill: "blue",
    zigzagOffset: 3,
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c14" width="410" height="110" style="display: none"></canvas>

## Sketching style

You can control the _roughness_ of the drawing:

```js
rc = init("c21");

// A rectangle with the roughness of 0 would be a perfect rectangle.
rc.rectangle(10, 10, 80, 80, {
    roughness: 0,
    fill: "red",
});

// The default value is 1.
rc.rectangle(110, 10, 80, 80, {
    roughness: 1,
    fill: "blue",
});

// There is no upper limit to this value.
rc.rectangle(210, 10, 80, 80, {
    roughness: 3,
    fill: "green",
});

// But a value over 10 is mostly useless.
rc.rectangle(310, 10, 80, 80, {
    roughness: 5,
    fill: "gold",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c21" width="410" height="110" style="display: none"></canvas>

You can also control _bowing_:

```js
rc = init("c22");

// Bowing indicates how curvy the lines are.
// A value of 0 will cause straight lines.
rc.rectangle(10, 10, 80, 80, {
    bowing: 0,
    fill: "red",
});

// The default value is 1.
rc.rectangle(110, 10, 80, 80, {
    bowing: 1,
    fill: "blue",
});

// There is no upper limit to this value.
rc.rectangle(210, 10, 80, 80, {
    bowing: 8,
    fill: "green",
});

// But a value over 20 is mostly useless.
rc.rectangle(310, 10, 80, 80, {
    bowing: 20,
    fill: "gold",
});
```

<codapi-snippet engine="browser" sandbox="javascript" editor="basic" template="render.js" output-mode="hidden">
</codapi-snippet>

<canvas id="c22" width="410" height="110" style="display: none"></canvas>

## Further reading

Rough.js also supports other configuration options. See the [documentation](https://github.com/rough-stuff/rough/wiki) for details.

See the [project page](https://roughjs.com/) to start using Rough.js on your website or in your product.

<script src="https://unpkg.com/roughjs@4.6.6/bundled/rough.js" defer></script>
**interact.js is a JavaScript library for drag and drop, resizing and
multi-touch gestures with inertia and snapping for modern browsers** (and also
IE9+).

What it tries to do is **present pointer input data consistently** across
different browsers and devices and provide convenient ways to **pretend that the
user's pointer moved in a way that it wasn't really moved** (snapping, inertia,
etc.).

The `interact` function takes an element or a CSS selector string returns an
`Interactable` object which has various methods to configure actions and event
listeners. A sequence of pointer down, move and up inputs can lead to
`InteractEvent`s being fired. If you add event listeners for an event type,
those listener functions are given an `InteractEvent` object which provides
pointer coordinates and speed and, in gesture events, scale, distance, angle,
etc.

Note that **interact.js doesn't move elements for you**.  Styling an
element so that it moves while a drag happens has to be done from your own event
listeners. This way, you’re in control of everything that happens.

The basic steps to setting up your targets are:

 1. Create an `Interactable` target.
 2. Configure it to enable actions and add modifiers, inertia, etc.
 3. Add event listeners to provide visual feedback and update your app's state.

For example, here's some code for [a very simple slider input][slider-demo]:

```js
// Step 1
const slider = interact('.slider')    // target elements with the "slider" class

slider
  // Step 2
  .draggable({                        // make the element fire drag events
    origin: 'self',                   // (0, 0) will be the element's top-left
    inertia: true,                    // start inertial movement if thrown
    modifiers: [
      interact.modifiers.restrict({
        restriction: 'self'            // keep the drag coords within the element
      })
    ]
  })
  // Step 3
  .on('dragmove', function (event) {  // call this listener on every dragmove
    const sliderWidth = interact.getElementRect(event.target.parentNode).width
    const value = event.pageX / sliderWidth

    event.target.style.paddingLeft = (value * 100) + '%'
    event.target.setAttribute('data-value', value.toFixed(2))
  })
```

<iframe height="265" style="width: 100%;" scrolling="no" title="interact.js
simple slider"
src="https://codepen.io/taye/embed/GgpxNq/?height=265&theme-id=dark&default-tab=result"
frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/taye/pen/GgpxNq/'>interact.js simple slider</a> by Taye A
  (<a href='https://codepen.io/taye'>@taye</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

Installation
============

```sh
$ npm install --save interactjs
```

```js
import interact from 'interactjs'

// or if using commonjs or AMD
const interact = require('interactjs')
```

If you're using [npm](npm), install the package as a dependency with `npm install
interactjs` then import or require the package in your JavaScript file.

```sh
$ npm install --save-dev @interactjs/types
```

If you're using TypeScript, also install the `@interactjs/types` package as a
dev dependency.

CDN
---

```html
<script src="https://cdn.jsdelivr.net/npm/interactjs@next/dist/interact.min.js"></script>
<!-- or -->
<script src="https://unpkg.com/interactjs@next/dist/interact.min.js"></script>
```

You can also use the [jsDelivr](https://www.jsdelivr.com/package/npm/interactjs)
or [unpkg](https://unpkg.com/interactjs@next) CDNs by adding a `<script>` tag
pointing to their servers.

`interact` is exposed as a CommonJS module, an AMD module, or a global variable
depending on what the environment supports.

Drag, Resize and Gesture Actions
================================

The `Interactable` methods `draggable()`, `resizable()` and `gesturable()` are
used to enable and configure actions for target elements. They all have some
common options as well as some action-specific options and event properties.

The common options for all actions include:

 - A `modifiers` array for applying snap, restrict, etc. to the action events
 - A `listeners` array or object for adding action event listener functions.
 - `onstart`, `onmove` and `onend` functions to add single listeners for action
   start, move and end events respectively.

Drag and resize actions (**but not gesture**) can also have an `inertia`
property which may be a `boolean` to enable or diable inertia, or an object with
inertia configuration.

Draggable
==========

<!-- TODO -->
 - for watching the pointer go down, move, then go back up
 - combined with dropzones

Dropzone
=========

<!-- TODO -->
 - use this to define elements that other draggable elements can be moved into
 - doesn't actually re-parent the draggable elements; that's up to you

Resizable
==========

<!-- TODO -->
 - for watching the size and position of an element while the pointer is used to
   move one or two of the element's edges
 - `event.rect` and `event.deltaRect`
 - there are some modifiers that are specifically for resizing which change the
   way the element's edges and size are updated in response to the pointer

Gesturable
==========

<!-- TODO -->
 - for 2-finger gestures
 - provide angle, scale and distance

[slider-demo]: https://codepen.io/taye/pen/GgpxNq
[npm]: https://docs.npmjs.com/about-npm/

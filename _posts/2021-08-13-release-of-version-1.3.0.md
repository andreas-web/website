---
layout: post
title: Release of 1.3.0
subtitle: Major new release
date: 2021-08-13T09:00:00+00:00
categories:
  - Releases
---


### Responsiveness

In version 1.3.0, JSXGraph makes major leaps towards usability in responsive web pages. Following the 
suggestions by Murray Bourne (see his talk at <http://jsxgraph.org/conf/program/>), 
JSXGraph recomputes the board's dimensions when the JSXGraph `div` tag is resized.
This is also the case when the div element's CSS property `display` switches from `none` to `block`.
Therefore, also the initialisation of JSXgraph boards in dia shows or register tabs should no longer be a problem.
No action as to be taken by the developer to make responsiveness available, it is activated automatically.
This feautrue can be turned off by

```javascript
    var board = JXG.JSXGraph.initBoard('jxgbox', {
        boundingbox: [-5,5,5,-5], 
        resize: {enabled: false}
    });
```

Here is are two examples how to use JSXGraph such that the ratio between height and width 
of the JSXGraph div stays constant. This has to be implemented by the developer. There are two 
possible approaches, one is an old hack, the other is a new CSS feature. 

1) The first version is an old hack known among CSS developers, see <https://bourne2learn.com/math/jsxgraph/jsxgraph-examples.php>
for a more detailed explanation:

```html
    <style>
        div.div1x1 {
            height: 0;
            padding-bottom: 100%;
        }

        div.jsxwrap500px {
            max-width: 500px;
            min-width: 100px;
            margin: 0 auto;
            overflow:hidden;
        }
    </style>
    <div class="jsxwrap500px">
        <div id='jxgbox' class='jxgbox div1x1'></div>
    </div>
    <script>
    var pol = board.create('polygon', [[-3,-3], [3,-3], [1,4]], {
            fillColor: 'yellow'
    });
    </script>
```

2) The second version uses the new CSS property `aspect-ratio`, which is - in August 2021 - supported 
by all major browsers beside Safari.

```html
    <style>
        div.jxgnew {
            aspect-ratio: 1/1;
            max-width: 500px;
            margin: 0 auto;
            overflow:hidden;
        }
    </style>
    <div id='jxgbox' class='jxgbox jxgnew'></div>
    <script>
    var pol = board.create('polygon', [[-3,-3], [3,-3], [1,4]], {
            fillColor: 'yellow'
    });
    </script>
```

Another suggestion from the user community is to allow dragging of points outside of the board. This may be useful on mobile devices
but poses the thread that the dragged point is "lost". To be on the safe side, use it for construction whose
only interactive elements are sliders. The attribute `moveTarget` takes the DOM object which captures `move` events.

```javascript
    var board = JXG.JSXGraph.initBoard('jxgbox', {
        boundingbox: [-5,5,5,-5], 
        axis: true,
        moveTarget: document
    });
```

Finally, texts support now the attribute `fontUnit` which accepts beside the default value `px` all CSS units like `vw`, `vmin`, `vmax`, ...

### Accessibility

Starting with version 1.3.0, JSXgraph can be controlled with the keyboard.

- The user can select JSXGraph objects with the `tab` key.
- Objects can be moved with the arrow keys.
- keyboard control is enabled by default, it can be turned off with the attribute `keyboard`, see the example below.
- Further, the step width can be set with `dx` and `dy`.
- If the user presses the `shift` key the construction is "panned", i.e. the view port is shifted.

```html
var board = JXG.JSXGraph.initBoard("jxgbox", {boundingbox: [-5,5,5,-5], 
    axis: true, 
    keyboard: {
        enabled: true,
        dy: 30,
        dx: 30,
        panShift: true,
        panCtrl: false
      }
    });
var circ = board.create('circle', [[-4, 3], 1]);
var seg = board.create('segment', [[-2, 0], [-2, 4]]);
```

### Further new features

- There is a new arrow head `type:7` which is especially well suited for curves. The arrow head ends exactly where the curve ends.
- To end a line / curve exactly at its delimiting coordinates, it may be necessary to set the attribute `lineCap:'square'`. 
This attribute is new for curves.
- There are some new math functions: erf, erfc, erfi, ndtr, ndtri, asinh, acosh in JXG.Math available. erf is the error function.
- Comparisons and logical operators are now available as functions in JXG.Math: lt, gt, leq, geq, eq, neq, and, or, not, xor.
These functions allow to avoid `<`, `>` and other characters in environments which filter out any occurrence of HTML tags.
- New boolean method `point.isOn(element)`

### Other

- The JessieCode script tag supports now the attribute `src` to load code from an external file:

```html
<script type="text/jessiecode" src="testsrc.jc" axis="true">
 // ...
</script>
```

- TypeScript: `index.d.ts` has been improved and updated
- As usual, several bugs have been fixed and the API documentation has been improved, see 
<https://github.com/jsxgraph/jsxgraph/blob/master/CHANGELOG.md> for a more detailed list of changes.


Enjoy, 

Matthias Ehmann, Michael Gerhäuser, Carsten Miller, Andreas Walter, and Alfred Wassermann
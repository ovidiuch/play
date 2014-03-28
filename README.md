Play.js
====
Agnostic frame-skipping animation library

Performs an abstract transition for a fixed amount of time, regardless of the
frame rate of the page (frame skipping.) Calls an `onFrame` callback with a
0-to-1 float ratio (representing the progress of the transition) as often as
the browser can perform.

```js
// Run a transition for one second
Play.start({
  time: 1,
  onFrame: function(r) {
    console.log('Animation is at ' + (r * 100).toFixed(2) + '%');
  }
});

// A custom transition easing can be set using the "easing" option, otherwise
// it defaults to linear.
Play.start({
  time: 0.1,
  easing: function(r) {
    // Similar to the default jQuery "swing" easing ($.easing.swing)
    return 0.5 - Math.cos(r * Math.PI) / 2;
  },
  onFrame: function(r) {
    // The first and last values will be closer, as the transition grows faster
    // in middle
    console.log(r);
  }
});

// Starting an animation returns a reference to its instance, exposing a few
// API methods
var myAnimation = Play.start({time: 5});

// You can stop an animation at any time...
Play.stop(myAnimation);
// ...or end it. The difference being that this time the onFrame handler will
// also be called on last time, with maximum ratio (1), as if the entire time
// for that animation had passed
Play.end(myAnimation);

// Another thing you can do is start a transition from a specific point in its
// development. This animation will go from 0.5 to 1 ratio and will last 500ms
Play.start({time: 1, ratio: 0.5});

// The beauty of the onFrame handler lays in its simplicity. You can use the
// frame ratio in any way you'd like, from rendering a progress bar to all
// sorts af complex DOM transitions
Play.start({time: 5, function(r) {
  $('.my-element').css({top: r / 2, left: r});
}});

// Play.js can also be installed as a npm package
var Play = require('play-js').Play;
```

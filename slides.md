:::: {.slide}

![](img/logo-main.png)

## Crash Course: Building Games with JavaScript

::::

## Parts of a game

* Drawing
* The event loop
* Collision detection
* Responding to events

## Drawing

* We'll use the `canvas` element
* Other options: DOM + CSS, SVGs

## Getting the canvas and context

```js
var canvas = document.getElementById("game-canvas");
var ctx = canvas.getContext("2d");
```

## Drawing shapes

```js
ctx.fillStyle = "rgb(0, 0, 0)";
ctx.fillRect(0, 0, x, y);
```

## Drawing text

```js
ctx.font = "24px sans-serif";
ctx.textAlign = "center";
ctx.fillStyle = "white";
ctx.fillText("Press UP to play", x, y);
```

## Drawing images

```js
var image = new Image();
image.src = "image.jpg";
ctx.drawImage(image, x, y, width, height);
```

## Sprites

<div style="background-color: black;">
<img src="/img/explosion.png">
</div>

## Using sprites

```js
var sprite = new Image();
sprite.src = "sprite.jpg";
ctx.drawImage(sprite, 
  sx, sy, sWidth, sHeight, 
  dx, dy, dWidth, dHeight);
```

## The event loop

In any game, you need to update all the game entities (the player, enemies, etc) and re-draw them. In many programming languages, you'd use a loop for that. In JavaScript, we use `requestAnimationFrame`.

## requestAnimationFrame

`requestAnimationFrame` tells the browser to call a function that you provide it on the next repaint. In order to repeatedly do this, you have to call `requestAnimationFrame` again from your function. Repaints happen generally 60 times/sec.

```js
function run() {
  update();
  draw();
  requestAnimationFrame(run);
}

requestAnimationFrame(run);
```

## Collision detection

```js
// The universal collision detection function for rectangles
colliding(obj1, obj2) {
  return !(
        obj1 === obj2 ||
        obj1.center.x + obj1.size.x / 2 <
            obj2.center.x - obj2.size.x / 2 ||
        obj1.center.y + obj1.size.y / 2 <
            obj2.center.y - obj2.size.y / 2 ||
        obj1.center.x - obj1.size.x / 2 >
            obj2.center.x + obj2.size.x / 2 ||
        obj1.center.y - obj1.size.y / 2 > 
            obj2.center.y + obj2.size.y / 2
      );
  }
```

## An explanation

```js
// are these things equal?
obj1 === obj2 || 
// is the right of obj1 left of the left of obj2?
obj1.center.x + obj1.size.x / 2 < 
  obj2.center.x - obj2.size.x / 2 ||
// is the bottom of obj1 above the top of obj2?
obj1.center.y + obj1.size.y / 2 < 
  obj2.center.y - obj2.size.y / 2 ||
// is the left of obj1 right of the right of obj2?
obj1.center.x - obj1.size.x / 2 > 
  obj2.center.x + obj2.size.x / 2 ||
// is the top of obj1 below the bottom of obj2?
obj1.center.y - obj1.size.y / 2 > 
  obj2.center.y + obj2.size.y / 2
```

If any of these things are true, the objects are not colliding.

## Responding to events

Use `addEventListener` on the window to watch for key events.

```js
window.addEventListener("keydown", event => {
  // do something
});

window.addEventListener("keyup", e => {
  // do something
});
```

## Keyboarder utility class

```js
class Keyboarder {
  constructor() {
      this.keyState = {};

      window.addEventListener("keydown", e => {
          this.keyState[e.keyCode] = true;
      });

      window.addEventListener("keyup", e => {
          this.keyState[e.keyCode] = false;
      });
  }

  isDown(keyCode) {
      return this.keyState[keyCode] === true;
  }
}
```

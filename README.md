# Graphics Scaling

A simple explanation of letter box scaling with example code.

```js
const canvas = document.querySelector("canvas");

// The pixels the canvas consists of are called logical pixels.
// The logical size of the canvas is defined on the canvas HTML element
// via the height and width attributes.

// The number of pixels that are rendered on the screen to display the canvas
// are called display pixels.
// They can be set via the style.width and style.height properties of the canvas.

// We can should resize the display size of the canvas while leaving the logical size untouched to scale it.

// Let's define the following methods for convenience.
canvas.setDisplayWidth = function (width) {
  this.style.width = width + "px";
};

canvas.setDisplayHeight = function (height) {
  this.style.height = height + "px";
};

function scale() {
  // Get the canvas aspect ratio...
  // Alternatively, we can also say that we are getting the size of the width in respect to the height.

  const canvasAspectRatio = canvas.width / canvas.height;

  // In our case the result will be 1.7
  // This means that the width is 1.7 times the length of the height.
  // e.g. height = 1 , width= 1.7
  //      height = 5, width = 5 * 1.7 = 8.5
  // This relationship must be kept in order to maintain the aspect ratio of our canvas.

  // Now, lets get the aspect ratio of the screen we are trying to display our canvas on.
  const screenWidth = window.innerWidth;
  const screenHeight = window.innerHeight;
  const screenAspectRatio = screenWidth / screenHeight;

  // The value of the screen's aspect ratio may be either bigger or smaller than the canvas's aspect ratio.
  // In other words the length of the width in respect to the height might be even bigger or smaller.

  if (screenAspectRatio > canvasAspectRatio) {
    // Assuming the screen's aspect ratio is 1.8 and the canvas aspect ratio is 1.7 ...
    // This means the target screen's width is 1.8 times its height.
    // This means that the canvas's width is 1.7 times its height.
    // The width of the screen's width in relation to its height is bigger than
    // the width of the canvas's width in relation to its height.
    // We conclude that we can safely accomodate the canvas, without any overflow, by scaling its display height to screens height
    // then calculating the canvas's display width by multiplying it by its aspect ratio in order to maintain the ratio.

    canvas.setDisplayHeight(screenHeight);
    canvas.setDisplayWidth(screenHeight * canvasAspectRatio);

    // The display width of the canvas will never overflow because the canvas display width will never be larger than the
    // the screen width when it's calculated based on the height that its just been scaled to.
    // screenWidth = screenHeight * screenAspectRatio
    // canvasDisplayWidth=  screenHeight * canvasAspectRatio
    // screenWidth > canvasDisplayWidth
    // e.g. screenHeight * 1.8 > screenHeight * 1.7

    // It's important to note that in this case we can not fill the width of the screen
    // and then calculate the height based on the width because the height will overflow.
    // screenHeight = screenWidth / screenAspectRatio
    // canvasHeight = screenWidth / canvasAspectRatio
    // Since screen width is divided by a larger value, it will always be smaller than canvas height
    // as a result the canvas height will overflow and clip through the screen.
    // screenHeight < canvasDisplayHeight
    // e.g. screenWidth / 1.8 < screenWidth / 1.7
  } else {
    // If the screen's aspect ratio is smaller than than the canvas's aspect ratio...
    // Assuming the screen's aspect ratio is 1.6 and the canvas aspect ratio is 1.7 ...
    //  we can't fill the height then calculate the width because the canvas width will overflow, see comments
    // above.
    // screenWidth = screenHeight * screenAspectRatio
    // canvasDisplayWidth=  screenHeight * canvasAspectRatio
    // screenWidth < canvasDisplayWidth
    // e.g. screenHeight * 1.6 < screenHeight * 1.7

    // In this case, we can fill the width of the screen then calculate the height and it will fit without overflow.
    // screenHeight = screenWidth / screenAspectRatio
    // canvasHeight = screenWidth / canvasAspectRatio
    // Since screen width is divided by a smaller value, screen height  will always be larger than canvas Height
    // screenHeight > canvasDisplayHeight
    // e.g. screenWidth / 1.6 > screenWidth / 1.7
    canvas.setDisplayWidth(screenWidth);
    canvas.setDisplayHeight(screenWidth / canvasAspectRatio);
  }
}

// Scale once at the start
scale();

// Also whenever the screen is resized...
window.onresize = scale;

```




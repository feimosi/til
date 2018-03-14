<h1 align="center">07.03.2018</h1>

## CSS Scroll Snap Points

```html
<div class="container">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

```css
.container {
  scroll-snap-points-y: repeat(100%);
  scroll-snap-type: mandatory;

  > div {
    width: 100vw;
    height: 100vh;
  }
}
```

:arrow_right: https://css-tricks.com/introducing-css-scroll-snap-points/

<h1 align="center">08.03.2018</h1>

## Cast array of strings to numbers

```js
attributes.split(" ").map(Number);
```

### Use SVG viewBox for zooming on hover

```js
const svgElement = ... // Get the SVG element
const [,,originalWidth, originalHeight] = svgElement.getAttribute("viewBox").split(" ").map(Number);

svgElement.addEventListener("mousemove", (event) => {
  const {top, left, width, height} = svgElement.getBoundingClientRect();

  const eventTop = event.clientY - top;
  const eventLeft = event.clientX - left;

  svgElement.setAttribute(
    "viewBox", 
    `${eventLeft / width * originalWidth - originalWidth / 4} ${
      eventTop / height * originalHeight - originalHeight / 4} ${
      originalWidth / 2} ${
      originalHeight / 2
     }`)
});
svgElement.addEventListener("mouseout", () => {
  svgElement.setAttribute("viewBox", `0 0 ${originalWidth} ${originalHeight}`);
});
```

:arrow_right: https://mailchi.mp/f423b0d7cc95/js-tips-combine-validators-1287685

<h1 align="center">14.03.2018</h1>

## Vim: Delete everything above a certain line

```
dgg
```

`d` is the deletion command, and `gg` is a movement command that says go to the top of the file,

`kdgg` delete all lines above the current one (`k` is moving the cursor up a line).

`dG` will delete all lines at or below the current one

<h1 align="center">01.11.2017</h1>

## Free Hand drawing on a Canvas using RxJS

```ts
private captureEvents(canvasEl: HTMLCanvasElement) {
  Observable
    // this will capture all mousedown events from teh canvas element
    .fromEvent(canvasEl, 'mousedown')
    .switchMap((e) => {
      return Observable
        // after a mouse down, we'll record all mouse moves
        .fromEvent(canvasEl, 'mousemove')
        // we'll stop (and unsubscribe) once the user releases the mouse
        // this will trigger a mouseUp event    
        .takeUntil(Observable.fromEvent(canvasEl, 'mouseup'))
        // pairwise lets us get the previous value to draw a line from
        // the previous point to the current point    
        .pairwise()
    })
    .subscribe((res: [MouseEvent, MouseEvent]) => {
      const rect = canvasEl.getBoundingClientRect();

      // previous and current position with the offset
      const prevPos = {
        x: res[0].clientX - rect.left,
        y: res[0].clientY - rect.top
      };

      const currentPos = {
        x: res[1].clientX - rect.left,
        y: res[1].clientY - rect.top
      };
    
      // this method we'll implement soon to do the actual drawing
      this.drawOnCanvas(prevPos, currentPos);
    });
}

private drawOnCanvas(
  prevPos: { x: number, y: number }, 
  currentPos: { x: number, y: number }
) {
  // incase the context is not set
  if (!this.cx) { return; }

  // start our drawing path
  this.cx.beginPath();

  // we're drawing lines so we need a previous position
  if (prevPos) {
    // sets the start point
    this.cx.moveTo(prevPos.x, prevPos.y); // from
    // draws a line from the start pos until the current position
    this.cx.lineTo(currentPos.x, currentPos.y);

    // strokes the current path with the styles we set earlier
    this.cx.stroke();
  }
}
```

:arrow_right: https://medium.com/@tarik.nzl/creating-a-canvas-component-with-free-hand-drawing-with-rxjs-and-angular-61279f577415

<h1 align="center">04.11.2017</h1>

## CSS Heading with a stroke line

```css
h2 {
  color: white;
  line-height: 2;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-weight: 100;
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  grid-gap: 20px;
  align-items: center;
}

h2:before,
h2:after {
  background: white;
  display: block;
  height: 2px;
  width: 100%;
  content: '';
}
```

:arrow_right: https://codepen.io/wesbos/pen/OONNKX

## Wrap the generator in an iterable

To allow multiple iterations, wrap the generator function in an iterable that creates a new instance every time:

```js
const wrapGenerator = (gen) => (...args) => ({
  [Symbol.iterator]: () => gen(...args)
});

const wrappedGenerator = wrapGenerator(gen);

const printTwice = (it) => {
  console.log(Array.from(it));
  console.log(Array.from(it));
};

printTwice(wrappedGenerator());
```

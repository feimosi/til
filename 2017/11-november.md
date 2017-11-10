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

<h1 align="center">05.11.2017</h1>

## Flow `Shape<T>` type

Matches the shape of T. React uses `$Shape` in the signatures for `setProps` and `setState`.

An object of type `$Shape<T>` does not have to have all of the properties that type T defines. But the types of the properties that it does have must match the types of the same properties in T.

<h1 align="center">10.11.2017</h1>

## Flow `ReadOnly<T>` type

Makes all properties on objects read-only. 

`$ReadOnly<{ a: number, b: string }>` behaves as if you wrote `{ +a: number, +b: string }`.

```ts
type O = { a: number, b: string };

function fn(o: $ReadOnly<O>) {
  o.a = 42; // Error!
}
```

## React Portal in a separate window

```jsx
class MyWindowPortal extends React.PureComponent {
  constructor(props) {
    super(props);
    // STEP 1: create a container <div>
    this.containerEl = document.createElement('div');
    this.externalWindow = null;
  }
  
  render() {
    // STEP 2: append props.children to the container <div> that isn't mounted anywhere yet
    return ReactDOM.createPortal(this.props.children, this.containerEl);
  }

  componentDidMount() {
    // STEP 3: open a new browser window and store a reference to it
    this.externalWindow = window.open('', '', 'width=600,height=400,left=200,top=200');

    // STEP 4: append the container <div> (that has props.children appended to it) 
    // to the body of the new window
    this.externalWindow.document.body.appendChild(this.containerEl);
  }

  componentWillUnmount() {
    // STEP 5: This will fire when this.state.showWindowPortal 
    // in the parent component becomes false.
    // So we tidy up by closing the window
    this.externalWindow.close();
  }
}
```

:arrow_right: https://hackernoon.com/using-a-react-16-portal-to-do-something-cool-2a2d627b0202

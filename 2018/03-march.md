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

<h1 align="center">18.03.2018</h1>

## The stacking context (z-index)

![](https://developer.mozilla.org/@api/deki/files/913/=Understanding_zindex_04.png)

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context


<h1 align="center">22.03.2018</h1>

## Git: double-dot ".." and triple-dot "..." difference

### Diff

![](https://i.stack.imgur.com/a8HHAm.png)
![](https://i.stack.imgur.com/6K2Rlm.png)

### Log

![](https://image.slidesharecdn.com/gitshakespearemergingrebasing-150902074501-lva1-app6891/95/grokking-git-with-shakespeare-14-638.jpg)

## Log commits with changed files
```git
git log --name-only
```
```git
git log --numstat
```

:arrow_right: https://stackoverflow.com/a/24186641/7350152

<h1 align="center">25.03.2018</h1>

## ES2018: Async interators

```js
async function asyncSum(a){
  let sum = 0;
  for await (const x of a) {
    sum += x;
  }
  return sum;
}
```

```js
interface AsyncIterable {
  [Symbol.asyncIterator](): AsyncIterator;
}

interface AsyncIterator {
  next(): Promise<IterResultObject>;
}

interface IterResultObject {
  value : any;
  done : boolean;
}
```

All iterables are implicitly async-iterable via so-called Async-from-Sync Iterator Objects™.

```js
asyncSum([1, 2, 3]).then(console.log);
// → 6
```

Node streams are async-iterable starting with Node 10 (WARNING: the feature is considered experimental!).

```js
const fs = require('fs');
(async () =>{
  const readStream = fs.createReadStream('file');
  for await (const chunk of readStream) {
    console.log(chunk);
  }
})();
```

## ES2018: Regex Lookahead / Lookbehind

```js
// Positive lookahead:
const pattern =/\d+(?= dollars)/u;
const result = pattern.exec('42 dollars');
// → result[0] === '42'
```

```js
// Negative lookahead:
const pattern =/\d+(?! dollars)/u;
const result = pattern.exec('42 pesos');
// → result[0] === '42
```

```js
// Positive lookbehind:
const pattern =/(?<=\$)\d+/u;
'Price: $42'.replace(pattern,'9001');
// → 'Price: $9001'
```

```js
// Negative lookbehind:
const pattern =/(?<!\$\d*)\d+/gu;
'My 17 children have $42 and €3 each.'.replace(pattern,'9001');
// → 'My 9001 children have $42 and €9001 each.'
```

<h1 align="center">30.03.2018</h1>

## New in React 16.3

### `createContext` API

```jsx
import React from "react";
import { render } from "react-dom";

const MyContext = React.createContext("Hello");

const MyProvider = ({children}) =>
  <MyContext.Provider value="world">
    {children}
  </MyContext.Provider>;

const App = () => (
  <MyProvider>
    <MyContext.Consumer>
      {value => <h1>Hello, {value}!</h1>}
    </MyContext.Consumer>
  </MyProvider>
);

render(
  <App />,
  document.getElementById("⚛️")
);
```

*Use cases*
- Reduce Prop Drilling
- Theming
- Localization
- Data Flow (Unstated)

### `createRef` API

```jsx
class Hello extends React.Component {
  myRef = React.createRef();
  render() {
    return <section>
      <input type="text" ref={this.myRef} />
    </section>;
  }
  componentDidMount() {
    this.myRef.current.focus();
  }
}
```

### `forwardRef` API

```jsx
const MyInput = React.forwardRef((props, ref) => (
  <div className="MyInput">
    <input type="text" ref={ref} {...props} />
  </div>
));

class Hello extends React.Component {
  myRef = React.createRef();
  render() {
    return <section>
        <MyInput ref={this.myRef} />
    </section>;
  }
  componentDidMount() {
    this.myRef.current.focus();
  }
}
```

### Deprecations

The following methods will soon be deprecated and prepended with UNSAFE_

- componentWillMount
- componentWillUpdate
- componentWillReceiveProps

### `getDerivedStateFromProps`

```jsx
class MyClass extends Component {
  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.value !== prevState.value) {
      return({value: nextProps.value});
    }
  }
}
```

### `getSnapshotBeforeUpdate`

```jsx
class MyClass extends Component {
  listRef = React.createRef();
  getSnapshotBeforeUpdate(prevProps, prevState) {
    return prevProps.list.length < this.props.list.length ?
      this.listRef.scrollHeight : null;
  }
  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot !== null) {
      const listRef = this.listRef;
      listRef.scrollTop += listRef.scrollHeight - snapshot;
    }
  }
  render() { return <div ref={this.listRef}>{ /*...*/ }</div>; }
}
```

:arrow_right: http://elijahmanor.com/talks/react-16-3/

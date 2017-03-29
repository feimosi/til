<h1 align="center">03.03.2017</h1>

## CSS

### Apply `box-sizing` globally

```css
html {
  box-sizing: border-box;
}

*, *::before, *::after {
  box-sizing: inherit;
}
```

### Comma-separated lists

```css
ul > li:not(:last-child)::after {
  content: ",";
}
```

### Select first n items

```css
/* select items 1 through 3 and display them */
li:nth-child(-n+3) {
  display: block;
}
```

### Accessibility fallback for icon-only buttons

```css
.no-svg .icon-only:after {
  content: attr(aria-label);
}
```

### Keep cells in a table at equal width

```css
.calendar {
  table-layout: fixed;
}
```

### Style "default" links

```css
a[href]:not([class]) {
  color: #008000;
  text-decoration: underline;
}
```

:arrow_right: https://github.com/AllThingsSmitty/css-protips

<h1 align="center">10.03.2017</h1>

## React patterns

### Handling multiple inputs in controlled components 

```jsx
class Reservation extends React.Component {
  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            value={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

### Autobind labels to inputs with unique IDs

```jsx
let count = 1;
export const getNextId = () => `element-id-${count++}`;
// Reset during app bootstrap phase (for SSR)
export const resetId = () => count = 1;

class Input extends React.Component {
  constructor(props) {
    super(props);
    this.id = getNextId();
  }
  
  onChange = (e) => {
    this.props.onChange(e.target.value);
  }

  render() {
    return (
      <label htmlFor={this.id}>
        {this.props.label}
      </label>
      <input
        id={this.id}
        value={this.props.value} 
        onChange={this.onChange}
      />
    );
  }
}
```

### Switching component pattern

```jsx
const PAGES = {
  home: HomePage,
  about: AboutPage,
  user: UserPage,
};

const Page = (props) => {
  const Handler = PAGES[props.page] || FourOhFourPage;
  return <Handler {...props} />
};

Page.propTypes = {
    page: PropTypes.oneOf(Object.keys(PAGES)).isRequired,
};
```

### Autofocus input

```jsx
class SignInModal extends Component {
  componentDidMount() {
    this.InputComponent.focus();
  }
  
  render() {
    return (
      <div>
        <label>User name: </label>
        <Input ref={comp => { this.InputComponent = comp; }} />
      </div>
    )
  }
}
```

### Components for formatting text

```jsx
const Price = (props) => {
    const price = props.children.toLocaleString('en', {
      style: props.showSymbol ? 'currency' : undefined,
      currency: props.showSymbol ? 'USD' : undefined,
      maximumFractionDigits: props.showDecimals ? 2 : 0,
    });
    
    return <span className={props.className}>{price}</span>
};

Price.propTypes = {
  className: React.PropTypes.string,
  children: React.PropTypes.number,
  showDecimals: React.PropTypes.bool,
  showSymbol: React.PropTypes.bool,
};

Price.defaultProps = {
  children: 0,
  showDecimals: true,
  showSymbol: true,
};

const Page = () => {
  const lambPrice = 1234.567;
  const jetPrice = 999999.99;
  const bootPrice = 34.567;
  
  return (
    <div>
      <p>One lamb is <Price className="expensive">{lambPrice}</Price></p>
      <p>One jet is <Price showDecimals={false}>{jetPrice}</Price></p>
      <p>
        Those gumboots will set ya back 
        <Price showDecimals={false} showSymbol={false}>{bootPrice}</Price>
        bucks.
      </p>
    </div>
  );
};
```

:arrow_right: https://hackernoon.com/10-react-mini-patterns-c1da92f068c5#.wkr2ib210

<h1 align="center">12.03.2017</h1>

## Cancellable promise wrapper

```js
const makeCancelable = (promise) => {
  let hasCanceled_ = false;

  const wrappedPromise = new Promise((resolve, reject) => {
    promise.then((val) =>
      hasCanceled_ ? reject({isCanceled: true}) : resolve(val)
    );
    promise.catch((error) =>
      hasCanceled_ ? reject({isCanceled: true}) : reject(error)
    );
  });

  return {
    promise: wrappedPromise,
    cancel() {
      hasCanceled_ = true;
    },
  };
};
```

## Git merge conflicts

The contents from a specific side of the merge can be checked out of the index by using `--ours` or `--theirs`.  
```sh
git checkout --theirs conflict.txt
git add conflict.txt
git commit
```

<h1 align="center">14.03.2017</h1>

## Normalizer functions

Normalizer functions can be used in many cases where you have logic that’s gated with an if-statement.

```js
function normalizeToPx(from, parentValue) {
  if(from.unit === 'px') {
    return from;
  }
 
  return {
    value: (from.value / 100) * parentValue,
    unit: 'px'
  };
}

// ...

totalPixelWidth += normalizeToPx(foo, parentSize).value;
```

:arrow_right: https://codeutopia.net/blog/2017/02/28/simplify-your-javascript-code-with-normalizer-functions/

<h1 align="center">19.03.2017</h1>

## DOMParser vs Range.createContextualFragment()

> A DocumentFragment is a minimal document object that has no parent. It is used as a light-weight version of document to store well-formed or potentially non-well-formed fragments of XML.

`DOMParser` can parse XML or HTML source stored in a string into a DOM Document

```js
const createNode = html =>
  new DOMParser().parseFromString(html, "text/html").body.firstChild;
```

The `Range.createContextualFragment()` method returns a DocumentFragment by invoking the HTML fragment parsing algorithm with the start of the range (the parent of the selected node) as the context node.

```js
const createNode = html =>
  document.createRange().createContextualFragment(html).firstElementChild
```

:exclamation: `DOMParser` is more compute expensive and generates a much heavier objects, so the performance difference is noticeable.

:arrow_right: https://twitter.com/bdc/status/835084223010734080  
:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/Range/createContextualFragment  
:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/DOMParser  

<h1 align="center">22.03.2017</h1>

## CSS Grid

### Fixed

```css
.fixed {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-gap: 4px;
}
```

### Fluid

```css
.fluid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: .25rem;
}
```

### Responsive

```css
.responsive {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  grid-gap: .25rem;
}
```

### Place on a specific column / row

```css
.element:nth-child(3) {
  grid-column: 4 / 4;
  grid-row: 3 / 4;
}
```

### Span across multiple columns / rows

```css
.element:nth-child(3) {
  grid-column: 2 / span 3;
  grid-row: 2 / span 4;
}
```

:arrow_right: https://youtu.be/16enLRDbOyY?list=PLo3w8EB99pqJJEL63RsaRcmArRsSuKu04

<h1 align="center">26.03.2017</h1>

## DOM Keyboard Input

### Old approach

In the legacy API, we use the three properties of KeyboardEvent: `keyCode`, `charCode` and `which`. Properties don’t have the same meaning when handling a key event (keydown or keyup) versus a character event (keypress). `keyCode` on key events tries to be international-friendly (non-dependent on the logical layout).

### New approach

The new API [UI Events](https://w3c.github.io/uievents/) brings two new very useful properties to a KeyboardEvent object: `key` and `code`. 

The property `key` is almost a direct replacement for the previously used `which`, except it’s a lot more predictable.
When the pressed key is a printable character, you get the character in string form. When the pressed key is not a printable character you get a multi-character descriptive string, like 'Backspace', 'Control', 'Enter', 'Tab'.

`code` gives you the physical key that was pressed, in a string form. This means it’s totally independent of the keyboard layout that is being used.


```js
document.addEventListener('keypress', event => 
  console.log('keypress: ', event.key, event.code, event.keyCode))

document.addEventListener('keyup', event => 
  console.log('keyup: ', event.key, event.code, event.keyCode))

keypress:  ф KeyA 1092
keyup:  ф KeyA 65 

keypress:  a KeyA 97
keyup:  a KeyA 65 

String.fromCharCode(97)
"a"

String.fromCharCode(65)
"A"
```

### Cross-browser usage

```js
window.addEventListener('keydown', function(e) {
  // We don't want to mess with the browser's shortcuts
  if (e.defaultPrevented ||
      e.ctrlKey || e.altKey || e.metaKey || e.shiftKey) {
    return;
  }

  // We try to use `code` first because that's the layout-independent property.
  // Then we use `key` because some browsers, notably Internet Explorer and
  // Edge, support it but not `code`. Then we use `keyCode` to support older
  // browsers like Safari, older Internet Explorer and older Chrome.
  switch (e.code || e.key || e.keyCode) {
    case 'KeyW': // This is 'W' on QWERTY keyboards, but 'Z' on AZERTY keyboards
    case 'w':
    case 119: // keyCode for 'w' 
    case 'ArrowUp':
    case 38: // keyCode for arrow up
      changeDirectionUp();
      break;

    // ...
  }
});
```

:arrow_right: https://hacks.mozilla.org/2017/03/internationalize-your-keyboard-controls/

## Create an array containing numbers 1 to N

```js
Array.from(Array(10), (e, i) => i)

// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

<h1 align="center">27.03.2017</h1>

## Equality algorithms

### Strict equality using ===

If the values have different types, the values are considered unequal. Otherwise, if they have the same type and are not numbers, they're considered equal if they have the same value. Finally, if both values are numbers, they're considered equal if they're both not NaN and are the same value, or if one is +0 and one is -0.

> For numbers it uses slightly different semantics to gloss over two different edge cases. The first is that floating point zero is either positively or negatively signed. This is useful in representing certain mathematical solutions, but as most situations don't care about the difference between +0 and -0, strict equality treats them as the same value. The second is that floating point includes the concept of a not-a-number value, NaN, to represent the solution to certain ill-defined mathematical problems: negative infinity added to positive infinity, for example.

Also used by `Array.prototype.indexOf`, `Array.prototype.lastIndexOf` and case-matching.

### Same-value equality

Determines whether two values are functionally identical in all contexts. It's provided by the Object.is method.

```js
Object.is(NaN, NaN)
> true

Object.is(-0, 0)
> false
```

### Same-value-zero equality

Similar to same-value equality, but considered +0 and -0 equal. Used by `TypedArray` and `ArrayBuffer` constructors, as well as `Map` and `Set` operations, and `includes` method.

<h1 align="center">29.03.2017</h1>

> Get into the habit of testing for the existence of a property rather than the truthiness of a property, even though they’re the same thing 99% of the time. `if ('fetch' in window)` rather than `if (window.fetch)`.

> Do not use float to store currency. You will loose precision and data. You should store it as a integer number of cents and then convert prior to output.

```js
var number = 123456.789;

number.toLocaleString('de-DE', { style: 'currency', currency: 'EUR' });
// → 123.456,79 €

console.log(number.toLocaleString('ja-JP', { style: 'currency', currency: 'JPY' }))
// → ￥123,457

console.log(number.toLocaleString('en-IN', { maximumSignificantDigits: 3 }));
// → 1,23,000

console.log(new Intl.NumberFormat('de-DE').format(number));
// → 123.456,789

// Arabic in most Arabic speaking countries uses real Arabic digits
console.log(new Intl.NumberFormat('ar-EG').format(number));
// → ١٢٣٤٥٦٫٧٨٩
```

### Only send polyfills to browsers that need them

```js
import loadPolyfills from './loadPolyfills';
import mountApp from './mountApp'; // the entry point for the rest of my app

loadPolyfills().then(mountApp);
```
```js
import 'core-js/es6/promise';

export default function loadPolyfills() {
  const fillFetch = () => new Promise((resolve) => {
    if ('fetch' in window) return resolve();

    require.ensure([], () => {
      require('whatwg-fetch');

      resolve();
    }, 'fetch');
  });

  const fillIntl = () => new Promise((resolve) => {
    if ('Intl' in window) return resolve();

    require.ensure([], () => {
      require('intl');
      require('intl/locale-data/jsonp/en.js');

      resolve();
    }, 'Intl');
  });

  const fillCoreJs = () => new Promise((resolve) => {
    if (
      'startsWith' in String.prototype &&
      'endsWith' in String.prototype &&
      'includes' in Array.prototype &&
      'assign' in Object &&
      'keys' in Object
    ) return resolve();

    require.ensure([], () => {
      require('core-js');

      resolve();
    }, 'core-js');
  });

  return Promise.all([
    fillFetch(),
    fillIntl(),
    fillCoreJs()
  ]);
}
```

At build-time, Webpack will create your normal bundle, plus three more bundles: fetch.js, Intl.js and core-js.js (although I can’t get this working in Webpack 2.2, I just get 0.js, 1.js, 2.js). When you code runs in the browser, Webpack will fetch these files if it needs to, before executing the rest of your app’s code.

:arrow_right: https://hackernoon.com/polyfills-everything-you-ever-wanted-to-know-or-maybe-a-bit-less-7c8de164e423

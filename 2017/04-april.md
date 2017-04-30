<h1 align="center">09.04.2017</h1>

## HTML

### `<kbd>`

Represents user input and produces an inline element displayed in the browser's default monospace font.

```html
<p>Type the following in the Run dialog: <kbd>cmd</kbd><br />

<p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>
```
:arrow_down:
> <p>Type the following in the Run dialog: <kbd>cmd</kbd><br />
> <p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>

### `<details>`

Is used as a disclosure widget from which the user can retrieve additional information.

```html
<details>
  <summary>Some details</summary>
  <p>More info about the details.</p>
</details>
```
:arrow_down:
> <details>
>  <summary>Some details</summary>
>  <p>More info about the details.</p>
> </details>

### `<wbr>`

Represents a word break opportunity—a position within text where the browser may optionally break a line, though its line-breaking rules would not otherwise create a break at that location.

```html
<p>http://this<wbr>.is<wbr>.a<wbr>.really<wbr>.long<wbr>.example<wbr>.com/With
<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages<wbr></p>

some.very.long<wbr>@email.com
```

## Promise finally

```js
new Promise((resolve, reject) => {
    throw new Error()
})
  .then(_ => console.log('success'))
  .catch(_ => console.log('fail'))
  .then(_ => console.log('finally'))
```

<h1 align="center">11.04.2017</h1>

## DOM

### clientHeight:

Returns the height of the visible area for an object, in pixels. The value contains the height with the padding, but it does not include the scrollBar, border, and the margin.

### offsetHeight:

Returns the height of the visible area for an object, in pixels. The value contains the height with the padding, scrollBar, and the border, but does not include the margin.

So, offsetHeight includes scrollbar and border, clientHeight doesn't.

### [`Document.elementFromPoint()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/elementFromPoint)

Returns the topmost element at the specified coordinates.

```js
elem = document.elementFromPoint(2, 2);
elem.style.color = newColor;
```

### [`Window.getComputedStyle()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle)

Gives the values of all the CSS properties of an element after applying the active stylesheets and resolving any basic computation those values may contain.

```js
var elem = document.getElementById("elem-container");
var theCSSprop = window.getComputedStyle(elem,null).getPropertyValue("height");
document.getElementById("output").innerHTML = theCSSprop;
```

The values returned by getComputedStyle are known as resolved values. These are usually the same as the CSS 2.1 computed values, but for some older properties like width, height or padding, they are instead the used values. 

### [`Window.getSelection()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getSelection)

Returns a Selection object representing the range of text selected by the user or the current position of the caret.

```js
var selObj = window.getSelection(); 
var selRange = selObj.getRangeAt(0);
var selectedText = selObj.toString();
```

<h1 align="center">23.04.2017</h1>

## CSS

### Modern clearfix

CSS now has a way to cause elements to clear floats. We set the value of display to flow-root and our floated box is cleared.

Old approach:
```css
.container::after {
  content: "";
  display: block;
  clear: both;
}
```

New approach:
```css
.container {
  display: flow-root;
}
```

<h1 align="center">30.04.2017</h1>

## React Performance Tips

### `render()` function
- do as little work as possible in the `render` function. If it is necessary to perform complex operations or calculations, consider moving them to a memoized function so that duplicate results can be cached

### State and props
- avoid storing easily computable values in a component’s state
- React will trigger a re-render anytime a prop (or state) value is not equal to the previous value. With this in mind, it is important to be mindful of situations where it’s possible to inadvertently cause a performance hit by creating new values for props or state each render cycle (e.g. function binding, Object or Array literals, fallback /default values)
- keep Props (and State) as simple and minimal as possible

### Component methods
- since component methods are created for each instance of a component, if possible, use either pure functions from a helper/util module or static class methods. This makes a noticeable difference especially in cases where there are a large number of a component being rendered in the app.

### Advanced
- in almost all cases, React.PureComponent is a better choice than React.Component. When creating new components, try building it as a pure component first and only if the component's functionality requires, use React.Component
- component Performance Profiling (in Chrome) with `?react_perf` as a query string
- certain common events can fire extremely rapidly, e.g. ‘scroll’, ‘resize’. It is wise to debounce these events, especially if the event handler is performing anything more than extremely basic functionality

:arrow_right: https://blog.vixlet.com/react-at-light-speed-78cd172a6411

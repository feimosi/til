<h1 align="center">03.08.2017</h1>

## `Event.stopImmediatePropagation()`

If several listeners are attached to the same element for the same event type, they are called in order in which they have been added. If during one such call, event.stopImmediatePropagation() is called, no remaining listeners will be called.

## React Event Delegation

React uses event delegation with a single event listener on document for events that bubble, like 'click' in this example, which means stopping propagation is not possible; the real event has already propagated by the time you interact with it in React. stopPropagation on React's synthetic event is possible because React handles propagation of synthetic events internally.

<h1 align="center">04.08.2017</h1>

## `URLSearchParams`

```js
const urlParams = new URLSearchParams(window.location.search);

console.log(urlParams.has('post')); // true
console.log(urlParams.get('action')); // "edit"
console.log(urlParams.toString()); // "?post=1234&action=edit"
console.log(urlParams.append('active', '1')); // "?post=1234&action=edit&active=1"

const keys = urlParams.keys(); // ['post', 'action']
const entries = urlParams.entries();
```

`URLSearchParams` reminds the `classList` API - very simple methods yet very useful.

## CSS

### `text-indent`

The text-indent CSS property specifies the amount of indentation (empty space) that is put before lines of text in a block. By default, this controls the indentation of only the first formatted line of the block, but the hanging and each-line keywords can be used to change this behavior.

```css
text-indent: 3mm;
text-indent: 5em each-line;
text-indent: 5em hanging;
```

:arrow_right: https://github.com/feimosi/til/edit/master/2017/08-august.md

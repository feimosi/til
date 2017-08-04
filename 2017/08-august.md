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

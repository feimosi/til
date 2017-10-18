<h1 align="center">05.10.2017</h1>

## Redux `createReducer()` util

Switch statements come with too much boilerplate of their own.
The idea is to ditch the switch and move towards a more functional approach.
What we can do is abstract all our switch case logic into “case functions” and create an object that maps action types to their corresponding case functions. We’ll call this object ‘actionHandlers’.

```js
export function createReducer(initialState, actionHandlers) {
  return function reducer(state = initialState, action) {
    if (actionHandlers.hasOwnProperty(action.type)) {
      return actionHandlers[action.type](state, action)
    } else {
      return state
    }
  }
}
```

The above function is verbose right now for explanation point of view. Below is the new and improved create reducer file.

```js
import { propOr, identity } from 'ramda';

export default (initialState, actionHandlers) =>
(state = initialState, action) =>
propOr(identity, action.type, actionHandlers)(state, action);
```

:arrow_down:

```js
import initialState from './initialState';
import createReducer from '../futils/newcreatereducer';

const actionHandlers = {
  GLOBAL_SEARCH: (state, action) => Object.assign({}, state, { results: action.results }),
  UPDATE_GLOBAL_SEARCH_STRING: (state, action) =>
    Object.assign({}, state, { searchString: action.searchString, isLoading: true }),
  IS_LOADING: (state, action) => Object.assign({}, state, { isLoading: action.bool })
};

export default createReducer(initialState.globalSearchState, actionHandlers);
```

:arrow_right: https://medium.freecodecamp.org/reducing-the-reducer-boilerplate-with-createreducer-86c46a47f3e2

## CSS `ch` unit

When defining widths of text blocks the `ch` unit may come in handy since 1ch is equivalent to the width of the zero (0) character - glyph "0". It also changes as the font-family or font-size changes.

```cs
p {
  max-width: 65ch;  /* Maximum width of 65characters */
}
```

<h1 align="center">07.10.2017</h1>

## `tr` command

Translate characters - run replacements based on single characters and character sets.

Replace all occurrences of a character in a file, and print the result:

```sh
tr {{find_characters}} {{replace_characters}} < {{filename}}
```

Replace spaces with new lines:

```sh
pacman -Su | tr ' ' '\n'
```

:arrow_right: https://tldr.ostera.io/tr

<h1 align="center">09.10.2017</h1>

## `scp` command

Secure copy - copy files between hosts using Secure Copy Protocol over SSH.

Copy a local file to a remote host:

```sh
scp load-tests/files/{ocr-back.png,ocr-front.png} aws@10.100.10.100:/home/load-tests/
```

Copy a file from a remote host to a local folder:

```sh
scp aws@10.100.10.100:/home/load-tests/artillery.json artillery-aws.json
```

:arrow_right: https://tldr.ostera.io/scp

<h1 align="center">17.10.2017</h1>

## Page smooth scroll

The [`Element.scrollIntoView()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView) method scrolls the element on which it's called into the visible area of the browser window.

```js
const element = document.getElementById("box");

element.scrollIntoView({ behavior: "smooth", block: "start" });
```

<h1 align="center">18.10.2017</h1>

## SVG clip-path

```svg
<svg>
  <defs>
    <clipPath id="logo-main-mask">
      <rect x="0" y="0" width="200" height="120" />
    </clipPath>
  </defs>

  <g clip-path="url(#logo-main-mask)">          
    <use xlink:href="#logo"/>
  </g>
</svg>
```

:arrow_right: https://eduardoboucas.com/blog/2017/09/25/svg-clip-path-logo-colour.html

## CSS Grid Template Areas

```css
.container {
  display: grid;
  width: 100%;
  height: 600px;
  grid-template-columns: 200px 1fr 1fr;
  grid-template-rows: 80px 1fr 1fr 100px;
  grid-gap: 1rem;
  grid-template-areas:
      "header header header"
      "sidebar content-1 content-1"
      "sidebar content-2 content-3"
      "footer footer footer";
}
```

```html
<div class="container">
  <div class="header">header</div>
  <div class="sidebar">sidebar</div>
  <div class="content-1">Content-1</div>
  <div class="content-2">Content-2</div>
  <div class="content-3">Content-3</div>
  <div class="footer">footer</div>
</div>
```

```css
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content-1 { grid-area: content-1; }
.content-2 { grid-area: content-2; }
.content-3 { grid-area: content-3; }
.footer { grid-area: footer; }
```

:arrow_right: https://mozilladevelopers.github.io/playground/08-template-areas

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

:arrow_right: https://tldr.ostera.io/tr

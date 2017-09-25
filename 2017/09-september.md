<h1 align="center">09.09.2017</h1>

## CSS `:focus-within`

A pseudo-class you add to an existing CSS selector, much like you would with `:focus` or `:hover`. The pseudo-class styling kicks in when either the element with :focus-within becomes focused, or a focusable child of that element is focused.

Use cases:
- Highlighting table rows
- Drop Down Navigation Menu

:arrow_right: http://www.scottohara.me/blog/2017/05/14/focus-within.html

<h1 align="center">10.09.2017</h1>

## React implicit props

```jsx
import React from 'react';

class Theme extends React.Component {
  render() {
    const children = React.Children.map(
      this.props.children, 
      (child, index) => 
        React.cloneElement(child, { 
          isActive: index === this.porps.activeIndex 
        }),
    );

    return (
      <div className="container">{ children }</div>
    );
  }
}

<Theme activeIndex={ 0 }>
  <First />
  <Second />
  <Third />
</Theme>
```

## `String.prototype.localeCompare()`

Returns a number indicating whether a reference string comes before or after or is the same as the given string in sort order.

```js
console.log('ä'.localeCompare('z', 'de')); // a negative value: in German, ä sorts before z
console.log('ä'.localeCompare('z', 'sv')); // a positive value: in Swedish, ä sorts after z
```

Sensitivity option:
- "base": Only strings that differ in base letters compare as unequal.  
Examples: a ≠ b, a = á, a = A.
- "accent": Only strings that differ in base letters or accents and other diacritic marks compare as unequal.  
Examples: a ≠ b, a ≠ á, a = A.
- "case": Only strings that differ in base letters or case compare as unequal.  
Examples: a ≠ b, a = á, a ≠ A.
- "variant": Strings that differ in base letters, accents and other diacritic marks, or case compare as unequal. Other differences may also be taken into consideration.  
Examples: a ≠ b, a ≠ á, a ≠ A.

```js
console.log('ä'.localeCompare('a', 'de', { sensitivity: 'base' })); // 0: ä has a as the base letter
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare

<h1 align="center">16.09.2017</h1>

## `GlobalEventHandlers.oncontextmenu`

An event handler property for right-click events on the window. Unless the default behavior is prevented (see examples below on how to do this), the browser context menu will activate (though IE8 has a bug with this and will not activate the context menu if a contextmenu event handler is defined).

```js
window.addEventListener('contextmenu', (e) => {
    e.preventDefault();
}, false);
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/oncontextmenu

## Make a placeholder for a `<select>` box

When the select element is required it allows use of the `:invalid` CSS pseudo-class which allows you to style the select element when in it's "placeholder" state. `:invalid` works here because of the empty value in the placeholder option.

Once a value has been set the `:invalid` pseudo-class will be dropped. You can optionally also use `:valid` if you so wish.

```html
<style>
    select:invalid { color: gray; }
</style>
<form>
    <select required>
        <option value="" disabled selected hidden>Please Choose...</option>
        <option value="0">First</option>
        <option value="1">Second</option>
    </select>
</form>
```

:arrow_right: https://stackoverflow.com/questions/5805059/how-do-i-make-a-placeholder-for-a-select-box/8442831#8442831

## Run Promises sequentially

```js
list.reduce((prev, element) =>
  prev.then(partial =>
  	makePromise(element).then(result => [...partial, result])
  ), 
  Promise.resolve([])
)
.then(results => console.log(results));
```

## `crypto.getRandomValues`

Lets you get cryptographically strong random values. The array given as the parameter is filled with random numbers (random in its cryptographic meaning).

```js
var array = new Uint32Array(10);
window.crypto.getRandomValues(array);
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/RandomSource/getRandomValues

<h1 align="center">25.09.2017</h1>

## CSS `flex-flow`

A shorthand property for flex-direction and flex-wrap individual properties.
```css
flex-flow: row nowrap;
flex-flow: column wrap;
```

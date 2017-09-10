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

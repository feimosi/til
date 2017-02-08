<h1 align="center">08.02.2017</h1>

## `console.log` styling

```js
console.log('%cerror%c message', 'color: white; background-color: red', '');

const label = ([raw]) => {
  const [color, label, ...msg] = raw.split(' ');
  return [
    `%c${label}%c ${msg.join(' ')}`,
    `color: white; background-color: ${color}; padding: 0 .5rem`,
    '',
  ]
}
console.log(...label`red error Some test error`
```

## `npm` tips

### List all available environment variables 
```js
npm run env
```

### Get bin folder location
```js
npm bin
```

## CSS 

### `all`
The `all` property resets every other property to their initial or inherited state

### `animation-fill-mode`
This property specifies what styles are applied to the element when the animation finishes playing.
By using `animation-fill-mode: forwards;` we can get the element to retain the styles in the final keyframe.

### `background-attachment`
This property specifies if the background-image stays fixed within the viewport when you scroll or scrolls along with the page.

### `flex-flow`
It's a shorthand for `flex-direction` and `flex-wrap`

### `object-fit`
Specifies how an image element is fitted into the box established by its height and width.

:arrow_right: https://css-tricks.com/lets-look-50-interesting-css-properties-values/

## HTML5 Input Datalist for Autocomplete Dropdowns

```html
<div class="ui-widget">
    <label for="tags">Tags: </label>
    <input id="tags" list="languages">
    <datalist id="languages">
        <option>ActionScript</option>
        <option>AppleScript</option>
        <option>Asp</option>
        <option>BASIC</option>
        <option>C</option>
    </datalist>
</div>
```

:arrow_right: https://developer.mozilla.org/en/docs/Web/HTML/Element/datalist

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

<h1 align="center">12.02.2017</h1>

## Difference between `exec` and `match` 

`match` (when used with the 'g' flag) returns an array with all matches. If you don't use the 'g' flag then it acts the same as the `exec` method. Although you can use a while loop with the `exec` method to find successive matches.

```js
const str = 'The quick brown fox jumped over the box like an ox with a sox in its mouth';

str.match(/\w(ox)/g); // ['fox', 'box', 'sox']

str.match(/\w(ox)/); // ['fox', 'ox']
/\w(ox)/.exec(str);  // ['fox', 'ox']
```

## `decodeURIComponent`
This function decodes a Uniform Resource Identifier (URI) component

```js
decodeURIComponent("JavaScript_%D1%88%D0%B5%D0%BB%D0%BB%D1%8B");
// "JavaScript_шеллы"
```

## `Navigator.onLine`

```js
navigator.onLine

window.addEventListener('offline', (e) => console.log('offline'));
window.addEventListener('online', (e) => console.log('online'));
```

### `String.fromCharCode` 
Return name of the key that was pressed

```js
String.fromCharCode(65); // 'A'
```

### `rel="noopener"`
Without this, the new page can access your window object via `window.opener`

```html
<a href="http://example.com" target="_blank" rel="noopener">
   Example site
</a>
```

## Download JSON File

```js 
const json = JSON.stringify(obj, null, 2);
const blob = new Blob([json], { type: 'application/json;charset=utf-8;' });
const downloadLink = event.currentTarget;
downloadLink.href = URL.createObjectURL(blob);
downloadLink.download = `${this.application.name}.json`;
```

## Fibonacci Generator

```js
function* fibonacci() {
    let a = 0;
    let b = 1;

    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

const [first, second, third, fourth, fifth, sixth, seventh] = fib;
```

## CSS pseudo classes

```css
tr:nth-child(2n+1)
tr:nth-child(odd)
```
Represents the odd rows of an HTML table.

```css
tr:nth-child(2n)
tr:nth-child(even)
```
Represents the even rows of an HTML table.

```css
span:nth-child(-n+3)
```
Matches if the element is one of the first three children of its parent and also a span.

```css
a[href^=”http”]:empty::before {
    Content: attr(href);
}
```
Display links when <a> element has no text value but the href attribute.

### Same value zero comparison algorithm

`includes()` method uses same value zero comparison algorithm, which is a modified version of the strict equality comparison. The main difference between the two is NaN with NaN equality consideration:

## ES2015 Proxies

```js
let dataStore = {  
  name: 'Billy Bob',
  age: 15
};
let handler = {  
  get(target, key, proxy) {
    const today = new Date();
    console.log(`GET request made for ${key} at ${today}`);
    return Reflect.get(target, key, proxy);
  }
}
dataStore = new Proxy(dataStore, handler);
const name = dataStore.name;

// Obscure the fact that the property exists using the has trap.
api = new Proxy(api, {  
  has(target, key) {
    return (RESTRICTED.indexOf(key) > -1) ?
      false :
      Reflect.has(target, key);
  }
});
```

## Autofill detail tokens

By default, the autocomplete attribute is set to on which means that the browser is free to store values submitted and to autofill them in the form. Recently, additional values have been added as options for the autocomplete attribute. These new options are called autofill detail tokens. These tokens tell the browser what information the field is looking for.

```html
<input type="tel" name="home-phone" autocomplete="home tel">
<input type="tel" name="work-phone" autocomplete="work tel">
<input type="email" name="home-email" autocomplete="home email">
```

:arrow_right: http://blog.cloudfour.com/autofill-what-web-devs-should-know-but-dont/

## Links vs. Buttons in Modern Web Applications

Buttons:
- Opening a modal window or popup menu
- Toggling an interface
- Playing media content
- Can be disabled with the disabled attribute
- Instruct a screen reader with the implicit button role
- Show :focus, :hover, :active, :disabled

Links:
- Navigate the user to a new page or view
- Change the URL
- Allow opening in new windows
- Can’t be disabled like buttons but can be made inert with tabindex="-1" and aria-hidden="true"
- Have the implicit link role
- Show :link, :visited, :focus, :hover, :active

:arrow_right: https://marcysutton.com/links-vs-buttons-in-modern-web-applications/

## Regex

Positive lookahead
```js
const pattern = /\d+(?= dollars)/u;
const result = pattern.exec('42 dollars');
// → result[0] === '42'
```

Negative  lookahead
```js
const pattern = /\d+(?! dollars)/u;
const result = pattern.exec('42 pesos');
// → result[0] === '42'
```

Positive lookbehind
```js
const pattern = /(?<=\$)\d+/u;
const result = pattern.exec('$42');
// → result[0] === '42'
```

Negative lookbehind
```js
const pattern = /(?<!\$)\d+/u;
const result = pattern.exec('€42');
// → result[0] === '42'
```

:arrow_right: https://mathiasbynens.be/notes/es-regexp-proposals

<h1 align="center">04.01.2017</h1>

## CSS - skip-ink underlines

```css
p {
  text-decoration: underline blue;
  text-decoration-skip-ink: auto;
}
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration-skip-ink

:arrow_right: https://caniuse.com/#feat=text-decoration


## ES2018 Regex features

- s (dotAll) flag - matches line terminator characters (e.g. '\n')

```
/^.$/s.test('\n') // == true
```

- Lookahead and lookbehind assertions

  - (?=<PATTERN>) for positive lookahead
  - (?!<PATTERN>) for negative lookahead
  - (?<=<PATTERN>) for positive lookbehind
  - (?<!<PATTERN>) for negative lookbehind
  
```js
// Positive lookahead, matches text that precedes "XYZ" 
/[\w]*(?=XYZ)/

// Negative lookahead, matches text that isn't preceded with "XYZ"
/[\w]*(?!XYZ)/

// Positive lookbehind, matches text that follows "XYZ"
/(?<=XYZ)[\w]*/

// Negative lookbehind, matches thaxt that doesn't follow "XYZ"
/(?<!XYZ)[\w]*/
```

- Named Capture Groups

```js
const pattern = /(?<day>[\d]{2})\.(?<month>[\d]{2})\.(?<year>[\d]{4})/;

const { day, month, year } = patter.exec("30.04.2017").groups;
```

:arrow_right: https://dev.to/kayis/javascripts-regular-expressions-get-more-power-4m4j

<h1 align="center">14.01.2017</h1>

## MouseEvent's coordinates

The MouseEvent object has several properties that indicate the position where the event happened. The primary distinction between them is the coordinate system they use:

- `offsetX/Y` have the coordinates relative to the target element
- `pageX/Y` are to the document
- `screenX/Y` are to the screen
- `clientX/Y` are to the viewport

### Get the coordinates relative to another element

The key is to get the element's coordinates in the same coordinate system as the event position. The simplest solution is to use `.getBoundingClientRect()` which uses viewport coordinates, the same as `clientX/Y`.

```js
const { top, left } = element.getBoundingClientRect();
const eventTop = event.clientY - top;
const eventLeft = event.clientX - left;
```

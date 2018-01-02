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

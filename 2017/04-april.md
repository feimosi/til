<h1 align="center">09.04.2017</h1>

## HTML

### `<kbd>`

Represents user input and produces an inline element displayed in the browser's default monospace font.

```html
<p>Type the following in the Run dialog: <kbd>cmd</kbd><br />

<p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>
```
:arrow_down:
> <p>Type the following in the Run dialog: <kbd>cmd</kbd><br />
> <p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>

### `<details>`

Is used as a disclosure widget from which the user can retrieve additional information.

```html
<details>
  <summary>Some details</summary>
  <p>More info about the details.</p>
</details>
```
:arrow_down:
> <details>
>  <summary>Some details</summary>
>  <p>More info about the details.</p>
> </details>

### `<wbr>`

Represents a word break opportunity—a position within text where the browser may optionally break a line, though its line-breaking rules would not otherwise create a break at that location.

```html
<p>http://this<wbr>.is<wbr>.a<wbr>.really<wbr>.long<wbr>.example<wbr>.com/With
<wbr>/deeper<wbr>/level<wbr>/pages<wbr>/deeper<wbr>/level<wbr>/pages<wbr></p>

some.very.long<wbr>@email.com
```

## Promise finally

```js
new Promise((resolve, reject) => {
    throw new Error()
})
  .then(_ => console.log('success'))
  .catch(_ => console.log('fail'))
  .then(_ => console.log('finally'))
```

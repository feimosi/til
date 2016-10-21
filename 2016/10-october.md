<h1 align="center">11.10.2016</h1>

## async vs defer

![](http://www.growingwiththeweb.com/images/2014/02/26/legend.svg)

### `<script>`

![script](http://www.growingwiththeweb.com/images/2014/02/26/script.svg)

### `<script async>`

![async](http://www.growingwiththeweb.com/images/2014/02/26/script-async.svg)

### `<script defer>`

![defer](http://www.growingwiththeweb.com/images/2014/02/26/script-defer.svg)

<br>
:arrow_right: http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html

## Auto-scroll snippet
```js
function scroll() {
  document.body.scrollTop++;
  requestAnimationFrame(scroll);
}
requestAnimationFrame(scroll);
```

<h1 align="center">15.10.2016</h1>

## Watch a file for changes
```sh
tail -F
less +F
```
With `less` you can press Ctrl+C to stop appending real time and check the file, once done you can again start appending new lines by pressing `F`.

## Copy only new or updated files
```sh
cp -u
```

## Execute the most recent command that contained the string 'foo'
```sh
!?foo
```

## Expand to the last word of the previous command
```sh
!$
```

## Repeat the last command with "sudo" prepended
```sh
sudo !!
```

## Group files by extension
```sh
ls -X 
```

## Add mulitple files by extension in Git (even in subdirectories)
```sh
git add "*.ts"
```

<h1 align="center">21.10.2016</h1>

## Git `--fixup & --autosquash`

### Automatically mark your commit as a fix of some previous commit
```sh
git commit --fixup <commit> 
```

### Automatically organize merging these fixup commits and associated normal commits
```sh
git rebase -i --autosquash
```

## Regex word boundary
```js
/\bm/.test('moon') // matches the 'm' in "moon" ;
```
This is the position where a word character is not followed or preceeded by another word-character, such as between a letter and a space.

## Rest/Spread Properties

### True clones of objects
```js
const clone1 = Object.defineProperties({},
               Object.getOwnPropertyDescriptors(obj));
```
Copy all own properties of an object and their attributes (writable, enumerable, ...), including getters and setters.
`Object.assign()` and the spread operator don’t work in this case - they only consider own enumerable properties.

:exclamation: Spread defines properties, Object.assign() sets them. Although both read values via a “get” operation.
```js
Object.defineProperty(Object.prototype, 'foo', {
    set(value) {
        console.log('SET', value);
    },
});
const obj = {foo: 123};

> Object.assign({}, obj)
SET 123
{}
> { ...obj }
{ foo: 123 }
```
<br>
:arrow_right: http://www.2ality.com/2016/10/rest-spread-properties.html

## Async / await

```js
async function logFetch(url) {
  try {
    const response = await fetch(url);
    console.log(await response.text());
  }
  catch (err) {
    console.log('fetch failed', err);
  }
}
```

`Body.json()` - Takes a Response stream and reads it to completion. It returns a promise that resolves with a JSON object.

`Body.text()` - Returns a promise that resolves with a USVString (text).

:exclamation: Calling an async function returns a promise for whatever the function returns or throws

:arrow_right: https://developers.google.com/web/fundamentals/getting-started/primers/async-functions

## Symbols

### Search for existing symbols in a runtime-wide symbol registry with the given key 

```js
Symbol.for(key)
```

### Retrieve a shared symbol key from the global symbol registry for the given symbol

```js
Symbol.keyFor(symbol);
```

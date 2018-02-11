<h1 align="center">06.02.2018</h1>

## ES2018 Named capture groups

```js
const DATE_REGEX = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/;

const match = DATE_REGEX.exec('1999-12-31');
const year = match.groups.year;
const month = match.groups.month;
const day = match.groups.day;

const { groups: { day, year } } = DATE_REGEX.exec('1999-12-31');
```

## ES2018 Regex Backreferences

`\k<name>` in a regular expression means: match the string that was previously matched by the named capture group name.

```js
const RE_TWICE = /^(?<word>[a-z]+)!\k<word>$/;
RE_TWICE.test('abc!abc'); // true
RE_TWICE.test('abc!ab'); // false
```

The backreference syntax for numbered capture groups works for named capture groups, too:

```js
const RE_TWICE = /^(?<word>[a-z]+)!\1$/;
RE_TWICE.test('abc!abc'); // true
RE_TWICE.test('abc!ab'); // false
```

:arrow_right: http://2ality.com/2017/05/regexp-named-capture-groups.html

## FileReader API to show thumbnails of selected images

```html
<input type="file" multiple id="filePicker" />
<div id="preview"></div>
```

```js
const filePicker = document.querySelector('#filePicker');
const preview = document.querySelector('#preview');

filePicker.addEventListener('change', handleFiles);

function handleFiles(e) {
  const { files } = e.target;
  
  Array.from(files)
  	.filter(file => file.type.startsWith('image/'))
    .map(file => {
    	const img = document.createElement("img");
      img.classList.add("obj");
      img.file = file;
      return img;
    })
    .forEach(img => {
      preview.appendChild(img);

      const reader = new FileReader();
      reader.onload = (e) => { img.src = e.target.result; };
      reader.readAsDataURL(img.file);
    })
}
```

`window.URL.createObjectURL(img.file)` could be used in this case as well.

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications

<h1 align="center">07.02.2018</h1>

## JavaScript Barrel

A barrel is a way to rollup exports from several modules into a single convenient module. The barrel itself is a module file that re-exports selected exports of other modules.

```js
// foo/a.ts
export function a() {}

// foo/b.ts
export function b() {}

// foo/index.ts
export {a} from './a';
export {b} from './b';
```

<h1 align="center">09.02.2018</h1>

## Use multiple SSH private keys

```sh
$ cat ~/.ssh/config
Host github
    HostName github.com
    IdentityFile ~/.ssh/id_rsa
Host bitbucket.org
    HostName bitbucket.org
    IdentityFile ~/.ssh/id_rsa_bitbucket
Host bitbucket.org-other
    HostName bitbucket.org
    IdentityFile ~/.ssh/id_rsa_other
```

### CSS `font-variant: small-caps`

Enables display of small capitals. Small-caps glyphs typically use the form of uppercase letters but are reduced to the size of lowercase letters.

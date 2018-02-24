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

<h1 align="center">09.02.2018</h1>

## Node.js: Encode / decode base64

```js
Buffer.from("Hello World").toString('base64'));

Buffer.from("SGVsbG8gV29ybGQ=", 'base64').toString('ascii');
```

<h1 align="center">17.02.2018</h1>

## React 17: `getDerivedStateFromProps` instead of `componentWillReceiveProps`

```jsx
componentWillReceiveProps(newProps) {
  if (this.props.userID !== newProps.userID) {
    this.setState({ profileOrError: undefined })
    fetchUserProfile(newProps.userID)
      .catch(error => error)
      .then(profileOrError => this.setState({ profileOrError }))
  }
}
```

:arrow_down:

```jsx
class ExampleComponent extends React.Component {
  static getDerivedStateFromProps(nextProps, prevState) {
    // Store prevUserId in state so we can compare when props change.
    // Clear out any previously-loaded user data (so we don't render stale stuff).
    if (nextProps.userId !== prevState.prevUserId) {
      return {
        prevUserId: nextProps.userId,
        profileOrError: null,
      };
    }

    // No state update necessary
    return null;
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.profileOrError === null) {
      // At this point, we're in the "commit" phase, so it's safe to load the new data.
      this._loadUserData();
    }
  }

  render() {
    if (this.state.profileOrError === null) {
      // Render loading UI
    } else {
      // Render user data
    }
  }
}
```

:arrow_right: https://github.com/reactjs/rfcs/issues/26#issuecomment-365744134

<h1 align="center">22.02.2018</h1>

## `string.replace` - specifying a function as a parameter

```js
'the quick brown fox jumps over the lazy dog'.replace(/\b\w\B/g, w => w.toUpperCase())
// => "The Quick Brown Fox Jumps Over The Lazy Dog"
```

<h1 align="center">24.02.2018</h1>

## `append()` vs `appendChild()`

- ParentNode.append() allows you to also append DOMString object, whereas Node.appendChild() only accepts Node objects.
- ParentNode.append() has no return value, whereas Node.appendChild() returns the appended Node object.
- ParentNode.append() can append several nodes and strings, whereas Node.appendChild() can only append one node.

## `KeyboardEvent.getModifierState()`

Returns the current state of the specified modifier key: true if the modifier is active (that is the modifier key is pressed or locked), otherwise, false.

```js
if (event.getModifierState("Fn") ||
    event.getModifierState("OS")) {
  /* ... */
}

if (event.getModifierState("Control") ||
    event.getModifierState("Alt")) {
  /* ... */
}
```

## `encodeURI` vs `encodeURIComponent`

- encodeURI is intended for use on the full URI.
- encodeURIComponent is intended to be used on .. well .. URI components that is any part that lies between separators (; / ? : @ & = + $ , #).

So, in encodeURIComponent these separators are encoded also because they are regarded as text and not special characters.

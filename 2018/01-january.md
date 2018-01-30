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

<h1 align="center">15.01.2017</h1>

## Resize images

With imagemagick

```sh
magick convert old.png -resize 50% new.png
```

The `mogrify` command is in many ways like `convert` except it is designed to modify images in place.

```sh
mogrify -resize 50% *.png
```

<h1 align="center">16.01.2017</h1>

## Angular: window scroll listener

```js
@HostListener('window:resize', []) handleWindowResize() {
  // ...
}
```

## Angular Dependency Injection: value providers

```js
@NgModule({
  providers: [
    { provide: 'highlight', useValue: highlight },
  ],
})
export class AppModule {}
```

```js
@Component({
  selector: 'example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.scss']
})
export class ExampleComponent {
  constructor(
    @Inject('highlight') private highlight,
  ) {}
}
```

<h1 align="center">17.01.2017</h1>

## CSS: `image-set`

Method of letting the browser pick the most appropriate CSS background image from a given set, primarily for high PPI screens.

```css
.foo {
    background-image: image-set(url(img/test.png) 1x,
                                url(img/test-2x.png) 2x,
                                url(my-img-print.png) 600dpi);
}
```

Polyfilling:
https://github.com/SuperOl3g/postcss-image-set-polyfill

:arrow_right: https://drafts.csswg.org/css-images-4/#image-set-notation

<h1 align="center">19.01.2017</h1>

## Git

Show which specific files are ignored by `.gitignore`:

```sh
git status --ignored
```

Look for specified patterns in the tracked files in the work tree, blobs registered in the index file, or blobs in given tree objects:

```sh
git grep <keyword>
```

```sh
# Looks for `time` in all tracked .c and .h files in the working directory
git grep 'time' -- '*.[ch]'

# Looks for a line that has `NODE` or `Unexpected` in files that have lines that match both.
git grep --all-match -e NODE -e Unexpected
```

Set an evaporating environment variable to use `cat` as a pager:

```sh
GIT_PAGER=cat git diff
```

<h1 align="center">21.01.2017</h1>

## JS Little-known features

### Label Statements

You can name `for` loops and blocks in JS and then refer to that name and use `break` or `continue`.

```js
loop:
for (let i = 0; i < 3; i++) {
   for (let j = 0; j < 3; j++) {
      if (i === 1) {
         continue loop1; // continues "loop"
         // break loop1; // breaks out of "loop"
      }
   }
}

foo: {
  console.log('one');
  break foo;
  console.log('this log will not be executed');
}
```

### Comma Operator

The comma operator evaluates each of its operands (from left to right) and returns the value of the last operand.

```js
function f(x) {
  return (x += 1, x); // same as return ++x;
}

y = false, true; // expression returns true
console.log(y); // false (left-most)

z = (false, true); // expression returns true
console.log(z); // true (right-most)

const isMale = type === 'man' ? (
    console.log('Hi Man!'),
    true
) : (
    console.log('Hi Lady!'),
    false
);
```

### Internationalization API

```js
const date = new Date();

const options = {
  year: 'numeric', 
  month: 'long', 
  day: 'numeric'
};

const formatter1 = new Intl.DateTimeFormat('es-es', options);
console.log(formatter1.format(date)); // 22 de diciembre de 2017

const formatter2 = new Intl.DateTimeFormat('en-us', options);
console.log(formatter2.format(date)); // December 22, 2017
```

### Atomics

Atomic operations give predictable read and write values when data is shared between the multiple threads, waiting for other operations to finish before the next one is executed. Useful for keeping data in sync between things like the main thread and another WebWorker.

### `setTimeout()` Parameters

```js
setTimeout(alert, 1000, 'Hello world!');

setTimeout(console.log, 1000, 'Hello World!', 'And Mars!');
 ```

:arrow_right: https://air.ghost.io/js-things-i-never-knew-existed/

<h1 align="center">22.01.2017</h1>

## npm: save exact version

In `.npmrc`:

```sh
save-exact = true
```

```sh
save-prefix = ''
```

## Git: rewrite all the history leading down to the root commit

```
git rebase -i --root
```

## CSS Attribute Selectors

### `[rel*="external"]`

Attribute Contains Certain Value Somewhere

```html
<h1 rel="xxxexternalxxx">Attribute Contains</h1>
```

### `[rel^="external"]`

Attribute Begins with Certain Value

```html
<h1 rel="external-link yep">Attribute Begins</h1>
```

### `[rel$="external"]`

Attribute Ends with Certain Value

```html
<h1 rel="friend external">Attribute Ends</h1>
```

### `[rel~="external"]`

Attribute is within Space Separated List

```html
<h1 rel="friend external sandwich">Attribute Space Separated</h1>
```

### `[rel|="friend"]`

Attribute is the start of a Dash Separated List

```html
<h1 rel="friend-external-sandwich">Attribute Dash Separated</h1>
```

:arrow_right: https://css-tricks.com/attribute-selectors/

<h1 align="center">24.01.2017</h1>

## Difference between `then` and `finally` in a Promise

```js
.then(...).catch(...).then(...);
// vs
.then(...).catch(...).finally(...);
```

Sometimes you don't want to catch errors at the place they arise, but in the code that uses this function, so you don't catch them. Sometimes you have to clean something up whether there was an error or not. That's where you use `finally()`.

Second difference: The function you pass to `catch()` could also throw, then you would have a rejected Promise and the following `then()` would not be called. `finally()` will be executed under any circumstance without changing the resolved value.

<h1 align="center">26.01.2017</h1>

## "Reducing" Redux code in reducers (with TypeScript)

```js
export const handleActions = <T>(
  actionsHandlers: { [s: string]: ((state: T, action: Action) => T) },
  defaultState: T,
) => (
  state = defaultState,
  { type, payload },
) => {
  const handler = actionsHandlers[type];
  return handler ?
    handler(state, payload) :
    defaultState;
};
```

```js
export const lessonsReducer = handleActions<LessonsState>({
  [TYPES.FETCH_LESSON]: (state) => requestActionHandler(state),
  [TYPES.FETCH_LESSON_FAIL]: (state, action) => errorActionHandler(state, action.payload),
  [TYPES.FETCH_LESSON_SUCCESS]: (state, action) => ({
    ...state,
    lesson: action.payload.lesson,
    isLoading: false,
  })
}, initialState);
```


<h1 align="center">27.01.2017</h1>

## Angular Dependency Injection: factory providers

```js
export const IS_PLATFORM_BROWSER = new InjectionToken<() => boolean>('isPlatformBrowser');

@NgModule({
  providers: [
    {
      provide: IS_PLATFORM_BROWSER,
      useFactory: (platformId) => () => isPlatformBrowser(platformId),
      deps: [PLATFORM_ID],
    },
  ],
})
export class AppModule {}
```

```js
@Component({
  selector: 'example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.scss']
})
export class ExampleComponent {
  constructor(
    @Inject(IS_PLATFORM_BROWSER) private isPlatformBrowser,
  ) {}
}
```

<h1 align="center">28.01.2017</h1>

## ES2018: RegExp Unicode property escapes

The proposal lets you additionally match characters by mentioning their Unicode character properties inside the curly braces of \p{}. In the Unicode standard, each character has properties – metadata describing it.

```js
/^\p{White_Space}+$/u.test('\t \n\r')
// true

/^\p{Script=Greek}+$/u.test('μετά')
// true

/^\p{Script=Latin}+$/u.test('mañana')
// true
```

:arrow_right: http://2ality.com/2017/07/regexp-unicode-property-escapes.html

## `@@unscopable` symbol

Contains property names that were not included in the ECMAScript standard prior to the ES2015 version. These properties are excluded from with statement bindings.

```js
var keys = [];

with (Array.prototype) {
  keys.push('something');
}
```

This prevents that the `Array.prototype.keys()` method is being scoped into the `with` statement.

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@unscopables

<h1 align="center">30.01.2017</h1>

## Angular: `<ng-container>`

`<ng-container>` is a grouping element that doesn't interfere with styles or layout because Angular doesn't put it in the DOM.

Conditionally exclude a select `<option>`:

```html
<select [(ngModel)]="hero">
  <ng-container *ngFor="let h of heroes">
    <ng-container *ngIf="showSad || h.emotion !== 'sad'">
      <option [ngValue]="h">{{h.name}} ({{h.emotion}})</option>
    </ng-container>
  </ng-container>
</select>
```

## Angular: `:host-context`

The `:host-context()` selector looks for a CSS class in any ancestor of the component host element, up to the document root.

```html
<body class="theme-dark">
```

```css
:host-context(.theme-dark) .content {
  background-color: #eee;
}
```

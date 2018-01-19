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

```ts
@HostListener('window:resize', []) handleWindowResize() {
  // ...
}
```

## Angular Dependency Injection: value providers

```ts
@NgModule({
  providers: [
    { provide: 'highlight', useValue: highlight },
  ],
})
export class AppModule {}
```

```ts
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

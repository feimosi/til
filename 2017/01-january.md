<h1 align="center">04.01.2017</h1>

## Sass/BEM - reference the component's base class
```scss
.ts-Button {
  $module: &;

  &--is-disabled {
    #{$module}__label {
      opacity: .4;
    }
  }
}
```
becomes
```scss
.ts-Button--is-disabled .ts-Button__label {
  opacity: .4;
}
```

<h1 align="center">05.01.2017</h1>

## Reset CSS value with `unset`
```css
.foo p {
  color: unset;
}
```
This keyword resets the property to its inherited value if it inherits from its parent (e.g. color) or to its initial value if not (e.g. border-color).

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/CSS/unset

## ES2015 Proxies
```js
const values = [
  'One',
  'Two',
  'Three',
  'Four',
];

const proxy = new Proxy(values, {
  get(target, name) {
    if (name in target) {
      return Reflect.get(target, name);
    }
    const index = Number(name);
    return Reflect.get(target, target.length + index)
  }
})

console.log(proxy[-1]);
```

## React - render callback pattern
A component receives a function as its child. The parent component manages the state and the consumer of the parent can decide in which way they’d like to apply the arguments they receive to their UI.
```jsx
<Twitter username='feimosi'>
  {(user) => user === null
    ? <Loading />
    : <Badge info={user} />}
</Twitter>

<Twitter username='feimosi'>
  {(user) => user === null
    ? <Loading />
    : <Profile info={user} />}
</Twitter>

class Twitter extends Component {
  // ...
  render () {
    return this.props.children(this.state.user)
  }
}
```

<h1 align="center">06.01.2017</h1>

## React - componentWillMount anti-pattern
- currently `componentWillUnmount` can be called without `componentDidMount` ever being called (in case of errors)
- `componentWillMount` is not a good place for side-effects
- `setState` results in rerender
- cDM of descendants is called before cDM of ancestors

```js
class Foo {
  state = { data: null };
  // ANTI-PATTERN
  componentWillMount() {
    this._subscription = GlobalStore.getFromCacheOrFetch(data => this.setState({ data: data });
  }
  componentWillUnmount() {
    if (this._subscription) {
      GlobalStore.cancel(this._subscription);
    }
  }
  ...
}
```
:arrow_down:
```js
class Foo {
  state = { data: GlobalStore.getFromCacheOrNull() };
  componentDidMount() {
    if (!this.state.data) {
      this._subscription = GlobalStore.fetch(data => this.setState({ data: data });
    }
  }
  componentWillUnmount() {
    if (this._subscription) {
      GlobalStore.cancel(this._subscription);
    }
  }
  ...
}
```

`getInitialState` (for `createClass`), `constructor` and `componentWillMount` always happen at the same time and have the same capabilities. The only difference is that `componentWillMount` can use `this.setState` where as the others have to return or initialize the state field. Technically you can do everything you can with `getInitialState`/`constructor` still. However, there is a good practice that you don't do side-effects in those.

The reason to deprecate componentWillMount is because there is a lot of confusion about how it works, and what you can safely do in it, and usually the answer is nothing. It is a life-cycle with void return so the only thing you can do is side-effects, and it is very common to use them for things like subscriptions and data fetching.

:arrow_right: https://github.com/facebook/react/issues/7671

## Angular 1 - FormController custom validators
On the example of confirm password validation
```js
export default () => ({
    require: 'ngModel',
    scope: {
        compareTo: '=',
    },
    link(scope, element, attributes, ctrl) {
        ctrl.$validators.compareTo =
            (modelValue) => ctrl.$isEmpty(modelValue) || modelValue === scope.compareTo;

        scope.$watch('compareTo', () => {
            ctrl.$validate();
        });
    },
});
```
```pug
input(type="password" name="repeatedPassword"
      ng-model="$ctrl.repeatedPassword"
      compare-to="$ctrl.password" required)               
```

:arrow_right: http://odetocode.com/blogs/scott/archive/2014/10/13/confirm-password-validation-in-angularjs.aspx

:arrow_right: https://docs.angularjs.org/guide/forms - Custom Validation

<h1 align="center">08.01.2017</h1>

## Reactive Programming
Reactive programming is programming with asynchronous data streams. A stream is a sequence of ongoing events ordered in time. It can emit three different things: a value (of some type), an error, or a "completed" signal.

Stream has many functions attached to it, such as `map`, `filter`, `scan`, etc. Each returns a new stream, it does not modify the original stream in any way (immutability).

- `throttle`  
```js
Rx.Observable.prototype.throttle(windowDuration, [scheduler])
```  
  Returns a stream that emits only the first item emitted by the source during sequential time windows.

- `debounce`  
```js
Rx.Observable.prototype.debounce(dueTime, [scheduler])
```
Emits an item from the source after a particular timespan has passed without the stream emitting any other items.

- `just`  
```js
Rx.Observable.return(value, [scheduler])
Rx.Observable.just(value, [scheduler])
```
Create an Observable that emits a particular single item.

- `flatMap`  
```js
Rx.Observable.prototype.flatMap(selector, [resultSelector])

const responseStream = requestStream
  .flatMap(requestUrl =>
    Rx.Observable.fromPromise(jQuery.getJSON(requestUrl))
  );
```
It basically merges an observable sequence of observable sequences into a single observable sequence.

- `combineLatest`  
```js
Rx.Observable.combineLatest(...args, [resultSelector])

const source1 = Rx.Observable.interval(100).map(i => `First: ${i}`);
const source2 = Rx.Observable.interval(150).map(i => `Second: ${i}`);

const source = Rx.Observable.combineLatest(
        source1,
        source2,
        (s1, s2) => `${s1}, ${s2}`
    ).take(4);

// => onNext: First: 0, Second: 0
// => onNext: First: 1, Second: 0
// => onNext: First: 1, Second: 1
// => onNext: First: 2, Second: 1
```
Merges the specified observable sequences into one observable sequence by using the selector function whenever any of the observable sequences produces an element. 

:arrow_right: https://gist.github.com/staltz/868e7e9bc2a7b8c1f754

## CSS - `word-break`
By default the browser breaks by white spaces and hyphens.
```css
word-break: break-word; /* same as `word-wrap: break-word` (also overflow-wrap: break-word;) */
```
```html
[X] I am a text that 
012345678901234567890123
4567890123456789 want to
live inside this narrow 
paragraph.
```

```css
word-break: break-all;
```
```html
[X] I am a text that 0123
4567890123456789012345678
90123456789 want to live 
inside this narrow paragr
aph.
```

:arrow_right: https://css-tricks.com/snippets/css/prevent-long-urls-from-breaking-out-of-container/

## CSS - `white-space`
Describes how whitespace inside the element is handled. Allows to preserve new lines,	spaces and tabs.

<table class="standard-table">
 <thead>
  <tr>
   <th>&nbsp;</th>
   <th>New lines</th>
   <th>Spaces and tabs</th>
   <th>Text wrapping</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <th><code>normal</code></th>
   <td>Collapse</td>
   <td>Collapse</td>
   <td>Wrap</td>
  </tr>
  <tr>
   <th><code>nowrap</code></th>
   <td>Collapse</td>
   <td>Collapse</td>
   <td>No wrap</td>
  </tr>
  <tr>
   <th><code>pre</code></th>
   <td>Preserve</td>
   <td>Preserve</td>
   <td>No wrap</td>
  </tr>
  <tr>
   <th><code>pre-wrap</code></th>
   <td>Preserve</td>
   <td>Preserve</td>
   <td>Wrap</td>
  </tr>
  <tr>
   <th><code>pre-line</code></th>
   <td>Preserve</td>
   <td>Collapse</td>
   <td>Wrap</td>
  </tr>
 </tbody>
</table>

:arrow_right: https://developer.mozilla.org/en/docs/Web/CSS/white-space

<h1 align="center">13.01.2017</h1>

## CSS Animations
Modern browsers can animate four things really cheaply: position, scale, rotation and opacity. Thus always use `transform` / `opacity` when possible.

![](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/devtools-waterfall.jpg)

The higher up you start on the timeline waterfall the more work the browser has to do to get pixels on to the screen, so stay on the `Composite Layers` most of the time.

Styles that affect layout:
- width
- height
- padding
- margin
- display
- border-width
- border
- top
- position
- font-size
- float
- text-align
- overflow-y
- font-weight
- overflow
- left
- font-family
- line-height
- vertical-align
- right
- clear
- white-space

:arrow_right: https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/

## High-performance UI with React 

We could improve the performance of our app substantially by limiting the number of props passed through the stack. We found that groups of props were often related and always changed at the same time. In these cases, it made sense to group those related props under a single “namespace” prop. If a namespace prop can be modeled as an immutable value, subsequent calls to shouldComponentUpdate can be optimized further by checking referential equality rather than doing a deep comparison.

A common theme: key iteration was expensive. 

Memoization of expensive computations

Declarative APIs
```jsx
// before
componentDidMount() {
    this.refs.someWidget.focus()
}

// after
render() {
    return <Widget focused={true} />;
}
```

:arrow_right: http://techblog.netflix.com/2017/01/crafting-high-performance-tv-user.html

<h1 align="center">14.01.2017</h1>

## `require.ensure`
The method ensures that every dependency in dependencies can be synchronously required when calling the callback. callback has no parameter.

```js
require.ensure(["module-a", "module-b"], function() {
  var a = require("module-a");
  // ...
});
```
Note: require.ensure only loads the modules, it doesn’t evaluate them.

## Webpack 2
```js
const config = {
  module: {
    rules: [{
      test: /.css$/,
      use: ['style-loader', {
        loader: 'css-loader',
        options: {
          modules: true
        }
      }],
      include: PATHS.style
    }
  ]},
  resolve: {
    modules: [
      path.join(__dirname, 'src'),
      'node_modules'
    ],
    alias: {
      Utilities: path.resolve(__dirname, 'src/utilities/'),
    },
    extensions: ['.js', '.json', '.jsx']
  }
};
```

Given it's not a good idea to bundle everything as JavaScript (FOUC, performance), there are plugins like **ExtractTextPlugin**

Loaders:
- css-loader - Resolves @import and url(...)
- style-loader - Attaches rules to document, implements HMR
- extract-text-webpack-plugin - Extracts text from bundle to a file → Separate CSS (avoids FOUC)
- url-loader - Inlines loads assets to JavaScript
- file-loader - Returns file paths and emits files
- image-webpack-loader - Minifies images using imagemin
- webpack-spritesmith - Converts images into a spritesheet using spritesmith
- svg-inline-loader - Inlines SVG as a module

Improving Build Speed
- include aggressively at loaders to avoid work
- Use combination of module.noParse and resolve.alias against minified files to avoid work during development

Devtool: 
'eval' is a good default for development. source-map for production.

:arrow_right: https://presentations.survivejs.com/advanced-webpack/

<h1 align="center">15.01.2017</h1>

## HTML tips

To handle semantics properly, pick your heading rank in sequential order, 

```html
<article>
    <h1>Monkey Island</h1>
    <h2>Look behind you! A three-headed monkey!</h2>
    <!-- ... -->
</article>
```

To create subheadings or tag lines to accompany headings use regular text markup rather than a lower-rank heading.

```html
<header>
    <h1>Star Wars VII</h1>
    <p>The Force Awakens</p>
</header>
```

Placeholders are meant to show examples of formatting valid for a field.

```html
<input type="email" placeholder="darth.vader@empire.gov" name="mail">
```

Pick the correct type for <input> elements.
- type="number" 
- type="email"
- type="tel"

SVG sprites to implement vector icons is much better than using Web Fonts – which is a hack: 
- browsers treat Web Font icons as text
- ad blockers can disabe the download of fonts

```html
<svg>
    <symbol id="social-twitter" viewBox="...">
        <!-- actual image data here -->
    </symbol>
</svg>
...
<svg class="social-icon">
    <use xlink:href="icons.svg#social-twitter" />
</svg>
```

:arrow_right: https://hacks.mozilla.org/2016/08/a-few-html-tips/

## CSS

### CSS Specificity:
- Type selectors (e.g., h1) and pseudo-elements (e.g., :before).
- Class selectors (e.g., .example), attributes selectors (e.g., [type="radio"]) and pseudo-classes (e.g., :hover).
- ID selectors (e.g., #example).

### Feature Queries
```css
@supports ( display: flex ) {
  .foo { display: flex; }
}
```

### Native Variables (Custom Properties)

```css
:root {
  --theme-colour: cornflowerblue;
}

h1 { color: var(--theme-colour); }  
a { color: var(--theme-colour); }  
```

### Pure CSS Tooltips

```css
.tooltip::after {
  content: attr(data-tooltip);
}
```

### CSS Counters

```css
.container {
	counter-reset: issues 0 examples 0;
}

.issue:before {
	counter-increment: issues 1;
	content: "Issue " counter(issues, decimal);
}
```

:arrow_right: https://bitsofco.de/3-new-css-features-to-learn-in-2017/

:arrow_right: https://www.sitepoint.com/8-clever-tricks-with-css-functions/

<h1 align="center">16.01.2017</h1>

## DevTools tips

- `$$(selector)` — select all the elements
- `document.body.contentEditable = true` — edit the HTML anywhere in the DOM
- `getEventListeners($(selector))` — find events associated with an Element in the DOM
- `monitorEvents($(selector), eventName)` — monitor the event associated with the element
- `$_` — retrieve the value of your last result

<h1 align="center">17.01.2017</h1>

## Optimizing React 

Performance optimization process should look like this:
- Realise there is a performance issue
- Measure using DevTools and analyze to find the bottleneck
- Work around the issue
- Test again to confirm an improvement
- Goto 2 if needed

### Profiling Components with Chrome Timeline  
http://localhost:3000/?react_perf

### `shouldComponentUpdate`, `PureComponent`
Should only be used for:
- components that use simple props (no deep objects or arrays in props)
- "leaf"-components or components located deep in the rendering tree. By bailing out of rendering high in the component tree, you might miss a required re-render further down the tree since it will skip rendering for all child components.

### Move expensive code to a higher level component
Off-loading these calculations to a higher-level component or memoizing the result. Using libraries like reselect can be a huge help.

### Don’t abuse this.setState
If you don’t use something in render(), it shouldn't be in the state.

:arrow_right: https://medium.com/@okonetchnikov/react-at-60fps-4e36b8189a4c

## React Context

```jsx
export default class ScrollPane extends Component {
  static contextTypes = {
    registerPane: PropTypes.func.isRequired,
    unregisterPane: PropTypes.func.isRequired
  };

  componentDidMount() {
    this.context.registerPane(this.el)
  }

  componentWillUnmount() {
    this.context.unregisterPane(this.el)
  }

  render() {...}
}

export default class ScrollContainer extends Component {

  static childContextTypes = {
    registerPane: PropTypes.func,
    unregisterPane: PropTypes.func
  }

  getChildContext() {
    return {
      registerPane: this.registerPane,
      unregisterPane: this.unregisterPane
    }
  }

  panes = []

  registerPane = (node) => {
    if (!this.findPane(node)) {
      this.addEvents(node)
      this.panes.push(node)
    }
  }
  ...
}
```

<h1 align="center">20.01.2017</h1>

## JavaScript's `void` operator

This operator allows inserting expressions that produce side effects into places where an expression that evaluates to undefined is desired. It's often used merely to obtain the undefined primitive value (using `void 0`).

When a browser follows a javascript: URI, it evaluates the code in the URI and then replaces the contents of the page with the returned value, unless the returned value is undefined. 

```html
<a href="javascript:void(document.body.style.backgroundColor='green');">
  Click here for green background
</a>
```

<h1 align="center">21.01.2017</h1>

## Optimizing react-redux

Rendering a list of some elements in a page is a common task for almost any web-app and here's a way to improve performance for that case.

```js
const targets = this.state.targets.map((target) => {
  return <Target key={target.id} target={target}/>;
});
```

### Don’t update unchanged elements

```js
class Target extends React.Component {
    shouldComponentUpdate(newProps) {
        return this.props.target !== newProps.target;
    }
    ...
}
```

### Make child components smarter

Don’t update the parent component if a list has the same number of elements and order of elements is the same.

```js
shouldComponentUpdate(newProps, newState) {
    // check if order or length of list is changed
    // if all ids are the same it means no "significant" changes
    const changed = newState.targets.find((target, i) => {
        return this.state.targets[i].id !== target.id;
    });
    return changed;
}
```

Children components should update itself if data is changed.

```js
store.subscribe(() => {
    const newTarget = store.getState().targets[this.props.index];
    if (newTarget !== this.state.target) {
	this.setState({
	    target: newTarget
	});
    }
});
```

### Change state shape

```js
const state = {
  targets: [{id: 'target1', radius: 10}, {id: 'target2', radius: 2}]
};
```
:arrow_down:
```js
const state = {
  targetsOrder: ['id-1', 'id-2'],
  targets: {
    'id-1': { id: 'id-1', radius: 10 },
    'id-2': { id: 'id-2', radius: 20 },
  }
};
```

In this case, we're passing item's ID into child view, not the whole item. Thus "TargetView" can not be a "dumb" component. It should be connected to the store:

```js
const TargetView = connect(
  (state, ownProps) => ({target: state.targets[ownProps.targetId]})
)(({ target }) => {
  return <Circle radius={target.radius} />;
});
```

:arrow_right: https://medium.com/@lavrton/optimizing-react-redux-store-for-high-performance-updates-3ae6f7f1e4c1

<h1 align="center">23.01.2017</h1>

## Applying Functional Thinking 

Given:
```js
function averageSalary(employees, minSalary, department){
  var total = 0
  var count = 0
  _.each(employees, function(e){
    if(minSalary < e.salary && (department == undefined || department.works(e))){
      total += e.salary
      count += 1
    }
  })
  return (count == 0) ? 0 : total / count
}
```
If we just added another condition, the signature of the function (thus, the public interface) would have to change, and the if statement would grow.

### Use Functions Instead of Simple Values

```js
function and(predicates) {
  return e => _.every(predicates, p => p(e))
}
function averageSalary(employees, conditions) {
  let total = 0
  let count = 0
  _.each(employees, function(e) {
    if(and(conditions)(e)){
      total += e.salary
      count += 1
    }
  })
  return (count == 0) ? 0 : total / count
}
```

### Model Data Transformations as a Pipeline

```js
function averageSalary(employees, conditions){
  const filtered = _.filter(employees, and(conditions))
  const salaries = _.map(filtered, 'salary')
  const total = _.reduce(salaries, (a, b) => a + b, 0)
  return (salaries.length == 0) ? 0 : total / salaries.length
}
```

### Extract Generic Functions

```js
function average(nums){
  var total = _.reduce(nums, function(a,b){return a + b}, 0)
  return (nums.length == 0) ? 0 : total / nums.length
}

function employeeSalaries(employees, conditions){
  var filtered = _.filter(employees, and(conditions))
  return _.pluck(filtered, 'salary')
}

function averageSalary(employees, conditions){
  return average(employeeSalaries(employees, conditions))
}
```

:arrow_right: https://nulogy.com/who-we-are/company-blog/articles/functional-js/

## Angular component's `require` property

```js
angular
  .module('app', [])
  .component('parentComponent', {
    transclude: true,
    template: `<div ng-transclude></div>`,
    controller() {
      this.foo = () => 'Foo from parent!';
    }
  })
  .component('childComponent', {
    require: {
      parent: '^parentComponent'
    },
    controller() {
      this.$onInit = () => {
        this.state = this.parent.foo();
      };
    },
    template: `<div>Component! {{ $ctrl.state }}</div>`
  });

document.addEventListener('DOMContentLoaded', function () {
  angular.bootstrap(document, ['app']);
});
```

:arrow_right: https://toddmotto.com/on-init-require-object-syntax-angular-component/

## Inlining Web Workers

```js
function createWorker(workerFunc) {
  if (! (workerFunc instanceof Function)) {
    throw new Error('Argument must be function');
  }
  const src = `(${workerFunc})();`;
  const blob = new Blob([src], {type: 'application/javascript'});
  const url = URL.createObjectURL(blob);
  return new Worker(url);    
}
```

:arrow_right: http://www.2ality.com/2017/01/messagechannel.html

<h1 align="center">24.01.2017</h1>

## Symbol.iterator

```js
var myIterable = {}
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
[...myIterable] // [1, 2, 3]
```

```js
var someString = new String("hi"); // need to construct a String object explicitly to avoid auto-boxing

someString[Symbol.iterator] = () => {
  return {
    next() {
      if (this._first) {
        this._first = false;
        return { value: "bye", done: false };
      } else {
        return { done: true };
      }
    },
    _first: true
  };
};
```

Some statements and expressions are expecting iterables, for example the for-of loops, spread operator, yield*, and destructuring assignment.

```js
for(let value of ["a", "b", "c"]){
    console.log(value);
}

[..."abc"];

function* gen(){
  yield* ["a", "b", "c"];
}
gen().next();

[a, b, c] = new Set(["a", "b", "c"]);
```

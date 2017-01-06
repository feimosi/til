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

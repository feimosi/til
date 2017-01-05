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

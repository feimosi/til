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

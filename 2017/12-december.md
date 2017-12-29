<h1 align="center">23.12.2017</h1>

## To make code clearer, remove possibilities

```js
const doSomething = (list) => {
  return list.map((item) => {
    return doSomethingElse(item)
  })
}
```

:arrow_down:

```js
const doSomething = (list) => 
  list.map(doSomethingElse)
```

A function with braces can have multiple lines of code. 
Even though there is only 1 line, when we see the braces it signals that there is a possibility of more.
If we remove that possibility, we can see at a glance that this function returns something, with no other effects.

Another example:

```js
(state) => {
  return {
    foo: state.foo,
    bar: state.bar
  }
}
```

:arrow_down:

```js
({ foo, bar }) => ({ foo, bar })
```

Reading this code, we have to look closely. Did we return the exact same keys that we selected? 
If not, was it an intentional renaming, or a typo?
In the second code snippet, we have removed the possibility of renaming keys and made less room for human error at the same time.

:arrow_right: https://github.com/joshwnj/knowledge/blob/e0975ac0707e84fb64f621ce56602cd43fed340a/_notes/2017-11/01-000.md

<h1 align="center">29.12.2017</h1>

## Sass recap

### Mixins

```scss
@mixin font-size($n) {
  font-size: $n * 1.2em;
}
body {
  @include font-size(2);
}
```

### Extend

```scss
.button {
  ···
}
.push-button {
  @extend .button;
}
```

### Loops

```scss
@for $i from 1 through 4 {
  .item-#{$i} { left: 20px * $i; }
}
```

```scss
$backgrounds: (home, 'home.jpg'), (about, 'about.jpg');

@each $id, $image in $backgrounds {
  .photo-#{$id} {
    background: url($image);
  }
}
```

```scss
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

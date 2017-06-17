<h1 align="center">11.06.2017</h1>

## `void` operator

void expression evaluates the expression and returns undefined no matter the result of evaluation.

```js
void 1;                    // => undefined  
void (false);              // => undefined  
void {name: 'John Smith'}; // => undefined  
void Math.min(1, 3);       // => undefined  
```

One use case of void operator is to suppress expression evaluation to undefined, relying on some side-effect of the evaluation.

## `undefined`

The existence of undefined is a consequence of JavaScript's permissive nature that allows the usage of:

- uninitialized variables
- non-existing object properties or methods
- out of bounds indexes to access array elements
- the invocation result of a function that returns nothing

> The main difference is that undefined represents a value of a variable that wasn't yet initialized, while null represents an intentional absence of an object

Mostly comparing directly against undefined is a bad practice, because you probably rely on a permitted but discouraged practice mentioned above.

An efficient strategy is to reduce at minimum the appearance of undefined keyword in your code. In the meantime, always remember about its potential appearance in a surprising way, and prevent that by applying beneficial habits such as:

- reduce the usage of uninitialized variables
- make the variables lifecycle short and close to the source of their usage
- whenever possible assign an initial value to variables
- favor const, otherwise use let
- use default values for insignificant function parameters
- verify the properties existence or fill the unsafe objects with default properties
- avoid the usage of sparse arrays

:arrow_right: https://rainsoft.io/7-tips-to-handle-undefined-in-javascript/

<h1 align="center">17.06.2017</h1>

## CSS `quotes`

The `quotes` property in CSS allows you to specify which types of quotes are used when quotes are added via the `content: open-quote` or `content: close-quote` rules. 

```css
q {
  quotes: "\201C""\201D""\2018""\2019";
}

q:before {
    content: open-quote;
}

q:after {
    content: close-quote;
}
```

:arrow_right: https://css-tricks.com/snippets/css/simple-and-nice-blockquote-styling/

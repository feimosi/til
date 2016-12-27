<h1 align="center">27.12.2016</h1>

## `String.prototype.split`
```js
str.split([separator[, limit]])
```
If separator is omitted, the array returned contains one element consisting of the entire string.

Limit specifies the maximum number of splits to be found.

## Mixins via subclass factories
```js
const Storage = Sup => class extends Sup {
    save(database) { ··· }
};
const Validation = Sup => class extends Sup {
    validate(schema) { ··· }
};

class Person { ··· }
class Employee extends Storage(Validation(Person)) { ··· }
```

## Global error handling
```js
window.addEventListener('error', (e) => {
    const stack = e.error.stack;
    const message = e.error.toString();
    // Handle the error...
});
```
:arrow_right: https://www.sitepoint.com/proper-error-handling-javascript/

## Closures 
In a nutshell, a closure is the combination of a function bundled together (enclosed) with 
references to it’s surrounding state (the lexical environment). In JavaScript, closures are created 
every time a function is created, at function creation time.
In practical terms, since a closure bundles the lexical environment, a closure gives you access to 
an outer function’s scope from an inner function.

Closures are commonly used:
* to give objects data privacy  
    Data privacy is an essential property that helps us program to an interface, not an implementation.
  
    > “Program to an interface, not an implementation.”

* stateful functions  
```js
const secret = (msg) => () => msg;
```

* partial application  
```js
const partialApply = (fn, ...fixedArgs) => {
  return function (...remainingArgs) {
    return fn.apply(this, fixedArgs.concat(remainingArgs));
  };
};
```

* callbacks

:arrow_right: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36

## Multi-line Padded Text 
![](http://callmenick.com/files/2016-11/box-decoration-break-output.png)
```css
box-decoration-break: clone;
```
:arrow_right: http://callmenick.com/post/multi-line-padded-text-css-box-decoration-break

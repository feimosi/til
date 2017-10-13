<h1 align="center">09.05.2017</h1>

## Web API

### URL

Is used to parse, construct, normalise, and encode URLs.
The constructor takes a url parameter, and an optional base parameter to use if the url parameter is a relative URL.

```js
const url = new URL('../cats', 'http://www.example.com/dogs');
console.log(url.hostname); // "www.example.com"
console.log(url.pathname); // "/cats"
```

URL properties can be set to construct the URL:

```js
url.hash = 'tabby';
console.log(url.href); // "http://www.example.com/cats#tabby"
url.pathname = 'démonstration.html';
console.log(url.href); // "http://www.example.com/d%C3%A9monstration.html"
```

### Get the search params from the current window's URL

```js
const parsedUrl = new URL(window.location.href);
console.log(parsedUrl.searchParams.get('id')); // 123
```

:arrow_right: https://developer.mozilla.org/en/docs/Web/API/URL

<h1 align="center">20.05.2017</h1>

## React

### HOC to conditionally render components

![](https://pbs.twimg.com/media/DAJOO8UUIAAvsf3.jpg:large)

## CSS

### Switching from display none to block with a CSS transition

![](https://pbs.twimg.com/media/DANxOXjV0AAcxXT.png)

<h1 align="center">21.05.2017</h1>

## Web API

### [Window.matchMedia()](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia)

Returns a new MediaQueryList object representing the parsed results of the specified media query string.

```js
if (window.matchMedia("(min-width: 400px)").matches) {
  /* the viewport is at least 400 pixels wide */
} else {
  /* the viewport is less than 400 pixels wide */
}
```
```js
componentDidMount() {
  const mql = window.matchMedia('(min-width: 768px)');
  this.mqListener(mql);
  mql.addListener(this.mqListener);
}
  
mqListener = (mq) => {
  if (mq.matches) {
    obj.setState({ smallScreen: false });
    obj.setState({ sidebarOpened: true });
  } else {
    obj.setState({ smallScreen: true });
    obj.setState({ sidebarOpened: false });
  }
};
```

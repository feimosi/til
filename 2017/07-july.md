<h1 align="center">18.07.2017</h1>

## Async Component

```jsx
export default class AsyncComponent extends React.Component {
  static propTypes = {
    // specifically loader is a function that returns a promise. The promise
    // should resolve to a renderable React component.
    loader: PropTypes.func.isRequired,
    placeholderHeight: PropTypes.number,
    renderPlaceholder: PropTypes.func,
  };

  this.state = {
    Component: null,
  };

  componentDidMount() {
    this.props.loader().then(Component =>
      this.setState({ Component });
    );
  }

  render() {
    const { Component } = this.state;
    const { renderPlaceholder, placeholderHeight, loader, ...rest } = this.props;
    if (Component) {
      return <Component {...rest} />;
    }

    return renderPlaceholder ?
      renderPlaceholder() :
      <WrappedPlaceholder height={placeholderHeight} />;
  }
}

```
```jsx
function mapLoader() {
  return new Promise(resolve => 
    airPORT('./Map', 'HomesSearchMap')
      .then(x => x.default || x);
  );
}

export default function MapAsync(props) {
  return <AsyncComponent loader={mapLoader} {...props} />;
}
```

<h1 align="center">31.07.2017</h1>

## Web APIs

### `Element.closest()`

Returns the closest ancestor of the current element (or the current element itself) which matches the selectors given in parameter. If there isn't such an ancestor, it returns null.

```html
<article>
  <div id="div-01">Here is div-01
    <div id="div-02">Here is div-02
      <div id="div-03">Here is div-03</div>
    </div>
  </div>
</article>
```

```js
var el = document.getElementById('div-03');

var r4 = el.closest(":not(div)"); // returns the outmost article
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/Element/closest

### Page Visibility API

Lets you know when a webpage is visible or in focus. With tabbed browsing, there is a reasonable chance that any given webpage is in the background and thus not visible to the user. When the user minimizes the webpage or moves to another tab, the API sends a visibilitychange event regarding the visibility of the page.

```js
function handleVisibilityChange() {
  if (document.hidden) {
    videoElement.pause();
  } else {
    videoElement.play();
  }
}

document.addEventListener('visibilitychange', handleVisibilityChange, false);
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API

### Storage API quotas

```js
navigator.storage.estimate().then(estimate => {
  // estimate.quota is the estimated quota
  // estimate.usage is the estimated number of bytes used
});
```

:arrow_right https://developer.mozilla.org/en-US/docs/Web/API/Storage_API

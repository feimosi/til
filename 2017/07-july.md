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

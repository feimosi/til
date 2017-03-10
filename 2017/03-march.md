<h1 align="center">03.03.2017</h1>

## CSS

### Apply `box-sizing` globally

```css
html {
  box-sizing: border-box;
}

*, *::before, *::after {
  box-sizing: inherit;
}
```

### Comma-separated lists

```css
ul > li:not(:last-child)::after {
  content: ",";
}
```

### Select first n items

```css
/* select items 1 through 3 and display them */
li:nth-child(-n+3) {
  display: block;
}
```

### Accessibility fallback for icon-only buttons

```css
.no-svg .icon-only:after {
  content: attr(aria-label);
}
```

### Keep cells in a table at equal width

```css
.calendar {
  table-layout: fixed;
}
```

### Style "default" links

```css
a[href]:not([class]) {
  color: #008000;
  text-decoration: underline;
}
```

:arrow_right: https://github.com/AllThingsSmitty/css-protips

<h1 align="center">10.03.2017</h1>

## React patterns

### Handling multiple inputs in controlled components 

```jsx
class Reservation extends React.Component {
  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            value={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

### Autobind labels to inputs with unique IDs

```jsx
let count = 1;
export const getNextId = () => `element-id-${count++}`;
// Reset during app bootstrap phase (for SSR)
export const resetId = () => count = 1;

class Input extends React.Component {
  constructor(props) {
    super(props);
    this.id = getNextId();
  }
  
  onChange = (e) => {
    this.props.onChange(e.target.value);
  }

  render() {
    return (
      <label htmlFor={this.id}>
        {this.props.label}
      </label>
      <input
        id={this.id}
        value={this.props.value} 
        onChange={this.onChange}
      />
    );
  }
}
```

### Switching component pattern

```jsx
const PAGES = {
  home: HomePage,
  about: AboutPage,
  user: UserPage,
};

const Page = (props) => {
  const Handler = PAGES[props.page] || FourOhFourPage;
  return <Handler {...props} />
};

Page.propTypes = {
    page: PropTypes.oneOf(Object.keys(PAGES)).isRequired,
};
```

### Autofocus input

```jsx
class SignInModal extends Component {
  componentDidMount() {
    this.InputComponent.focus();
  }
  
  render() {
    return (
      <div>
        <label>User name: </label>
        <Input ref={comp => { this.InputComponent = comp; }} />
      </div>
    )
  }
}
```

### Components for formatting text

```jsx
const Price = (props) => {
    const price = props.children.toLocaleString('en', {
      style: props.showSymbol ? 'currency' : undefined,
      currency: props.showSymbol ? 'USD' : undefined,
      maximumFractionDigits: props.showDecimals ? 2 : 0,
    });
    
    return <span className={props.className}>{price}</span>
};

Price.propTypes = {
  className: React.PropTypes.string,
  children: React.PropTypes.number,
  showDecimals: React.PropTypes.bool,
  showSymbol: React.PropTypes.bool,
};

Price.defaultProps = {
  children: 0,
  showDecimals: true,
  showSymbol: true,
};

const Page = () => {
  const lambPrice = 1234.567;
  const jetPrice = 999999.99;
  const bootPrice = 34.567;
  
  return (
    <div>
      <p>One lamb is <Price className="expensive">{lambPrice}</Price></p>
      <p>One jet is <Price showDecimals={false}>{jetPrice}</Price></p>
      <p>
        Those gumboots will set ya back 
        <Price showDecimals={false} showSymbol={false}>{bootPrice}</Price>
        bucks.
      </p>
    </div>
  );
};
```

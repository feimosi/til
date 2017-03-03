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

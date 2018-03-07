<h1 align="center">07.03.2018</h1>

## CSS Scroll Snap Points

```html
<div class="container">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

```css
.container {
  scroll-snap-points-y: repeat(100%);
  scroll-snap-type: mandatory;

  > div {
    width: 100vw;
    height: 100vh;
  }
}
```

:arrow_right: https://css-tricks.com/introducing-css-scroll-snap-points/

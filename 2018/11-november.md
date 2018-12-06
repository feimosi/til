# SVG `<use>`

The `<use>` element takes nodes from within the SVG document, and duplicates them somewhere else.
It also allows moving elements in the tree and therefore change their z-index.

```html
<svg viewBox="0 0 30 10" xmlns="http://www.w3.org/2000/svg">
  <circle id="myCircle" cx="5" cy="5" r="4"/>
  <use href="#myCircle" x="10" fill="blue"/>
  <use href="#myCircle" x="20" fill="white" stroke="blue"/>
</svg>
```

:arrow_right: https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use

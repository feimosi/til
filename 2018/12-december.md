# CSS Shapes

Basic Shape Functions:

- circle()
- ellipse()
- inset()
- polygon()

```css
.circle {
  float: left;
  height: 200px;
  width: 200px;
  shape-outside: circle();
  clip-path: circle();
  background: linear-gradient(to top right, #FDB171, #FD987D);
}
```

If we want to actually display our shape functions, we’ll have to use the `clip-path` property.
`clip-path` takes many of the same values as shape-outside, so we can give it the same `circle()` value.

![](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2018/11/cssshapes_circle4.jpg)

We can use the `shape-margin` property to add a margin to the shape.

The most interesting and flexible of the shape functions is the `polygon()`,
which can take an array of x and y points to make any complex shape.

```css
.polygon {
  float: left;
  shape-outside: polygon(0 0, 0 300px, 200px 300px);
  clip-path: polygon(0 0, 0 300px, 200px 300px);
  height: 300px;
  width: 300px;
  background: linear-gradient(to top right, #86F7CC, #67D7F5);
}
```

![](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2018/11/cssshapes_polygon1.jpg)

You can also use a url of a semi-transparent image to define a shape, and the text will automatically flow around it.

```html
<img src="./star.png" class="star">
<p>Example text...</p>
```

```css
.star {
  float: left;
  height: 350px;
  width: 350px;
  shape-outside: url('./star.png')
}
```

:arrow_right: https://tympanus.net/codrops/2018/11/29/an-introduction-to-css-shapes/

# `forEach` with `async/await`

The following does not wait for completion:

```js
arr.forEach(async (i) => {
  await wait(i);
  console.log(i);
})
```

This does:

```js
await Promise.all(arr.map(async (i) => {
  await wait(i);
  console.log(i);
}))
```

Keeping sequential execution:

```js
await arr.reduce(async (prev, i) => {
  await prev;
  await wait(i);
  console.log(i);
}, Promise.resolve());
```

:arrow_right: https://mailchi.mp/c1914d867390/js-tips-combine-validators-1447373

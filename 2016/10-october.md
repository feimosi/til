<h1 align="center">11.10.2016</h1>

## async vs defer

![](http://www.growingwiththeweb.com/images/2014/02/26/legend.svg)

### `<script>`

![script](http://www.growingwiththeweb.com/images/2014/02/26/script.svg)

### `<script async>`

![async](http://www.growingwiththeweb.com/images/2014/02/26/script-async.svg)

### `<script defer>`

![defer](http://www.growingwiththeweb.com/images/2014/02/26/script-defer.svg)

<br>
:arrow_right: http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html

## Auto-scroll snippet
```js
function scroll() {
  document.body.scrollTop++;
  requestAnimationFrame(scroll);
}
requestAnimationFrame(scroll);
```

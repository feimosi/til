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

## Watch a file for changes
```sh
tail -F
less +F
```
With `less` you can press Ctrl+C to stop appending real time and check the file, once done you can again start appending new lines by pressing `F`.

## Copy only new or updated files
```sh
cp -u
```

## Execute the most recent command that contained the string 'foo'
```sh
!?foo
```

## Expand to the last word of the previous command
```sh
!$
```

## Repeat the last command with "sudo" prepended
```sh
sudo !!
```

## Group files by extension
```sh
ls -X 
```

## Add mulitple files by extension in Git (even in subdirectories)
```sh
git add "*.ts"
```

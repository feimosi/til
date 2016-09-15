<h1 align="center">10.09.2016</h1>

## 11 Simple npm Tricks

### Open a package’s homepage 
```sh
npm home $package
```

### Open a package’s GitHub repo
```sh
npm repo $package
```
  
### Check for packages not declared in package.json
```sh
npm prune
```

Prints a list of modules not present in `package.json` and strips them out.

### Lock down your dependencies versions
```sh
npm shrinkwrap
```

When you run `npm install` and there is a `npm-shrinkwrap.json` present, it will override the listed dependencies of `package.json`.

If you need consistency across `package.json`, `npm-shrinkwrap.json` and `node_modules`, consider using `npm-shrinkwrap`.

### Allow npm install -g without needing sudo
```sh
npm config set prefix $dir
```

Where `$dir` is the directory you want to install global modules to.

The only caveat: you need to make sure you adjust user permissions with `chown -R $USER $dir` and add `$dir/bin` to your `PATH`.

### Change the default save prefix for all your projects
```sh
npm config set save-prefix ~
```

### Strip your project's devDependencies for a production environment
```sh
npm install --production
```

Or you can set the `NODE_ENV` environment variable to `production`.

### Automate npm init with defaults
```sh
npm config set init.author.name $name  
npm config set init.author.email $email  
```

<br>
:arrow_right: https://nodesource.com/blog/eleven-npm-tricks-that-will-knock-your-wombat-socks-off/

<h1 align="center">11.09.2016</h1>

### Install the latest version of a package and save with exact number
```sh
npm install $package@* -D -E
```

<h1 align="center">14.09.2016</h1>

## Useful Lodash utility functions

### Find array values not included in the other given arrays
```js
_.difference([1, 2], [2, 3]);
// => [1]
```

### Find unique values that are included in all given arrays
```js
_.intersection([1, 2], [2, 3]);
// => [2]
```

### Group elements of several arrays
```js
_.zip(['a', 'b'], [1, 2], [true, false]);
// => [['a', 1, true], ['b', 2, false]]
_.zipObject(['a', 'b'], [1, 2]);
// => { 'a': 1, 'b': 2 }
```

### Omit given properties from an object
```js
_.omit({ 'a': 1, 'b': '2', 'c': 3 }, ['a', 'c']);
// => { 'b': '2' }
```

### Select given properties from an object
```js
_.pick({ 'a': 1, 'b': '2', 'c': 3 }, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

<h1 align="center">15.09.2016</h1>

### Allow click behaviour to “pass through” an element to another element below it 
```css
pointer-events: none;
```

## Flexbox

### Behave like an inline element and lay out the content according to the flexbox model
```css
display: inline-flex;
```

### Align a flex's lines within the container when there is extra space on the cross-axis (used with wrap)
```css
align-content: flex-start;
```

### Align flex items of the current flex line overriding the align-items value
```css
align-self: stretch;
```

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

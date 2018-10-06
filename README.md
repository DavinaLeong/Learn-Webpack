# Learn Webpack 4

## About
Me following this [tutorial](https://www.youtube.com/watch?v=deyxI-6C2u4) on Webpack4 by Traversy Media.


## Probable Errors and Possible Resolutions
*Note: This is NOT a conclusive list, just the list of errors I encountered while doing the tutorial.*

### You may need an appropriate loader...
**Full error:**
```javascript
ERROR in ./src/index.js 5:16
Module parse failed: Unexpected token (5:16)
You may need an appropriate loader to handle this file type.
| import App from './components/App.jsx';
|
> ReactDom.render(<App />, document.getElementById('app'));
 @ multi (webpack)-dev-server/client?http://localhost:8080 (webpack)/hot/dev-server.js ./src main[2]
```

**Probable cause:**

Typo of *export* in `webpack.config.js`.
```javascript
module.export
```

**Probable resolution:**

Change to:
```javascript
module.exports
```

Kill webpack and re-run `npm start`.

---

### Cannot find module '@babel/core'...
**Full error:**
```javascript
ERROR in ./src/index.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
Error: Cannot find module '@babel/core'
 babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7'.
    at Function.Module._resolveFilename (module.js:547:15)
    at Function.Module._load (module.js:474:25)
    at Module.require (module.js:596:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\developer\learn-webpack\node_modules\babel-loader\lib\index.js:10:11)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
 @ multi (webpack)-dev-server/client?http://localhost:8080 (webpack)/hot/dev-server.js ./src/index.js main[2]
```


**Probable cause:**
Outdated Babel packages.


**Probable resolution"**
Uninstall all Babel packages except `babel-loader` and reinstall them as `@babel/<babel-package>`.

Commands:
```bash
npm uninstall babel-core babel-preset-env babel-preset-react --save-dev
npm install @babel/core @babel/preset-env @babel/preset-react --save-dev
```

Package mapping:
```
babel-core -> @babel/core
babel-preset-env -> @babel/preset-env
babel-preset-react -> @babel/preset-react
```

If done right, the `devDependencies` section of your `package.json` should look like this:
```json
"devDependencies": {
    "@babel/core": "~7.1.2",
    "@babel/preset-env": "~7.1.0",
    "@babel/preset-react": "~7.0.0",
    "babel-loader": "~8.0.4",
    "html-webpack-plugin": "~3.2.0",
    "webpack": "~4.20.2",
    "webpack-cli": "~3.1.2",
    "webpack-dev-server": "~3.1.9"
}
```

Also remember to update your `.babelrc`:
```json
{
    "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

Again, kill webpack and re-run `npm start`.


## Extras
### Resolve for `.jsx` extension
After going through the tutorial, I decided to see how I could resolve for the `.jsx` extension.

First, rename `App.js` to `App.jsx`.

Then in your `webpack.config.js`, modify the `rules` section as such:
```javascript
rules: [
    {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: 'babel-loader'
    }, {
        test: /\.js$/,
        exclude: /node_modules/,
        use: 'babel-loader'
    }
]
```

Kill webpack and re-run `npm start`.
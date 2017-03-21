# Webpack Wiki


## Split app and vendors

When your application is depending on other libraries, especially large ones like Angular JS, you should consider splitting those dependencies into its own vendors bundle. This will allow you to do updates to your application, without requiring the users to download the vendors bundle again. Use this strategy when:

* When your vendors reaches a certain percentage of your total app bundle. Like 20% and up
* You will do quite a few updates to your application
* You are not too concerned about perceived initial loading time, but you do have returning users and care about optimizing the experience when you do updates to the application
* Users are on mobile

*webpack.production.config.js*

```javascript
var path = require('path');
var webpack = require('webpack');
var nodeModulesDir = path.resolve(__dirname, '../node_modules');

var config = {
  entry: {
    app: path.resolve(__dirname, '../app/main.js'),
    vendors: ['angular'] // And other vendors
  },
  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: 'app.js'
  },
  module: {
    loaders: [{
      test: /\.js$/,
      exclude: [nodeModulesDir],
      loader: 'babel'
    }]
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
  ]
};

module.exports = config;
```
This configuration will create two files in the `dist/` folder. **app.js** and **vendors.js**.

#### Important!

Remember to add both files to your HTML file, or you will get the error: `Uncaught ReferenceError: webpackJsonp is not defined`.




## Resolve Modules loaded under node_modules

```
resolveLoader: {
  root: path.join(__dirname, 'node_modules')
}
```

## Resolve modules loaded in multiple directories
```
resolveLoader: {
      modulesDirectories: [
          path.resolve(__dirname, "bower"), 
          path.resolve(__dirname, "node_modules")
          // ...
      ]
}
```

# soundcloud-client-app
Instructions to follow in order to create this app from my own notes taken from a tutorial by Robin Wieruch
from https://www.robinwieruch.de/the-soundcloud-client-in-react-redux/#aProjectFromScratch

Minimal React + Webpack + Babel - Setup

mkdir minimal-react-boilerplate
cd minimal-react-boilerplate

Next you can initialize it as npm project
npm init -y

npm config list

npm set init.author.name "<Your Name>"
npm set init.author.email "you@example.com"
npm set init.author.url "example.com"
npm set init.license "MIT"



mkdir dist
cd dist
$null> index.html


dist/index.html  ---> ADD content

<!DOCTYPE html>
<html>
  <head>
    <title>The Minimal React Webpack Babel Setup</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="/bundle.js"></script>
  </body>
</html>


npm install --save-dev webpack webpack-dev-server webpack-cli


Add in package.json
"scripts": {
  "start": "webpack-dev-server --config ./webpack.config.js --mode development",
  ...
},

From root folder create:

$null> webpack.config.js

and add in that file:

module.exports = {
  entry: './src/index.js',
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist'
  }
};


mkdir src
cd src
$null> index.js

Add any console.log("anything") in your index.js



Install Babel
npm install --save-dev @babel/core @babel/preset-env

Moreover, in order to hook it up to Webpack, you have to install a so called loader:

npm install --save-dev babel-loader

As last step, since you want to use React, you need one more configuration to transform the React’s JSX syntax to vanilla JavaScript.

From root folder:

npm install --save-dev @babel/preset-react


Now, with all node packages in place, you need to adjust your package.json and webpack.config.js to respect the Babel changes. These changes include all packages you have installed.

package.json

...
"keywords": [],
"author": "",
"license": "ISC",
"babel": {
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
},
"devDependencies": {
...


webpack.config.js

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      }
    ]
  },
  resolve: {
    extensions: ['*', '.js', '.jsx']
  },
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist'
  }
};

An optional step would be to extract your Babel configuration in a separate .babelrc configuration file.

From root folder:

$null> .babelrc

Now you can add the configuration for Babel, which you have previously added in your package.json, in the .babelrc file. Don’t forget to remove the configuration in the package.json afterward. It needs to be configured at only one place.

.babelrc

{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}

Install React

npm install --save react react-dom

From root folder:

npm install --save-dev react-hot-loader

You have to add some more configuration to your Webpack configuration file.

webpack.config.js

const webpack = require('webpack');

,
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ],
  devServer: {
    contentBase: './dist',
    hot: true
  }
};


Additionally in the src/index.js file you have to define that hot reloading is available and should be used.

src/index.js

ReactDOM.render(
  <div>{title}</div>,
  document.getElementById('app')
);

module.hot.accept();



Install REDUX

npm install --save redux

npm install --save redux-logger
The logger middleware shows us console output for each action: the previous state, the action itself and the next state. It helps us to keep track of our state changes in our application.

 connect Redux to our React components.

From root folder:

npm install --save react-redux

React Router to provide our app with some simple routing.

From root folder:

npm --save install react-router react-router-redux

You have to add the following line to your web pack configuration.

webpack.config.js
devServer: {
    contentBase: './dist',
    hot: true,
    historyApiFallback: true  // this one
  }
  
  We might need to install a polyfill for fetch, because some browser do not support the fetch API yet.

From root folder:

npm --save install whatwg-fetch
npm --save-dev install imports-loader exports-loader


webpack.config.js

var webpack = require('webpack');

 devServer: {
    contentBase: './dist',
    hot: true,
    historyApiFallback: true
  },
  plugins: [
    new webpack.ProvidePlugin({
      'fetch': 'imports-loader?this=>global!exports-loader?global.fetch!whatwg-fetch'
    })
  ]
  
  
  
   redux-thunk. The thunk middleware returns you a function instead of an action. Since we deal with an asynchronous call, we can delay the dispatch function with the middleware. Moreover the inner function gives us access to the store functions dispatch and getState.
   
   
   npm --save install redux-thunk
   
   configurationStore.js  add
   import thunk from 'redux-thunk'
   
   and thunk to 
   const createStoreWithMiddleware = applyMiddleware(thunk, router, logger)(createStore)
   
   
   We need the CLIENT_ID to authenticate the audio player with the SoundCloud API in order to stream a track via its stream_url

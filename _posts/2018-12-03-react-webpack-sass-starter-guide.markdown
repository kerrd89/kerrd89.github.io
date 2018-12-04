---
layout: post
title:  "React Webpack Sass Starter Guide"
date:   2018-12-03 18:15:05 -0700
categories: technical
---

[Create React App](https://github.com/facebook/create-react-app) is a wonderful resource which makes React accessible to beginners.  Initializing a project is the most intimidating aspect for any new developer.  Having something which just works allows people to go straight to learning React. Configuration is scary, but unfortunately default configuration also means bloated.  It doesn't matter *that* much, but with a little extra effort you can configure your own project without the bells and whistles.

The simplest basic starter for me is [React](https://github.com/facebook/react), [Webpack](https://github.com/webpack), and [SASS](https://github.com/sass/node-sass).  Any website will require these 3 things, to not include them will cost time later.  LESS is fine too, as are other Frontend frameworks.  I question using React lately because it comes from the sunken place (Facebook), but it is well supported and open-source. For bundlers, I only use Webpack. It is easy to use, well supported and offers dev-server options for hot-reloading.

Lets get started.

```
mkdir your_project
cd your_project
npm init
// Press Enter for defaults
npm install --save react react-dom
npm install --save webpack webpack-dev-server webpack-cli
npm install --save-dev babel-cli babel-core babel-loader@7 babel-preset-es2015 babel-preset-react babel-register
```

Okay at this point we stop and take a breath.  We have react and react-dom.  We have the 3 required webpack modules to setup our dev environment.  We have babel and a bunch of babel dependencies.  I *think* this is all you need to write ES6, but babel dependencies are confusing.

Speaking of [babel](https://babeljs.io/), you need to update your package.json.

```
"babel": {
    "presets": [
      "es2015",
      "react"
    ]
}
```

Now install the sass loaders and dependencies.

```
npm install --save-dev node-sass css-loader style-loader sass-loader
```

Make a basic `webpack.config.js` in the root of your project.

```
// default path for webpack configuration file
touch webpack.config.js

// in webpack.config.js
var webpack = require('webpack');
var path = require('path');

var rootDirectory = path.join(__dirname, '/');

module.exports = {
  mode: 'development',
	entry: [
		path.join(rootDirectory, '/index.js')
	],
  output: {
    path: rootDirectory + '/dist',
    filename: 'bundle.js'
  },
  module: {
    rules: [{
      test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },{
        test: /\.scss/,
        use: ["style-loader", "css-loader", "sass-loader"]
      }
    ]
  },
  devServer: {
    contentBase: rootDirectory,
    historyApiFallback: true
  }
};

```

Okay, lets make that `index.js` file we reference in `webpack.config.js` in the root our our project.

```
touch index.js

// in index.js
import React from 'react';
import ReactDOM from 'react-dom';

import App from './containers/App';

ReactDOM.render(<App />, document.getElementById('app'));
```

Now we make the App component referenced in the `index.js` file.  Make a folder for your containers and create the component there.

```
mkdir containers
touch containers/App.js

// in containers/App.js
import React, { Component } from 'react';

export default class App extends Component {
  render () {
    return <div>hello world</div>;
  }
}
```

Now, lets make a basic `index.html` in the root of the directory as an entry point.

```
touch index.html

// in index.html
<!DOCTYPE html>
<html>
    <head>
        <title>React Webpack Sass Starter</title>
    </head>
    <body>
        <div id="app"></div>
        <script type="text/javascript" src="/bundle.js"></script>
    </body>
</html>
```

Add a dev script to your package JSON.

```
...
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server"
},
...
```

Running `npm run dev` everything should load for your dev environment. Watch your console for errors.  I may have messed something up.  Also keep in mind dependencies often time change their usage with major releases.  This is the reason for installing `babel-loader@7` instead of simply installing `babel-loader`.  `babel-loader` installs the latest babel loader (`babel-loader@8`), which is incompatible with `babel-core`.  A better developer would probably have continued down that rabbit hole to find the correct usage of `babel-loader@8`, but here we are.

The last step is to confirm that you can import `.scss` files directly into your react components.

First, lets make an `assets` directory and a file `main.scss`. In `assets/main.scss` lets use a sass variable so we know it is working.

```
mkdir assets
touch assets/main.scss

// in main.scss
$red: red;

div {
  background-color: $red;
}
```

In `/containers/App.js`, add an import statement at the top after importing modules and other dependencies.

```
import '../assets/main.scss';
```

Now, running `npm run dev` will show your background as red instead of white.

That should be all you need to get started! Please let me know how I can improve this if you disagree with any of the default options I used here.  Thanks for reading!

---
layout: post
title:  "Setting-Up ESLint with Webpack and React"
date:   2018-12-05 18:15:05 -0700
categories: technical
---

[ESLint](https://eslint.org/) is bumper lanes for your project.  Text editor extensions are useful while coding, but ESLint can enforce norms accross developers and remove the unpleasant discussions about syntax.  Agree to syntax norms once and move forward.  If individuals have issues with particular rules, they can present a coherent argument as to why they want to change that rule.

## Getting Started

Lets start by installing npm dependencies.

```
npm install --save-dev eslint babel-eslint eslint-plugin-react eslint-watch
```

Next, lets create a basic `.eslintrc` file in the root of your directory.  Eslint will work without this, but I would encourage customizing rules for your project.  Here is a basic config file for `react@16`.

```
{
    "plugins": [
        "react"
    ],
    "parser": "babel-eslint",
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "env": {
        "es6":     true,
        "browser": true,
        "node":    true,
        "mocha":   true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "rules": {
      "semi": [2, "always"],
      "indent": ["error", 2],
      "object-curly-spacing": ["error", "always"],
      "no-extra-parens": "error",
      "max-len": ["error", { "code": 100 }],
      "no-multi-spaces": "error"
    },
    "settings": {
      "react": {
        "version": "16.3"
      }
    }
}
```

## Running the linter

The linter can be run against a single file using the node_module binary.

```
./node_modules/eslint/bin/eslint.js index.js
```

It is better to configure NPM scripts to do this for you.  Here are what mine look like for a basic react starter project.

```
// in package.json under scripts
...
"lint": "eslint ./index.js ./containers/*.js ./components/*.js",
"lint:fix": "eslint --fix ./index.js ./containers/*.js ./components/*.js",
...
```

A few notes on the syntax there.  The `*.js` refers to any file in that directory with a `.js` extension. If I had folders with components in my `/components` folder, I could test all folders and files within components like this `./components/**/*.js`.

```
...
"lint": "eslint ./index.js ./containers/*.js ./components/**/*.js",
"lint:fix": "eslint --fix ./index.js ./containers/*.js ./components/**/*.js",
...
```

The fix option is your best friend.  It will fix basic linter errors for you.  Anyone who has had to do this manually for basic rules will appreciate what a help this is.

## Example Outputs

If this ran correctly, you should see an output like the following.

```
> eslint ./index.js ./containers/*.js ./components/*.js


/home/david/blogbank.com/components/EnhancedTable.js
   65:4   error  Unnecessary semicolon                          no-extra-semi
  126:1   error  Expected indentation of 8 spaces but found 10  indent
  127:1   error  Expected indentation of 8 spaces but found 10  indent
  128:1   error  Expected indentation of 6 spaces but found 8   indent
  130:1   error  Expected indentation of 8 spaces but found 10  indent
  131:1   error  Expected indentation of 8 spaces but found 10  indent
  132:1   error  Expected indentation of 6 spaces but found 8   indent
  154:28  error  Unnecessary parentheses around expression      no-extra-parens
  158:13  error  Unnecessary parentheses around expression      no-extra-parens
  166:28  error  Unnecessary parentheses around expression      no-extra-parens
  172:13  error  Unnecessary parentheses around expression      no-extra-parens
  313:33  error  Unnecessary parentheses around expression      no-extra-parens

/home/david/blogbank.com/components/SearchAppBar.js
   5:8  error  'IconButton' is defined but never used  no-unused-vars
  10:8  error  'MenuIcon' is defined but never used    no-unused-vars

/home/david/blogbank.com/components/SimpleTable.js
  24:5  error  'id' is assigned a value but never used  no-unused-vars

✖ 15 problems (15 errors, 0 warnings)
  12 errors and 0 warnings potentially fixable with the `--fix` option.
```

Now, we can fix those errors using our `npm run lint:fix` script. Without doing anything, 12/15 errors are auto-fixed.

```
> eslint --fix ./index.js ./containers/*.js ./components/*.js


/home/david/blogbank.com/components/SearchAppBar.js
   5:8  error  'IconButton' is defined but never used  no-unused-vars
  10:8  error  'MenuIcon' is defined but never used    no-unused-vars

/home/david/blogbank.com/components/SimpleTable.js
  24:5  error  'id' is assigned a value but never used  no-unused-vars

✖ 3 problems (3 errors, 0 warnings)
```

That is it! Go forth and play nicely with other developers instead of arguing about syntax!

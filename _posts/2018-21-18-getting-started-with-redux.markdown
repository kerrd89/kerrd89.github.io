---
layout: post
title:  "Getting Started With Redux"
date:   2018-12-18 18:15:05 -0700
categories: technical
---
I found [Redux](https://redux.js.org/) a bit intimidating.  There are actions, reducers, types, and you have to connect the store to your components.  Oh, and don't forget to implement middleware.  It can be difficult to debug across so many different components, especially during set-up.

In my opinion, Redux is overkill for most projects.  You can create a simple store through a root component's state that is far easier to manage, understand, and implement.

If you are building something that you intend to be a production application, it is probably worth implementing React/Redux.  It makes debugging easy since you have a log of all actions and state changes.  It makes testing easy since you have explicit methods managing state.

![React-Redux](/assets/react-redux.png)

The most important principle with Redux is to keep state flat.  Avoid nesting objects when you can reference ID's and create a separate store for an item.  For example, if you have a company with has employees, this is the wrong way to represent your state.

```
--company
	-id
	-name
	-employees: [ employee_1, employee_2 ]
```

These nested objects can lead to [javascript reference errors](https://medium.com/@dkerrious/copying-and-object-in-javascript-64ca048cd8c7).  The most common way to copy an object is with Object.assign({}, state, { changes }).  Unfortunately, this creates reference issues for any employees since Object.assign only applies first level properties.

Instead, we can flatten our state and reference those employees by their id.

```
--employees: [ employee__1, employee__2 ]
--company
	-id
	-name
	- employees: [1, 2]
```

Now, we can import employees only into the components which need them.  The more you can avoid passing extra data around, the better.

In order to get redux set-up and implemented, we need to follow 3 simple steps.  First, install dependencies.  Second, create a basic redux store.  Third, access that basic store in your components.

## Install Dependencies ##
```
npm install --save redux  react-redux redux-thunk
```
I would also recommend installing the Redux Devtools extension on whatever browser you use for development.  It provides a visual representation of state as well as a log of events making debugging a breeze.

## Create a Basic Redux Store ##
```
mkdir store
touch store/configureStore.js

// in configureStore.js
// import redux methods, compose is used to combine redux
// applyMiddleware implements redux-thunk
import {createStore, compose, applyMiddleware} from 'redux';
import rootReducer from '../reducers/rootReducer';
import thunk from 'redux-thunk';

export default function configureStore() {
  return createStore(
    rootReducer,
    compose(
      applyMiddleware(thunk),
      window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__(),
    )
  );
```

`compose` from redux is necessary to add multiple libraries to your redux store, like middleware and dev tools.  `createStore` used to take more than 2 values, so many tutorials online will not include `compose`.  Latest versions require using compose.  Redux-thunk will prevent async issues associated with a centralized data store.

Now, we need to create a `rootReducer`.  As a rule, you should have a reducer for each thing in your application.  For this example, we will only have a blogs reducer.  The rootReducer will pull in a blogs store.  The `configureStore.js` file will instantiate that store.  Let's create our reducers.
```
mkdir reducers
touch reducers/rootReducer.js
touch reducers/blogReducer.js

// in rootReducer.js
import {combineReducers} from 'redux';
import blogs from './blogReducer';

const rootReducer = combineReducers({ blogs });

export default rootReducer;
```

Now, we have to create the `blogReducer`.

```
touch reducers/blogReducer

// in blogReducer.js
export default function blogs(state = [], action) {
  switch (action.type) {
    default:
      return state;
  }
}
```

Nothing fancy here.  Our default state is an empty array.  All we are doing is returning in that default scenario.  This is all we need to set-up a store with `blogs: []`.

Now, we need to pass that store into our root component.  Up until now, everything we have been doing has been using `redux` or `redux-thunk`.  Now, we will include `react-redux` in the fun.  In your root component rendering your application, wrap your application in the Provider component.
```
// in index.js
import React from 'react';
import { render } from 'react-dom';
import { Provider } from 'react-redux';
import App from './containers/App';
import configureStore from './store/configureStore';

const store = configureStore();

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('app')
);
```

If you start your application and open your redux dev tools you should see your `blogs: []` store!

## The last step is connecting your redux store to a component. ##

To connect our blogs store, we will use a sample SimpleTable component.  Replace this with any component in your library.

```
import { connect } from 'react-redux';
...
// at the bottom of SimpleTable.js

SimpleTable.propTypes = {
  ...
  blogs: PropTypes.array.isRequired,
  ...
};

function mapStateToProps({ blogs }) {
  return { blogs };
}

export default connect(mapStateToProps)(SimpleTable);
```

You should now be able to access `this.props.blogs` from within your component.   If you are unsure, console logs are your friend.

If you are also using React Router, I would advise configuring your routes and components inside an `App.js` component and setting up Redux in an `index.js` file.

Good luck with your project, happy prying!

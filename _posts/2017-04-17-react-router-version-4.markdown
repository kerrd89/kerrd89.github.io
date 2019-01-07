---
layout: post
title:  "React Router Version 4"
date:   2017-04-17 18:15:05 -0700
categories: technical
cover: "/assets/react_router_v4.jpeg"
---
As with any new technology, resources and documentation online lack for using the new version of React Router. The goal of this article is to show and explain a basic implementation of React Router v4, as well as to highlight useful features and key differences from prior versions. Whether you are familiar with React Router or new to the module altogether, my hope is that this article can help someone implement this simplified and streamlined router.

![React Router v4](/assets/react_router_v4.jpeg)

Here is the link to the [Github Documentation](https://github.com/ReactTraining/react-router), which outlines specific dependencies depending on your technical stack. For the basic in browser implementation, you will need to install react-router and react-router-dom.

```
npm install react-router react-router-dom —-save
```

Popular patterns for prior versions of React Router have a `routes.js` file to centrally managing the routing depending on the path, but in my opinion, this is no longer necessary. In my root React component, which I am calling `App.js`, I can select which specific components to display depending on the path.

```
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import Header from "./components/Header";
import Home from "./components/Home";
import Dashboard from "./components/Dashboard";
import Footer from "./components/Footer";
class App extends Component {
  constructor() {
    super();
    this.state = {
      stuff: stuff;
    };
  }
render() {
 let { stuff } = this.state;
 return (
  <Router> //wrapper for your router, given alias from BrowserRouter
   <div className="App">
    <Header /> //this component will always be visible because it is outside of a specific Route
    <Route exact path="/" component={Home}/>  //at the root path, show this component
    <Route path="/dashboard" component={()=><Dashboard stuff={stuff} />}/>  //at the path '/dashboard', show this other component
    <Footer /> //this is also permanently mounted
   </div>
  </Router>
 );
 }
}
export default App;
```

There are two ways to link components to routes. The first option is to use the syntax found in most react-router tutorials and documentation.

```
<Route exact path="/" component={Home} />
```

This has its pluses and minuses. The benefit is that sub-components are passed default properties which are really useful for navigation.

![React Router History](/assets/router_history_example.png)


Being able to call `this.props.history.goBack()` allows you to quickly change the state of the application without linking to a specific page. There are other useful methods as well which I encourage you to explore, but to keep this article simple I will not address them here.

The downside of this approach is you cannot pass properties from the parent component down into sub-components. To do this, you need to use the alternative syntax to link a component to a given route.

```
<Route path="/dashboard" component={()=><Dashboard stuff={stuff} />}/>
```

As someone who prefers to manage state as close as possible to the root React component, I prefer the second approach. That being said, being a good programmer is about using the tools available to solve a specific problem, not trying to solve every problem using the same tools and patterns. Depending on your specific use case, you might find one more practical than the another.

The final basic react-router feature I will point out is the Link method.

```
import { Link } from 'react-router-dom';
...
<Link to="/dashboard">Dashboard</Link>
<Link to="/home">Home</Link>
```

This replaces `<a href=””></a>` whenever you want to navigate within your application.

React Router is as complex or as simple as you want to make it. For additional information on using the Router, I would suggest you start at the official training link provided in the documentation. There is an embedded video which goes into much greater depth about all the nifty features available in the router.

If you felt I misrepresented the basics of react-router at all, please don’t hesitate to let me know with a response. I am always looking to improve these posts to help others and by no means consider myself an expert on the subject.

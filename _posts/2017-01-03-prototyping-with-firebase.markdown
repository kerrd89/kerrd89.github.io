---
layout: post
title:  "Prototyping with Firebase"
date:   2017-01-03 18:15:05 -0700
categories: technical
---

What firebase is doing is brilliant. They simplified a customizable, distributed, non-relational database to the point where front end developers can build a viable prototype without needing to invest in backend resources. This convenience can save developers valuable set-up time, but be warned that the basic implementation of firebase is not the right answer for all projects.

*If* your project requires a relational database and continuity to users, then you should use an SQL database to avoid the cost and embarrassment of trying to make those changes without affecting user experience. There are ways to customize data-structures here, but mastering these tools is outside of the scope of this post.

*If* all you need is to keep track of users, basic aggregated information, or a single table of data, then using firebase is a great option. OAuth, WebSockets, free CDN, free hosting, and large enough limits that it only costs you once you have enough users to justify real investment.

Initiating firebase in your project is as simple as creating a `firebase.js` file, instantiating firebase, and exporting that instantiation of the firebase database for use in your application. Firebase provides simple and secure OAuth with several options for providers. You just need to enable the sign in method you want to use and follow the steps for registering your application with that provider.

![Sign In Providers](/assets/sign-in-providers.png)

Here is an example of the code, including the exporting of the database and login/logout methods.

```
import firebase from 'firebase';
const config = {
    apiKey: '##############',
    authDomain: 'turing-fridays.firebaseapp.com',
    databaseURL: 'https://turing-fridays.firebaseio.com',
    storageBucket: '###########',
    messagingSenderId: '#########'
  };
firebase.initializeApp(config);
const auth = firebase.auth();
const provider = new firebase.auth.GithubAuthProvider();
export default firebase;
export const signIn = () => auth.signInWithPopup(provider);
export const signOut = () => auth.signOut();
```

In your application, you can now use firebase and access the user object. In an on-load function (I am using react’s ComponentDidMount), firebase will check if a user has an active authentication session. If so, I can set that user object aside for later use. If not, I can show a login screen.

```
firebase.auth().onAuthStateChanged(user => this.setState({ user }));
```

You can also create a database reference object in your firebase.js file, export that, and have access to adding, updating, and deleting database objects from your application.

```
export const reference = firebase.database().ref('messages');
```

Later in my app, I can use that reference:

```
reference.push(message);
reference.update();
```

This is all the code you need to create a basic app using a central data-store and OAuth from firebase. You can go one step further by subscribing your app to firebase data, so as updates occur on the server they are proactively pushed to users. To do this, we can subscribe to a firebase ‘value’ event listener which triggers on a value change in firebase. I put this in my on-load function as well, directly beneath where I resolve the user.

```
reference.limitToLast(100).on('value', (snapshot) => {
  const messages = snapshot.val() || {};
  this.setState({
    messages: map(messages, (val, key) => extend(val, { key }))
    });
  });
```

Firebase solves problems like OAuth, accessing that user data, WebSockets, and basic database features which can speed up the development cycle of basic apps and prototypes. As I mentioned initially, each project has unique needs. Its important to pick the technical stack which makes sense for your project rather than the technical stack which is easiest to the developer.

Firebase is a great solution for proof of concepts and basic databasing needs, but is not a long term solution for most companies. If you have a viable business it’s cheaper for you to use a custom backend. If you don’t know if you have a viable business, then firebase is a simple answer to quickly get a product to market for testing.

Originally posted in [Hackernoon](https://hackernoon.com/) as [Prototyping with Firebase](https://hackernoon.com/prototyping-with-firebase-5d987c2169ac).

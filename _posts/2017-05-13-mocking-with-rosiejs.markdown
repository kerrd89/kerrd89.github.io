---
layout: post
title:  "Mocking with RosieJs"
date:   2017-05-13 18:15:05 -0700
categories: technical
---
After failing to find a strong RosieJs tutorial for creating mock data for testing or development purposes, I thought I would share my experiences using the library in hopes it helps someone use this awesome tool.

## First, when and why you need RosieJS.

We all know testing is important. But in a fast-paced environment, it is the first thing to be thrown into the ‘technical debt’ pile. Eventually, after that pile becomes too much to ignore or step over, we go back to try and test something we hardly remember building. At this point, we usually encounter the issue (if you are using a modern MVC framework) of how to test components without access to external data. Applications are designed to not work without the correct data, and for good reason. Now, instead of creating a helpers directory and copying and pasting JSON responses, try these simple steps to create a RosieJS data ‘factory’ to build objects as you need them.

(Here is a link to the docs)[https://github.com/rosiejs/rosie], which welcome you with an encouraging message from Rosie herself I assume.

## Getting started

```
npm install rosie --save-dev
```

If this is something you are only using to support testing, remember to keep this dependency in your dev-dependencies list.

Create a factory file, probably in a helpers folder.

```
// using ES6 deconstruction
import { Factory } from 'rosie'
// or not
var Factory = require('rosie').Factory
// define an object you want to build
Factory.define('user')
  .sequence('id')
  .attr('first_name', 'Jane')
  .attr('last_name', 'Doe')
export default Factory
```

* Define the object type you want to create. If you have widgets, this would be a single widget.
* Create a primary key id using sequence. This avoids errors from having duplicate objects in a map or render.
* An object key is added using attr. The second value this takes is either a function(more on more advanced options later) or in this example, just a default value.

Pretty straightforward right?

Import the exported/updated factory module from the factory file, and start building things!

```
// import the factory into a test file using a relative path to your factory
import Factory from '../helpers/factory'
// build a user
let user = Factory.build('user')
// any key included in the second parameter will overwrite the default value, in this case we have Jane's brother Jon
let user = Factory.build('user', { first_name: Jon })
console.log(user)
// { id: 2, first_name: Jon, last_name: Doe }
// build a list of users
let users = Factory.buildList('user', 4)
console.log(users.length)
//4
```

## More complex options

Now, back to the factory to show some more advanced options for creating more complex objects and data structures.

```
import { Factory } from 'rosie'
// lets make a second factory for our user factory to use
Factory.define('account')
  .sequence('id')
  .attr('balance', '100.00')
Factory.define('user')
  .sequence('id')
  .option('verified', false)
  .attr('first_name', 'Jane')
  .attr('last_name', 'Doe')
  .attr('account', ['verified'], (verified) => {
     if (!verified) { return null }
     else { return Factory.build('account') }
  })
```

* Create an optional state with option. This takes a default value as well.
* Import a dependency into the attribute, as the second parameter (as a string in an array), the third parameter becomes a function to create that attributes value.
* We can import any other attributes as well. If we wanted to build a full_name attribute, we could create that attribute using the first and last name fields.
```
.attr('full_name', [‘first_name’, ‘last_name’], (first, last) => {
  return `${first_name} ${last_name}`
}
```
* We can use the option to create different types of the same objects, which can be really useful for sad-path testing.

Using our updated user factory to create verified and non verified users.

```
import Factory from '../helpers/factory'
// if no option is provided the default for verified will be false
let unverfied_user = Factory.build('user')
console.log(unverified_user.account)
// null
// passing an option value is similar to passing an attribute, but in the 3rd parameter
let verified_user = Factory.build('user', {}, {verified: true})
console.log(verified_user.account)
// { id: 1, balance: '100.00' }
```

We can use these options, and other Factories, to easily build custom states for application testing.

RosieJs is a readable, easy to use, and scaleable alternative to copying and pasting JSON. Whether you are using RosieJS for testing or to support your development process, with minimal work you can create custom data factories which can do the heavy work for you. Plus, you get to say you made a data factory which sounds pretty cool.

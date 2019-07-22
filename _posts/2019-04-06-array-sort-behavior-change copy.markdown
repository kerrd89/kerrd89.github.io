---
layout: post
title: "Array.prototype.sort() Behavior Change in Node v11"
date: 2019-04-05 18:15:05 -0700
categories: technical
---
Several weeks ago, I started to experience random test failures in a previously stable part of an application.  I started where any developer would start, asking myself if there were any changes recently which might have caused these issues.  There was nothing, and [the real culprit](https://github.com/nodejs/node/issues/24294) was not even on my radar.

Between Node v10 and Node v11, the behavior of Array.prototype.sort() changed.  Previously in Node v10, this method accepted a boolean as a return value.  Starting in Node v11, functions returning a boolean will not sort an array as expected.

Here is a [guide for setting up `nvm` on macOS](http://dev.topheman.com/install-nvm-with-homebrew-to-use-multiple-versions-of-node-and-iojs-easily/) if you would like to play around with this for yourself.

## Simple Demonstration of Changes ##

In Node v10:
```
var array = [ 3, 2, 1 ];
array.sort((a,b) => a > b);
[ 1, 2, 3 ]
```

In Node v11:
```
var array = [ 3, 2, 1 ];
array.sort((a,b) => a > b);
[ 3, 2, 1 ]

// instead, return a 0, negative, or positive value
array.sort((a, b) => a - b);
[ 1, 2, 3 ]
```

## What I Learned ##

After seeing this, I had to go back and check on some other projects which had the same misuse of the `Array.prototype.sort()` method.  I realized through this experience that it is my job as a developer to be proactive.  Being proactive as a developer means staying on top of the latest information relating to technologies I am using as well as making an effort to get in front of those potential problems in applications.
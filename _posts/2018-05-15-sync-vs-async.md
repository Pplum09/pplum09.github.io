---
layout: post
title: Sync Vs Async
date: 2018-05-15 05:00:00
tags: ajax asynchronous synchronous functions
author: Izzy
published: true
---

Coming out of cs 201, asynchronous functions were super confusing to me.
My introduction into javascript was through trying to use socket.io in NodeJS.
The first time I saw an asynchronous function, I thought that the documentation was surely wrong.
Here's my gift back to the programming noobs that were just as confused as I was.

#### Synchronous Programming
When students first learn how to program, they're most likely taught how to program in a synchronous manner.
This means that instructions in a program run sequentially, from top to bottom, where each line will execute only when the previous line has finished.

```javascript
function add(x, y) {
    var ans = addition_that_takes_five_seconds(x, y);
    return ans;
}

var x = 2;
var y = 3;
var ans = add(x, y);
console.log(ans); // 5
```

In the example above, ```console.log()``` will not be run until the function ```add()``` completes.
You can see that the function will wait 5 seconds before continuing with the rest of the instructions.
Note that while the program waits for ```add()``` to return, the program can do nothing else and is therefore __blocked__.

#### Asynchronous Programming
There may be times where you want to do other things while you are waiting for an asynchronous function to return. A great example is how you are able to make comments on Youtube videos while the video is still loading. If the video loading process was synchronous, then you wouldn't be able to do that.

```javascript
function async_add(x, y) {
    var ans = addition_that_takes_five_seconds(x, y);
    return ans;
}

var x = 2;
var y = 3;
var ans = async_add(x, y);
console.log(ans); // undefined
```

The javascript continues to run as if ```async_add()``` had already returned, even though it hasn't, and because it hasn't yet, ```result``` is not given a value in time for it to be defined by the time the print statement is ran.
So then how will the program know when ```async_add()``` is done and then print the answer?

#### Callback Functions
Callbacks are the last instruction run in an async function. Whatever is defined within the callback is what we want to do with the result of the asynchronous call.

```javascript
function async_add(x, y, callback) {
    var ans = addition_that_takes_five_seconds(x, y);
    callback(ans);
}

function add_cb(result) {
    console.log(result);
}

var a = 2;
var b = 3;
async_add(a, b, add_cb);
```

Even with the code shown above, this concept can still be pretty confusing. Let's first take a look at the ```async_add()``` function.

```javascript
function async_add(x, y, callback) {
    var ans = addition_that_takes_five_seconds(x, y);
    callback(ans);
}
```
In the case with the function above, the parameters mappings are shown below.
* param x           = a
* param y           = b
* param callback    = add_cb

In this example, ```addition_that_takes_five_seconds()``` is a pseudo code representation of an http request to a server that takes a long time to respond. Once the wait is over, ```result``` is populated and we can then run the callback with the ```result``` param passed in. Now from within the callback, ```add_cb```, we can print out the result.

Quick side note that ```var x = functionName``` will copy over ```functionName``` to the ```x``` alias. However calling the function with the parenthesis, ```var x = functionName()```, will __execute__ it. That is why you are able to just pass in the ```add_cb``` function by name to the ```async_add()``` function and have it be executed from within.

#### Inline Callback Functions
So because javascript is confusing af, you can also just define the callback function in the spot where you pass it in as a param.

```javascript
function async_add(x, y, callback) {
    var ans = addition_that_takes_five_seconds(x, y);
    callback(ans);
}

var a = 2;
var b = 3;
async_add(a, b, function (result) {
    console.log(result);
});
```

Doesn't the above look much more familiar? You've probably seen this pattern a lot of times through your javascript career. Below are some examples of where you might have seen them.

```javascript
$('#someDiv').click(function() {
    alert('woahhh callback function');
});

$(document).ready(function() {
    alert('O M G a callback function');
});

$.get('someApp.com/tweets/', function(result) {
    alert('omg here are all of the tweets:' + results);
});


array.async(x => x.result + 2);
```

With that explanation out of the way, let's now talk about where you'll probably see this the most.

#### AJAX
In case you never knew what this stood for:
* A - Asynchronous
* J - JavaScript
* A - And
* X - XML

AJAX is one way to perform HTTP requests using javascript. It's one of the first ways that you learn how to get data from a server and display it in your HTML. So here's a basic example of AJAX.

```javascript
$.ajax({
    url: 'someApp.com/tweets',
    success: function(result) {
        alert('result: ' + result);
    }
});
```

So now let's break down this function call. ```$.ajax()``` is probably a wrapper around an asynchronous function that performs the HTTP request. It probably also accepts a json that describes what endpoint to hit, ```url```, and what callback function to run, ```success```, when the async function does return. This is just a guess, but I would assume that the ```$.ajax()``` pseudo code would look something like this.

```javascript
var $ = {};

$['ajax'] = http_request; // remember that this is the function defined below

// wrapper
function http_request(request_params) {

    var url = request_params.url;
    var success_cb = request_params.success;
    do_http_request(url, success_cb)
}

// async function
function do_http_request(url, callback) {
    var result = do_thing_that_takes_5_minutes(url);
    callback(result);
}
```

All right hopefully that AJAX example wasn't super confusing.
With that being said, congrats!
You are now the master of basic callback functions.
If you find any mistakes in this doc, create an issue [here](https://github.com/Pplum09/pplum09.github.io).

If you're here to learn about asynchronous functions, then maybe you might also be interested in how I approach [git collaboration](https://pplum.io/2018/03/24/git-collaboration.html)

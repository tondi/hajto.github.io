---
layout: post
title: "JS (!) For Dummies #4"
status: released
type: post
published: true
comments: true
draft: true
---

Most of programming languages share an idea of Functions, a reusable piece of code. In JavaScript you have pretty much same thing, but the thing that differs is that you can treat some Functions as variables and pass them around and even return them as result of other Functions.

<!--more-->
<h1> Functions </h1>

Basic Function syntax:
{% highlight javascript %}
function name(someparams){
  //body
}

var name = function(someParams){
  //some instructions
}
{% endhighlight %}

But in JavaScript you have also anonymous Functions, which are normal Functions without a name. Why do we need a Function without a name? Some people use them to encapsulate things, some use them to pass the anonymous functions as parameters. Let's do some exercises.

{% highlight javascript %}
Example 1
var arrayWithFunctions = [
  function(){
    console.log(1)
  },
  function(){
    console.log(1+2)
  },
  function(){
    console.log("It works")
  }
]

for(var i=0; i<arrayWithFunctions.length; i++)
    arrayWithFunctions[i]()

Example 2
var something = 1;
(function(){
  console.log(something)
  var anotherThing = 2
  console.log(anotherThing)
})()
console.log(anotherThing)

{% endhighlight %}

The first example will of course log 1, 2 and "It works", because as i mentioned you can treat functions as variables in most cases. They are stored in array and being called with "()". You could also pass some parameters if you would like.

The second example is a bit more interesting. The program should log 1 and 2 but you shouldn't see second 2, instead you should see "anotherThing" is undefined. Why? Because the presented "pattern", "construction" or whatever you want to call it encapsulates everything inside and denies access to those things.
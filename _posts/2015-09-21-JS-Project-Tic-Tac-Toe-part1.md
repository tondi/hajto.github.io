---
layout: post
title: "JS Project #1: Tic Tac Toe"
status: draft
draft: true
type: post
published: true
comments: true
---

  I have receiver many requests about making some guidelines materials about making Tic-Tac-Toe school project on PhoneGap. I won't cover PhoneGap strict part of job because that's actually very easy. The think i will do is breaking this project in smaller accomplishable parts.
   
   
   <h2>1. Drawing some blocks! </h2>
   
   The first we need to do is make some blocks that will represent the game's playground. Most of Tic-Tac-Toe implementations offer playground in square sizes for example 3x3, 4x4, 5x5 so will ours. So to begin something we need to create some that will be clickable blocks. You might want to look at me previous article about this. [Previous]({% post_url 2015-09-14-JSForDummies1 %})
   
   {% highlight javascript %}
    //Creation of an HTML element for example div
    var createdElement = document.createElement("tagname")
    //Application of styles
    createdElement.style.property = "value"
    //adding some event listeners
    createdElement.addEventListener("click",function(event){
        //do some awesome onClick stuff here
    },false)
   {% endhighlight %}
   
   Now that we got that clear we will obviously want to generate more than div
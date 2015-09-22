---
layout: post
title: "JS Project #1: Tic Tac Toe p1"
status: released
#status: released
type: post
published: true
comments: true
---

  I have receiver many requests about making some guidelines materials about making Tic-Tac-Toe school project on PhoneGap. I won't cover PhoneGap strict part of job because that's actually very easy. The think i will do is breaking this project in smaller accomplishable parts.
   
   
 
 
 
   <h2>1. Drawing some blocks! </h2>
   
   The first we need to do is make some blocks that will represent the game's playground. Most of Tic-Tac-Toe implementations offer playground in square sizes for example 3x3, 4x4, 5x5 so will ours. So to begin something we need to create some that will be clickable blocks. You might want to look at me previous article about this. [Link]({% post_url 2015-09-14-JSForDummies1 %})
   
   {% highlight javascript %}
    //Creation of an HTML element for example div
    var createdElement = document.createElement("tagname")
    //Application of styles
    createdElement.style.property = "value"
    //Add some CSS class
    createdElement.className = "class1 class2"
    //adding some event listeners
    createdElement.addEventListener("click",function(event){
      //do some awesome onClick stuff here
    },false)
   {% endhighlight %}
   
   Now that we got that clear we will obviously want to generate more than div... So how are we gonna achieve that? Do you remember our old little friend for loop? It might pretty helpful here, because it can hold some state on variables that it uses to iterate. That variable might be also with positioning those divs, because we are gonna position them absolutely. All styles that remain static i suggest assigning to some CSS class giving this class to generated element. So let's get this done.
   
   {% highlight javascript %}
    div.gameBlock {
     position: absolute;
     background-color: whatever you want;
     color: whatever you want different than background;
    }
   {% endhighlight %}
   
   {% highlight javascript %}
    var elementsCount = 3;
    //c stands for columns, r stands for rows
    for(var c = 0; c < elementsCount; c++){
     for(var r = 0; r < elementsCount; r++){
      //create Your elements here
      //I suggest assigning margin-left or left to width * numberOfElement
      //and assigning margin-right or right to width * numberElement
     }
    } 
   {% endhighlight %}
   
   So i guess that your model element might be just a little bit too big, isn't it? Wouldn't be awesome to make it always fit on the screen? So every screen has two dimensions width and height and if you want to make your playground always visible you have to take the lower value and based on that calculate each blocks width. It's worth noting that every element besides width have also padding and margin you have to take in count. Here's the equation you will have to implement.
   
   {% highlight javascript %}
    computationalWidth = screenWidth min screenHeight
   
    width + margin + padding = computationalWidth / numberOfElements
   {% endhighlight %}
   
   So if you want have pure width you have to manipulate it like so.
   
   {% highlight javascript %}
    width = computationalWidth / numberOfElements - margin - padding
    //Where padding and margin are constants that you declared in CSS
    // ofcourse you have to type them manually in JS
   {% endhighlight %}
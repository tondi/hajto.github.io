---
layout: post
title: "JS Project #1: Tic Tac Toe p2"
status: released
type: post
published: true
comments: true
---

  I have receiver many requests about making some guidelines materials about making Tic-Tac-Toe school project on PhoneGap. I won't cover PhoneGap strict part of job because that's actually very easy. The think i will do is breaking this project in smaller accomplishable parts.
   
   <!--more-->
   
   <h2> 2. Let's make it interactive! </h2>
   
   In previous article i have shown you how to draw a perfectly scaled playfield. Let's make it a little bit reactive. In [second]({% post_url 2015-09-16-JSForDummies2 %}) article i showed you how to add some event listeners to HTML elements. Let's do that. In the loop we wrote previous time we should add some onClicks. 
   
   {% highlight javascript %}
       var elementsCount = 3;
       //c stands for columns, r stands for rows
       for(var c = 0; c < elementsCount; c++){
        for(var r = 0; r < elementsCount; r++){
         var element = document.createElement("div")
         div.addEventListener("click",function(event){
          var clickedDiv = event.target
          clickedDiv.innerHTML = "X"
         },false)
        }
       } 
      {% endhighlight %}
      
   Above code should allow you yo write letter X to inside of clicked div. What you want to do now is make a global (unfortunately i can't see any other way of storing state that can be accessible between callbacks) variable that will hold data about what letter should be written next. I suggest doing making a bool and negating each time you click. But i will leave that to you and your programming taste.
   
   Now we need to store which div was marked with which mark i suggest using an integer two dimensional array.  By the time of reading this you should have an idea how to make one. Let's take 0 as neutral, 1 as X and -1 as Y. After we have done that we need somehow bind the cells of array to corresponding divs. You could either divide the left, top values by width but i think that we could make use of brand new HTML5 custom attributes system.
   
   
   Setting some attributes
   
   {% highlight javascript %}
    //i'll reuse element from previous example, 
    //think of this code as being inside of two above loops
    element.setAttribute("data-row",r)
    element.setAttribute("data-col",c)
   {% endhighlight %}
   
   Reading Values
   
   {% highlight javascript %}
    //i'll reuse element from previous example, 
    //think of this code as being inside of two above loops
    element.getAttribute("data-row")
    //will return string containing row number (counting from 0)
    element.setAttribute("data-col",c)
    //will return string containing col number (counting from 0)
   {% endhighlight %}
   
   Cool we've done almost everything! Now it's time to mix it together. You have to modify your onclick to access your array with coordinates you read from element and write -1 or 1 (depending on whose turn is now). I hope you'll manage this. Feel free to ask questions if you have problems.
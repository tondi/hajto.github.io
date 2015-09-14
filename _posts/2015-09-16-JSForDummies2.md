---
layout: post
title: "JavaScript (!) for dummies #2 Making your work more responsive, handling HTML element objects"
#status: draft
type: post
published: true
comments: true
---

Yey! Now we know a lot about JS. It's time to make our code react to users actions. JS handles such things using concepts called EventListeners. If you want program to react on users click you'll have to "create" an event suitable listener. But how we are gonna do that? Every single listener is assigned in the same manor.

{% highlight javascript %}
  htmlObject.addEventListener("listerName",callback,bobbing)
{% endhighlight %}

But first of all we need create onLoad eventListener which triggers when every element on page has loaded. It's important because you can't assign eventListener to element that hasn't been loaded yet.

{% highlight javascript %}
  window.addEventListener("load",function(){
    //Do some stuff
  },false)
{% endhighlight %}

But how we gonna access HTML tags? It's simple. All of those tags are stored in HTML DOM object that you can use. If you want to learn more about HTML DOM itself you should read following articles wrote by people that probably know more than me.

- <a href="http://www.w3schools.com/jsref/dom_obj_object.asp">W3Schools</a><br/>
- <a href="https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model">Mozilla Developer</a>

{% highlight javascript %}
  window.addEventListener("load",function(){
    var div = document.getElementById("div1")
    var input = document.getElementById("input1")
    console.log(div.innerHTML)
    console.log(input.value)
  },false)
{% endhighlight %}
{% highlight html %}
  <div id="div1" > Some very, very important content </div>
  <input id="input1" value="Some important info" />
{% endhighlight %}

Following code will print the div1's contents as string and input1's value. You should get familiar with those, because you will use them quite often. Now that we know how and where capture furious children clicks lets handle them!

{% highlight javascript %}
  window.addEventListener("load",function(){
    document.getElementById("clickableDiv").addEventListener("click",function(event){
        console.log(event.target.innerHTML)
    },false)
  },false)
{% endhighlight %}

{% highlight html %}
  <div id="clickableDiv" > Stuff and Things !!! </div>
{% endhighlight %}

Above code will console log whatever clicked div contains. Take a closer look at event.target expression. That field contains reference to clicked element. If click on an button element it'll contain same reference as you would get by document.getElementById. Same for divs, inputs, selects, radio buttons and most of other HTML objects.

So you already know how to get random numbers, create divs and append them to body, you also know how to listen to events and handle them.. So what about small little task same as previously?

Just write short and simple program that will:<br/>
- Generate div <br/>
- Make that div to move into a random corner when clicked <br/>

Hints:<br/>
- You can use position:absolute to determine div's position <br/>
- Current window height is contained in field window.innerHeight and width in window.innerWidth <br/>
- As seen in previous tut you can change elements style by element.style.somethingInCamel = "something"

As promised example solution, this should create marvelous 6 random colored divs:
 {% highlight javascript %}
 var amount = 6
 for(var i = 0; i < amount; i++){
     var element = document.createElement("div")
     element.style.background = "#" + Math.round(Math.random()*255*255*255).toString(16)
     element.style.width = "100px"
     element.style.height = "100px"
     body.appendChild(element)
 }
 {% endhighlight %}
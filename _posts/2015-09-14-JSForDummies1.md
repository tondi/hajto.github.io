---
layout: post
title: "JavaScript (!) for dummies #1"
#status: draft
type: post
published: true
comments: true
---
Every single JS course begins with standard crap like explaining every single operator, which in my opinion might be a little bit pointless due to fact that everyone of us (programmers) usually know basic mathematics like subtraction or multiplication. So what's the point of explaining how "*" operator works? Let's get into to the real meat.

<!--more-->
<h2>#1 Basic random stuff</h2>

{% highlight javascript %}
//Write on document
document.write(stuff)
//Write in console
console.log(stuff)
{% endhighlight %}

Let's start with something really simple. Let's create some divs! Everyone loves divs, they are phreaking everywhere. In order to do that we should use document's createElement method and to make them appear on the actual webpage we need to append them using appendChild method of any existing HTML tag object.

{% highlight javascript %}
var element = document.createElement("div")
//this is how you edit styles
element.style.background = "black"
//px or other unit is extremely important
element.style.width = "100px"
element.style.height = "100px"
document.getElementById("someId").appendChild(element)

//if you want append element to the raw page you can do that by
body.appendChild(element)
{% endhighlight %}

 Easy, isn't it? So let's generate some divs to work with! Wouldn't it be awesome to generate 5 divs with random color? It would be so useful! (no.. it wouldn't) Javascript/HTML uses either RGB or # notation. So we have choice to either generate string with 3 numbers in range 0-255 or 3 numbers in range 0 - FF. If i were you i would pick the first one for now. To generate random numbers in JS we can use it's built in Math.random() function which yields a number from 0 - 1. So in order to have outcome 0-255 we need to multiply it by 255. If you want to begin randomizing from certain number for example 7, just add it after multiplication. Because in worst case when 0 will be generated it will result in number 7 after addition. Simple isn't it?

{% highlight javascript %}
//this will generate random number 0-1
var randomNumber = Math.random()
//number from 0 - 7
var randomUpToSeven = Math.random()*7
//number from 4-7
var fourToSeven = Math.random()*3+4
//because when 0 => 0*3 +4
// and when 1 => 1*3 + 4 => 7 etc.
{% endhighlight %}

  So we want to generate a random color? It's as simply as concatenation some random generated numbers to string!

{% highlight javascript %}
//Math.round() is used to round numbers,
//Math.floor() will round lower, 9.6 will result in 9
//Math.ceil() will round higher, 13.1 will result in 13
function getRandomColorFragment(){
  return Math.round(Math.random()*255)
}
var randomColor = "rgb("+getRandomColorFragment()
  +","+getRandomColorFragment()
  +","+getRandomColorFragment()+")"
//and i got "rgb(118,107,239)"
{% endhighlight %}

  Hmm... We have tiny little element missing.. How the heck we will run something multiple times? Should we just copy and paste? Hell no! Fortunately some very wise person invented thing called loops. Loops are concept that literally loop some amount of code. (executes it multiple times)

{% highlight javascript %}
var someNumber = 55
for(var i = 0; i < someNumber; i++){
 //some code
 console.log(i)
}

//Important warning i should be accesible here after loop finishes
//due to JS weird closures
console.log(i)
{% endhighlight %}

  Now it's only matter of combining two following concepts into one beautifully working masterpiece, but i'll leave that to you. I will give you example solution in next material.

  Some more exercises for you:<br/>
  - Generate random amount of green divs<br/>
  - Generate three rows of dives, each row with random color<br/>
  - Generate two column table. In each row in one column write random number from 1-1000 and in the another one write boolean value whether the neighbour number is prime


 PS: If you read till this point you might appreciate this little snippet:
{% highlight javascript %}
"#" + Math.round(Math.random()*255*255*255).toString(16)
{% endhighlight %}
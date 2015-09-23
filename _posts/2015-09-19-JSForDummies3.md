---
layout: post
title: "JavaScript (!) for dummies #3"
status: released
#draft: false
type: post
published: true
comments: true
---

If you're new to programming you might not be familiar with concept of storing data in arrays. They are tiny little (in this case dynamic) containers that can contain some value mapped to numbers from 0 to n. But in fact they can offer much more functionalities that you might not be aware of. 

<!--more-->

Really short info for new comers:
{% highlight javascript %}
//initialization
var array = []
//assigning values
array[1] = 1
//more dimensions!
array[2] = []
array[2][1] = 1
console.log(array[2]) //will log an array containing 2 items [undefined, 1]
console.log(array[2][1]) //will log 1

{% endhighlight %}

Arrays in JS can be jagged. What does it mean? I means that if you more than one dimensions the sizes don't have to be even. The following code fully legal.

{% highlight javascript linenos %}
var array = [[1],[3,2,1],[2,2]]
array[0] // returns array containing 1
array[1] //returns array containing 3,2 and 1
array[2] //returns array containing 2 and 2
{% endhighlight %}

Obviously you won't access every single element independently. The most convenient way to do it for now is using a for loop that i introduced in [first]{% post_url 2015-09-14-JSForDummies1 %} article.

{% highlight javascript %}
var array = [1,2,3]
//in order to access arrays length we can call
alert(array.length)
//let's put that into the for loop
for(var i = 0; i < array.length; i++ ){
  //do something more useful
  console.log(array[i])
}
{% endhighlight %}

Cool! But what if array have more than dimension? The answer is very simple, more for loops...

{% highlight javascript %}
var array = [[1,2,3],[3,2],[1,2,3,4]]
for(var i = 0; i<array.length;i++)
  for(var j = 0; j< array[i].length; j++)
    console.log("cell "+i+","+j+" contains number "+array[i][j])
{% endhighlight %}

If you'll run the code you'll find out that it prints every single element without concerning whether the dimensions of inside array have changed. It does that because array[1] return another array [3,2] which has it's independent .length field.

Exercises: WIP
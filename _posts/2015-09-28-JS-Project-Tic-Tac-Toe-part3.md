---
layout: post
title: "JS Project #1: Tic Tac Toe p3"
status: draft
draft: true
type: post
published: true
comments: true
---

  I have received many requests about making some guidelines materials about making Tic-Tac-Toe school project on PhoneGap. I won't cover PhoneGap strict part of job because that's actually very easy. The think i will do is breaking this project in smaller accomplishable parts.
   
<!--more-->
 
   <h2> Detecting win! </h2>
   
Let's take it apart. You have 4 possible cases of win and 1 for tie. The tie happens only when number of moves is equal to number of fields (rows * columns). The win conditions are:

- n fields in line horizontally
- n fields in line vertically
- n fields in line across from left to right
- n fields in line across from right to left

How to check if all fields in line are the same? There are few approaches, but we will choose the simplest one, imperative. We will use a loop and mutable variable.

{% highlight javascript %}
function checkHorizontally(numberwearelookingfor, onWin){
  var temp
  for(var i = 0; i < numberOfRows; i++){
    temp = true
    for(var j = 0; j < numberOfCols; j++){
      if(tab[i][j] != numberwearelookingfor){
        temp = false
        break;
      }
    }
    if(temp){
      onWin()
      return true;
    }
  }
  return false;
}
{% endhighlight %}

In above code i'm using De Morgan's laws. If every element is not different then an element X that means all of them are same as element X. You should also note the onWin variable. This code is designed to call onWin when it founds the win so if you want to use it, you have to either pass a function as onWin parameter or comment call. Figuring other i will leave for you.

Some hints:
- when detecting "cross" wins you will need only one loop
- detecting vertical line is similar to horizontal except changing second param of table, you will have to change first (tab[x][y] => tab[y][x])
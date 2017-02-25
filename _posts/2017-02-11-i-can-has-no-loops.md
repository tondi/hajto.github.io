---
layout: post
title: "Elixir is for everyone! I can has no loopz?"
status: draft
type: post
comments: true
list: etut
---

In the previous post I was trying to show you how to live without equality operator. This time I will try to kill yet another unnecessary abstraction, that all of us were addicted to at some point.

<!--more-->

# I can has no loopz?
I was of course talking about loops. One of very first abstractions that made programming actually possible. I am not saying that every single loop is evil, but I would say that when it comes to mutating data I think there might be better ways.

In Elixir you don't have traditional loops. Why?! Because all of the Elixir data is immutable. It would make no sense to constantly increment values. As seen in my previous post you can get away with using recursion, but in most cases it can be done using `Enum` module.

# Map & Reduce
Hadoop MapReduce hasn't taken it's name from thin air. Those two operations are most prominent and well known to functional programmers. The legend says that every operation that transform a collection using loop can be also done using those two functions.

# Map
`Enum.map` is basically a function that takes an enumerable (most of collections, lists, maps etc.) as an argument and takes also a function as second argument and applies that function to every single element of the collection and returns new collection. <!-- Too much collections -->
<table>
<tr>
<td>
{% highlight c %}
for(int i = 0; i < n; i++){
  collection[i] = collection[i] * 2;
}
{% endhighlight %}
</td>
<td>
{% highlight elixir %}
Enum.map(&(&1*2))
{% endhighlight %}
</td>
</tr>
</table>

As you can see Functional way is way more compact.

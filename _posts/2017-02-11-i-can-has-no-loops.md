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
Those two operations are most prominent and well known to functional programmers. The legend says that every operation that transform a collection using loop can be also done using those two functions.

# Map
`Enum.map` is basically a function that takes an enumerable (most of collections, lists, maps etc.) as an argument and takes also a function as second argument and applies that function to every single element of the collection and returns new collection. <!-- Too much collections -->

In Elixir map operation takes two parameters: collection on which it will perform computations and function reference that will be applied on every single element of that collection. Let's assume we want to multiply list's elements by number 2.
<table class="code-samples">
<tr>
<td>
{% highlight c %}
//That's how you would do in imperative language
for(int i = 0; i < n; i++){
  collection[i] = collection[i] * 2;
}
{% endhighlight %}
</td>
<td>
{% highlight elixir %}
#and that's how you would do it in more functional oriented language
Enum.map(collection, fn a-> a*2 end)
#or in shorthand
Enum.map(collection, &(&1*2))
{% endhighlight %}
</td>
</tr>
</table>

As you can see Functional way is way more compact and readable. Yet another plus is that if data is immutable, you'll still have access to previous data state as you can see in following example:

{% highlight elixir %}

list = [1,2,3,4]
changed_list = Enum.map(list, &(&1 *2))
{% endhighlight %}
<a href="http://elixirplayground.com?gist=c0bc8b7c45f96add2a772181e69f6f12"> Try for yourself </a>

# Reduction
It is yet another important operation. It allows you to take a bunch of things and turn into one totally brand new thing. And most common task we all have been through on the beggining of our IT career was performing a summation. So let's just assume that we have to implement such thing right now.

<table class="code-samples">
<tr>
<td>
{% highlight c %}
double sum = 0;
for(int i = 0; i < n; i++){
  sum += collection[i];
}
{% endhighlight %}
</td>
<td>
{% highlight elixir %}
Enum.reduce(list, 0, fn element, acc -> element+acc end)
#or
Enum.reduce(list, 0,&(&1 + &2))
{% endhighlight %}
</td>
</tr>
</table>

As you might have guessed so far, that function you pass must take two arguments: current element and accumulator. On the first slot function will receive an element from the list and second element is an accumulator. Accumulator is just value that was returned in previous iteration thus function must return a single value. And that's exactly why we supplied that 0 in second slot. It will be treated as result of iteration prior to first. If you would look at docs you would realize that's there is also `reduce/2`. It doesn't take an initial value and will treat first element of collection as first accumulator.

{% highlight elixir %}
list = [1,2,3,4]
result = Enum.reduce(list, 0, fn element, acc -> element+acc end)
another_result = Enum.reduce([0 | list], fn element, acc -> element+acc end)
{% endhighlight %}
<a href="http://elixirplayground.com?gist=41f708d059bed6a63d71178c3c85421a"> Try for yourself </a>

# Matrix multiplication

Let's take a little bit harder example. Matrix multiplication is very is to implement when we have random access to elements, but as you may noticed we have been only accessing elements sequentially. If we would do this in C we would just take two matrices multiply corresponding elements (row X column, first element in row times first element in the collumn and so forth and so on). And in functional languages that's not very efficient. We technically could operate on parallel rows by `zipping` two rows together. If you recall your math lessons you probably know operation called `matrix transposition`, which is basically swapping rows and columns.

```
Sample transposition

Original matrix
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]


Transposed
[1, 4, 7]
[2, 5, 8]
[3, 6, 9]

```

Remember the `zip` I mentioned? It will prove pretty handy here. Matrix is basically lists of lists. Zipping takes nth elements of m lists and group those element into a single tuple which can be conveniently converted into list... Do you see where this is going to?

{% highlight elixir %}
List.zip(matrix)
|> Enum.map(&Tuple.to_list(&1))
{% endhighlight %}
<a href="http://elixirplayground.com?gist=14b217714dfdd9c77873af0ad7e109e4"> Try for yourself </a>

Knowing that rest is pretty easy. You have two matrices. First one you'll have to transpose, then zip corresponding rows and reduce them multiplying elements of zipped tuples and summing them. Consider this your homework. Here is an <a href="http://elixirplayground.com?gist=7d0fda1cdec8be55ef761e974f620024"> example solution </a> and sample implementation in one of the elixir existing <a href="https://github.com/a115/exmatrix/blob/master/lib/exmatrix.ex#L58"> libraries </a>.

# Why does this matter?

If you're coming from an imperative ecosystem you probably noticed that this code may be slower than it's imperative counterpart. It has a lot of function calls, a bunch of map and reduce operations... But there is a catch! A big one actually. Most of operations we've done depend on map, zip and reduce implementation. And map can be very easily made parallel! Thus your code can be run simultaneously on all of the cores without any headache (mostly). Actually author of library I mentioned earlier implemented this algorithm in <a href="https://github.com/a115/exmatrix/blob/master/lib/exmatrix.ex#L72" > parallel </a>.

---
layout: post
title: "Elixir's essence"
status: draft
type: post
published: released
draft: true
comments: true
---

I was trying to present basic concepts of Elixir to my friends, but each time they would rather stay with their boring imperative languages.. They usually find it overcomplicated. I think that's time to change that.

<!--more-->

Most of commonly used languages revolve around two basic operations: assigment and looping. I think that people usually get so confused by Erlang/Elixir because those languages actually don't have any of those tools available for people to use. In fact, they have much greater toys.

## Patern Matching

Someone probably would say that Elixir has `=` operator and he would be right. But this operator in Elixir is much more powerfull. It does not simply assign value to a variable. It's more like mathematical `=`. When you solve a mathematical equation you don't assign value to X, you look for X that matches a pattern. Same thing applies for Elixir.

Let's consider a pair of x and y. You know that that pair is equal to (1,2). So you know that x is 1 and y is 2. Elixir can do pretty much same thing. Similarly if you have pair (x,2) and it should be equal to (1,3) you know that their is no such element that substituted for x would match the pattern! So pattern matching (that's how `=` is actually called) must fail!

{% highlight elixir %}
{x,y} = {1,2}
{% endhighlight %}

## Functions! Functions everywhere..

So now we know what pattern matching kinda is and we know it can fail. So what is the fuss all about? We can pattern match argument of functions actually. Given definition of factorial we can implement it in seconds! We know that:

>factorial of 0 is 1<br/>
>factorial of 1 is 1 <br/>
>factorial for x is factorial(x-1)*x for x bigger than 1

So we can to that:

{% highlight elixir %}
def factorial(0), do: 1
def factorial(1), do: 1
def factorial(x) when x > 1, do: factorial(x-1)*x
{% endhighlight %}

What have we done? We just written factorial in Elixir by simply writing it's definition... Let's go step by step through that code, because there are few structures that may be new for you. You can define functions in Elixir in at least two ways.

{% highlight elixir %}
def name(pattern), do: what_to_return
def name(pattern) do
  computations
end
{% endhighlight %}

Remember when I said that pattern matching is awesome and it can fail? You can do it in function clauses! Function will only execute when pattern matching on it's arguments is successful. So if the argument value is equal 0 it will return 1, if the value is equal to 1 it will return 1 and finally if the value is equal to x which is unbound so it matches everything it will return x times factorial(x-1). But what's doing that little `when` there? It's guarding the domain of the function and it will only let values greater than one in. That's why `when` is called guard.


As you probably realised the last expression in block is the value to be returned. Let's go back to our pair example and tinker a bit with it.

{% highlight elixir %}
def first({x,_}), do: x
def second({_x,y}), do: y
def friend_of_2({x,2}), do: x

{% endhighlight %}

Wait.. What? Why does it work? There is a new keyword that made it possible (or at least pretty). `_` is used when you don't give a damn about what is being passed as an argument. If you want for some reason to name an argument of function and never plan to use it, please prefix it with `_`, for example `_x`.

{% highlight elixir %}

first({1,2}) #Will return 1
second({1,2}) #Will return 2
friend_of_2({1,2}) #Will return 1
friend_of_2({2,3}) #Will return an error saying that there is no such function, because it doesnt match the pattern
{% endhighlight %}

Given these tools you can achieve most of the things much faster and express your thoughts more naturally.

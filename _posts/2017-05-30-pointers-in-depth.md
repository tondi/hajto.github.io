---
layout: post
title: "Pointers in C in depth"
status: draft
type: post
comments: true
list: etut
set: jipp
---

//Introduction here

<!--more-->

Pointers are used in few different ways. They can be used as reference to an array of elements or they can be used as reference to a variable so you can change it and keep the changes.

# Let's start it gently
Pointers can be understood as an address to a variable. They are much powerful though. You can do quite a lot of math with them too. If you ever actually read a tutorial for `Hello World` in C you probably already encountered them. `scanf` function takes some pointers as it's 2nd-nth argument.

{% highlight c %}

int some = 0;
scanf("%d", &some);

{% endhighlight %}

What `&` expression does is it gives you `pointer` to place where your variable value is held in memory. If you have only pointer to variable and want the value back you can use `*` operator. You can also use `*` for changing value of variable you have pointer to.

{% highlight c %}

int some = 0;
int* pointer = &some; //Pointer to some

some = 3;

printf("Value: %d",some);
printf("Value from pointer: %d", *pointer);

*pointer = 4;

printf("Value: %d",some);
printf("Value from pointer: %d", *pointer);

{% endhighlight %}

Some of you might be wandering right now... I used `*` in two totally different ways. And only mentioned one meaning.. If you put `*` AFTER `type` name such as `int` it will mean that you have (`int*`) pointer to integer, which is type of it's own. You can even have such weird types as `int***` which reads as pointer to pointer to pointer to integer. If you place it prior to variable name (`*variable`) it will mean that you want to pick the value from pointer.

# Pointers as reference to arrays

Some of you may actually now that in C arrays variables are actually reference to their first elements. Even those that are statically allocated.

{% highlight c %}

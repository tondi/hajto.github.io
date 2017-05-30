---
layout: post
title: "Extensive guide for Structures"
status: released
type: post
comments: true
list: etut
set: jipp
---

For some reason a lot of people in my Group at Uni have quite a lot of problems with understanding concept and usage of Structs (structures) in C.

<!--more-->

# What structures are?!

They are basically compound types defined by the programmers. Imagine you want to model a Dog. Any dog has a name, an age and maybe favorite food. You would have to declare at least three variables to describe such a Dog for example in such a way:

{% highlight c %}
char name[50];
int age;
char favoriteFood[40];
{% endhighlight %}

What if you want to have more dogs?! Create multiple arrays and keep them in Sync? Even thinking about such solution is considered madness. (At least in my books) That's exactly why structures were introduced into programming. They just couple some values together and allow you to access each of them individually. They are usually defined in headers. Their definition is pretty straight forward.

{% highlight c %}
struct STRUCT{
  type1 name1;
  type2 name2;
};

{% endhighlight %}

So it's just few variables under one name? Exactly.. Knowing that we can easily declare our first struct. A dog.

{% highlight c %}
struct Dog{
  char name[50];
  int age;
  char favoriteFood[40];
};
{% endhighlight %}

I am using fixed size arrays just for convenience. Using pointers as fields may introduce some problems which I will try to cover a bit later.

# Our first Struct

So we finally declared our first Struct. But that's only description. How do we actually create a usable Dog?

{% highlight c%}
//Declaration of variable of type Dog
Dog dog;
//Setting example field values
strcpy_s(dog.name, sizeof(dog.name), "Burek");
strcpy_s(dog.favoriteFood, sizeof(dog.favoriteFood), "Sausage");
dog.age = 11;

//Accessing Stored data
int tempAgeOfACertainDog = dog.age;
printf("%s", dog.name);
{% endhighlight %}

We can also assign structures to each other.

{% highlight c %}
Dog woofer = dog;
{% endhighlight %}

And if we print each value individually we will see that corresponding values were indeed copied. But there is also a tiny little problem. C compiler is actually transpiling `=` into shallow memory copy. You can read more about it <a href="http://blog.zhangliaoyuan.com/blog/2013/01/28/structure-assignment-and-its-pitfall-in-C-language">here</a>. It will be all fine unless you use a Structure or Pointer as a field.

{% highlight c %}
struct WITH_POINTER {
  char* pointer;
}

//Let's define few instances
WITH_POINTER a,b;

//Assign a String to field of structure a
a.pointer = one_string;
b = a; //And assign one structure to another

//Let's change a character in a String which is a field in a structure
b.pointer[3] = 'a';
{% endhighlight %}

The problem is that when you change `b.pointer` the `a.pointer` will also change... That's why you should manually copy pointers that are members of structs to prevent that.

{% highlight c %}
b=a; //If you have another fields that are non pointers
b.pointer = allocate_something();
strcpy(b.pointer, a.pointer);
{% endhighlight %}

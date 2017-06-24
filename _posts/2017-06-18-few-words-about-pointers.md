---
layout: post
title: "Few words about pointesrs"
status: released
type: post
comments: true
list: etut
set: jipp
---

Pointers are used in few different ways. They can be used as reference to an array of elements or they can be used as reference to a variable so you can change it and keep the changes.

<!--more-->

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
#define MAX 10

int fewNumbers[MAX] = {0};

for(int i=0; i < MAX; i++){
  fewNumbers[i] = i;
}

int* first = fewNumbers; //reference to first element
int* last = fewNumbers+(MAX-1); //length - 1 = 9, first + 9 will yield same result
                                //thus first+9 should yield pointer to last element

int length = last - first;
printf("Length: %d\n", length+1);

int* temp = first;
while(temp != last+1){
  printf("Index: %d, value: %d\n", temp-first,*(temp));
  temp++;
}

{% endhighlight %}

Quite a lot new things there, huh? You are probably already familiar with `[]` notation, but the way I printed array elements may be new for you. As I said I said earlier, pointers can be understood as addresses. Addresses can be added and subtracted. IF you want to visit next cell you can take current address and add 1 to it. (as I did by `temp++`) Similarly if you want to know how many houses is on a street you can take address of last and subtract address of first one. (that's how variable length was calculated)

Knowing that we could actually implement our own access function (same as `[]`) as follows.
{% highlight c %}

int access_int(int* array, int index){
  return *(array+index);
}

{% endhighlight %}

And lets test it out!

{% highlight c %}
#define MAX 10

int fewNumbers[MAX] = {0};

for(int i=0; i < MAX; i++){
  fewNumbers[i] = i;
}

for(int i=0; i < MAX; i++){
  printf("[]: %d", fewNumbers[i]);
  printf("[]: %d", access_int(fewNumbers,i));
}


{% endhighlight %}

<a href="http://rextester.com/LHFV29063"> Live demo </a>

# Pointers as reference to functions

Let's say you have to write a program that compares different implementation of same algorithm and measure it's performance. You have come to point when your `main` function is result of copiyng and pasting same code over and over and only one line changes. Situations like this always tingle my `don't repeat yurself nerve`.

What would you do in such situation if that wasn't case of functions. What if that was simple that. You would probably put into array and iterate over with some kind of loop. What if I told you that you can actually do that.

{% highlight c%}
deftype uintmax_t(*FibonacciImplementation)(int n);
{% endhighlight %}

Let's say you have to implement program that compares different implementation of fibanacci algorithm. All of them have same signature. They take `int` and return large number that probably will exceed `int` range. And that's what excactly that `deltype` statement mean. It defines alias to `function pointer`.

{% highlight c %}
    FibonacciImplementation testy[] = { fiboCondensed,fiboZwykly };

	float diff[20][2];

	int b;
	for (int i = 0; i < 2; i++)
	{
        b = 0;
		for (int n = 0; n < 600; n += 30)
		{

			clock_t t1, t2;
			t1 = clock();
			testy[i](n);
			t2 = clock();
			diff[i][b] = ((float)(t2 - t1) / CLOCKS_PER_SEC) * 1000;
			b++;
		}
	}
{% endhighlight %}
Check the <a href="http://rextester.com/EYLX37632">demo</a>

Unfortunately this topic is far too broad to fit it here. I think it deserves post of it's own, but you should have a general idea of how they work after reading this.

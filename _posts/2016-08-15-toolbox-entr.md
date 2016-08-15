---
layout: post
title: "Toolbox: Entr, the tool you wished you know before"
status: released
type: post
published: true
comments: true
draft: true
---

Every programmer needs tools to achieve his goals. Most of developers just do everything manually, but that's not the best idea, because we (developers) should strive to be lazy. And that's where this comes in. This tool will launch command or script whenever a passed file has changed.

<!--more-->

That's all that it does. It's easy to install and it gets the job done. You can download from <a href="http://entrproject.org/">here</a>. Proceed with uber easy installation and you're ready to go.

# Installation

Installation is as simple as extracting archive downloaded from website and calling

{% highlight bash %}

make test       #it will perform tests, make sure every passed
make install    #may require sudo

{% endhighlight  %}

# How it works?

Entr is basically a function that takes list of files as first param and action as second.

Let's pretend that we have hypothetical Scala project in Akka HTTP that serves data for Authentication Service of sort.

So we can call entr being in main project's directory like that:

{% highlight bash %}

find src -type f | entr echo "It works goddamit"

{% endhighlight %}

It will run watcher that will print that text. That's ain't practical isn't it? To put it into more practical work we can do something like this:

{% highlight bash %}

find src -type f | entr sbt test

{% endhighlight %}

Whoa! Now every time you'll save change the test will run! (Memory RIP) The good news is that it will w8 till last call is finished before starting new one ;-)


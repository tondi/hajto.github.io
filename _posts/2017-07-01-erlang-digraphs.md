---
layout: post
title: "Erlang Digraph"
status: draft
type: post
comments: true
list: etut
set: erlang
---

Erlang has tons of goodies that most of people don't know about. If you are studying IT just like me you should definitely learn `digraph`, because you will have to implement a graph algorithm at some point and this can be considered real lifehack in such situations.

<!--more-->

As you might have already guessed by the name it is a data structure that is used to store directed graphs in Erlang or Elixir. It is really easy to use. First thing you have to do is to actually initialize a graph.

{% highlight elixir%}
iex(1)> graph = :digraph.new()
{:digraph, #Reference<0.3992624098.55705602.46770>,
 #Reference<0.3992624098.55705602.46771>,
 #Reference<0.3992624098.55705602.46772>, true}
{% endhighlight %}

If you ran it in REPL you should get five element tupple with three `#Reference<Something>` thingies. Don't bother with what's inside just for now.

After graph is successfuly initialized you can add vertices and edges.

{% highlight elixir%}
iex(2)> london = :digraph.add_vertex(graph, "London")
"London"
iex(3)> ny = :digraph.add_vertex(graph, "New York")
"New York"
iex(4)> sf = :digraph.add_vertex(graph, "San Francisco")
"San Francisco"
iex(5)> warsaw = :digraph.add_vertex(graph, "Warszawa")
"Warszawa"
iex(6)>
nil
iex(7)> :digraph.add_edge(graph, london, ny)
[:"$e" | 0]
iex(8)> :digraph.add_edge(graph, ny, sf)
[:"$e" | 1]
iex(9)> :digraph.add_edge(graph, warsaw, london)
[:"$e" | 2]
iex(10)> :digraph.add_edge(graph, london, sf)
[:"$e" | 3]
{% endhighlight %}

Now you can for example find shortest road between two vertices.
{% highlight elixir %}
iex(13)> :digraph.get_path(graph,warsaw, sf)
["Warszawa", "London", "San Francisco"]
{% endhighlight %}

# Nitty gritty details

Rembember those three values I told you not to care about? It's time to take a closer look at them.

{% highlight elixir %}
iex(14)> graph
{:digraph, #Reference<0.3992624098.55705602.46770>,
 #Reference<0.3992624098.55705602.46771>,
 #Reference<0.3992624098.55705602.46772>, true}
iex(15)>
{% endhighlight %}

First thing we should do to find enlightment is to look up how it is actually implemented in Erlang. In the `digraph` module <a href="https://github.com/erlang/otp/blob/master/lib/stdlib/src/digraph.erl#L42">source</a> we can find following lines:

{% highlight elixir %}
-record(digraph, {vtab = notable :: ets:tab(),
		  etab = notable :: ets:tab(),
		  ntab = notable :: ets:tab(),
	      cyclic = true  :: boolean()}).
{% endhighlight %}

It is quite similar to our tupple? If record is similar to struct where did the names go? Actually.. In Erlang records are bare tupples. The names exist only in compile time, after that they are gone. It is quite troublesome approach, but it clarifies a lot of what we can see right now. `vtab` is first element, `etab` is second and so worth and so on.

You might also have realized that all of the operations we performed previously were impure. That's because `digraph` is internally represented as set of three `ets` tables and those weird `#Reference` are actually results of creating new unnamed `Erlang Term Storage` table.

What's even more important we can actually see how it is internally represented! We can just call `:ets.tab2list/1` and see how digraph manages it data.

{% highlight elixir %}
iex(15)> {_, vertices, edges, neighbours,cyclic} = graph
{:digraph, #Reference<0.3992624098.55705602.46770>,
 #Reference<0.3992624098.55705602.46771>,
 #Reference<0.3992624098.55705602.46772>, true}

iex(16)> :ets.tab2list(vertices)
[{"London", []}, {"Warszawa", []}, {"New York", []}, {"San Francisco", []}]
iex(17)> :ets.tab2list(edges)
[{[:"$e" | 0], "London", "New York", []},
 {[:"$e" | 3], "London", "San Francisco", []},
 {[:"$e" | 2], "Warszawa", "London", []},
 {[:"$e" | 1], "New York", "San Francisco", []}]

iex(18)> :ets.tab2list(neighbours)
[{{:out, "Warszawa"}, [:"$e" | 4]}, {:"$eid", 5}, {:"$vid", 0},
 {{:in, "Warszawa"}, [:"$e" | 2]}, {{:out, "London"}, [:"$e" | 0]},
 {{:out, "London"}, [:"$e" | 2]}, {{:out, "London"}, [:"$e" | 3]},
 {{:out, "New York"}, [:"$e" | 1]}, {{:in, "London"}, [:"$e" | 4]},
 {{:in, "San Francisco"}, [:"$e" | 1]}, {{:in, "San Francisco"}, [:"$e" | 3]},
 {{:in, "New York"}, [:"$e" | 0]}]
{% endhighlight %}

We can see a lot of interesting information here. As we can see edges are actually numbered. Even more interesting is neighbours table which kind of resembles math notation of Neighbour relation.

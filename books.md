---
layout: page
title: Library
permalink: /books/
---

One of the best ways to learn is to read some books. Here is list of my favorite positions that taught me programmingg and more.

<div class="posts">
  {% for post in site.posts %}
    {% if post.status == 'hidden' and post.set == 'books' %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a> </h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
    {% endif %}
    {% endfor %}
</div>

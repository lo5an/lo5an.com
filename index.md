---
layout: home
title: lo5an.com
---



<div class="container-fluid">
  <div class="row-fluid">
    <div class="span8">

<section>
<h1> Recent Posts</h1>
  <ul class="posts">
    {% for post in site.posts %}
       <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</section>

    </div>
  </div>
</div> <!-- /container -->


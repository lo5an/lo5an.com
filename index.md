---
layout: home
title: lo5an.com
---
<div class="container-fluid">
  <div class="row-fluid">
    <div class="span8">


      <h1>Logan Cox</h1>
      <p> Web geek, word nerd, office supply enthusiast.</p>

    <h3>Recently on my mind:</h3>
    <ul class="posts">
      {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endfor %}
    </ul>


    </div>
  </div>
</div> <!-- /container -->


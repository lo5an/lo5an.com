---
layout: home
title: lo5an.com
---

<header class="site-home container-fluid">
<h1 class="light">Logan Cox</h1> 
<p>web geek, word nerd, office supply enthusiast</p>
</header>

<div class="container-fluid">
  <div class="row-fluid">
    <div class="offset2 span8">




<section>
<h1>{{site.posts.first.title}}</h1>

{{site.posts.first.content}}

</section>




<section>
<h1> Other Posts</h1>
  <ul class="posts">
    {% for post in site.posts offset:1 %}
       <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</section>



    </div>
  </div>
</div> <!-- /container -->


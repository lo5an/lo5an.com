---
layout: home
title: Barbarous Names
tagline: Supporting tagline
---
{% include JB/setup %}
    
## Posts

<div class="posts">
  {% for post in site.posts %}
    <section>
		<h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>
		{{ post.content }}
    </section>
  {% endfor %}
</div>





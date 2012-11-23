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
<h1>Most Recent Stuff</h1>

<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc id lorem ligula, non gravida dui. Praesent vel scelerisque justo. Praesent rhoncus pulvinar fringilla. Sed consectetur laoreet arcu, commodo rhoncus ante varius et. Mauris non magna quis massa convallis convallis. Vivamus ultricies, arcu eu faucibus eleifend, neque mauris pulvinar elit, ut semper libero neque id est. Mauris vitae posuere elit. Integer turpis lacus, ornare id volutpat a, faucibus lacinia tellus. Cras mattis, leo nec pulvinar molestie, diam libero semper erat, et vestibulum tortor massa at magna. Proin rhoncus, ligula vitae auctor dapibus, nulla quam congue justo, eu dapibus purus velit ac enim. Sed eu sapien felis, non commodo ligula. 
</p>

</section>




<section>
<h1> Other Posts</h1>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</section>



    </div>
  </div>
</div> <!-- /container -->


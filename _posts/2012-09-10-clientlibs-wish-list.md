---
layout: post
tags : [CQ]
title: ClientLibs Wish List
---
{% include JB/setup %}

I've been working on some projects where we're trying to use CQ's client library stuff to manage JavaScript and CSS. We've ruled out using the client side library loader provided by `<cq:includeClientLib>` becaue it just doesn't provide the level of control that we want, but we've found the server side part to be a decent way to organize things. 

That said, I've definitely got a new-features wish list:


## Origin Path Labels 

Once you've concatenated a bunch of CSS or JS together into a single file, it sometimes gets to be a pain to figure out where you need to go to make changes.  I've started adding a comment showing the path to the file at the top of each file in any of my client library folders. This is working fine, but it'd be even better if CQ did it out of the box. 


## Recursive Embeding 

I created a client library folder for each of my included jQuery libraries, and then I created a `jquery.all` client library folder that embedded them. My plan was to use that to embed jQuery and all of the plugins that we're using. It didn't quite work as expected, though. If A embeds B, and B embeds C, then A doesn't end up with C as well. Drag.  


## Post and Pre Processing Filters 

This is my Big Ask. Less is built in to CQ 5.5, and that's pretty nice, but I would really like to have a generic way to define post and pre-processing filters on a per-file-type basis. So, basically, "run all .css files through filter X before concatenating them together" and "run all .js files through command Y before concatenating them together"







 
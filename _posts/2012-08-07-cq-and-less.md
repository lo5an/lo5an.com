---
layout: post
title: CQ 5.5 has LESS built in 
tags : [CQ, css]
---
{% include JB/setup %}


I was poking around the CQ 5.5 docs and I found something cool in the [release notes](http://dev.day.com/docs/en/cq/current/release_notes/overview.html) that I'd missed, 

>ClientLibs have built-in support for LESS http://lesscss.org/ to simply CSS development
	
This is exciting because we've been working a lot with [Twitter Bootstrap](http://twitter.github.com/bootstrap/) in my office and we've actually got this feature on our development TODO list.  

I tried it out by adding ```test.less``` to an existing client library folder and including it via ```css.txt```.  CQ dutifully turned 

    @color: #4D926F;
    #header {			
        color: @color;		
    }					
    h2 {				
	    color: @color;		
	}					


in to CSS

    #header {
       color: #4d926f;
    }
    h2 {
      color: #4d926f;
    }

As you would expect, files using LESS syntax needed to be named with the ```.less``` extension to let CQ know to handle them.

## Bummer: less-1.1.6.js

The one downside, so far, is that the bundled version of LESS seems to be 1.1.6 (based on some errors that I'm seeing in my logs). That's a little too old for my purposes and there doesn't seem to be an obvious way to upgrade to a newer version in a clean way.



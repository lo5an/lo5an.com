---
layout: post
title: My WCMMode Scripts 
tags : [CQ, js]
---
{% include JB/setup %}


I've written a couple of scripts to make WCMMODE a little more friendly. First, Here's a simple bookmarklet that toggles between the content finder with no params and the normal url with `wcmmode=DISABLED` ( [Toggle CF][1] ). 

The source is below. At some point, I should probably  make it a little smarter about the query part of the url. I figure I'll do that when I end up needing it.  

    javascript:
    (function(){
      if("/cf"===location.pathname){
        location.href="//"+location.host+location.hash.replace("#","")+"?wcmmode=DISABLED";
      } else {
        location.href="//"+location.host+"/cf#"+location.pathname;
      }
    })()

To go along with that, I wrote a Greasemonkey (I use [Scriptish](http://scriptish.org/), actually) script that rewrites all CMS-local links to include the `?wcmmode=DISABLED` query param if it was present in the url for the page. You can [get that](http://userscripts.org/scripts/show/149242) at [userscripts.org](http://userscripts.org).

[1]:javascript:(function(){if("/cf"===location.pathname){location.href="//"+location.host+location.hash.replace("#","")+"?wcmmode=DISABLED";}else{location.href="//"+location.host+"/cf#"+location.pathname;}})()




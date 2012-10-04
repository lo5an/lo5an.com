---
layout: post
title: Stupid CSS Tricks 
tags : [CQ, css]
---
{% include JB/setup %}

I was playing with some CSS today and it turns out that this works:

    head,style,script { 
      display:block;
      font-family: monospace;
      white-space: pre;
    }

Not sure when this would actually come up, but I thought it was interesting. 

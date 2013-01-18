---
layout: post
title: less Has a Follow Mode
tags : [unix]
---

I often find myself looking at log files and switching back and forth between `less` to page through and search for old events and `tail -f` to watch for new ones.

I got annoyed that `less` couldn't just do it all, so I took another look in the man page where I found:


    F     Scroll forward, and keep trying to read when the end of file is
          reached.  Normally this command would be used when already at
          the end of the file.  It is a way to monitor the tail of a file
          which is growing while it is being viewed.  (The behavior is
          similar to the "tail -f" command.)
		  
I'm not sure when that got added. Maybe it's always been there and I never noticed it. In any case, it reminded me of my dad's long ago admonition that it's always good to review the basics every so often. Because sometimes they've changed, and sometimes you have. 




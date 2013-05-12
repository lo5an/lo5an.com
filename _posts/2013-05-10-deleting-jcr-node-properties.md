---
layout: post
title: Deleting JCR Node Properties
tags : [jcr]
---
Today, I needed to remove a property from a JCR node. Somewhat surprising, I haven't actually had to do that before.

First, I checked out the Node API for `delProperty(key)`, or something along those lines. Nothing. So I thought maybe I'd be able to get the result that I wanted by setting the property value to null.

I tried `myNode.setProperty("foo", null);` and it didn't work. I got a Runtime Exception with the message

    The method setProperty(String, Value) is ambiguous for the type Node

Looking closer at the API docs set me straight

> Passing a null as the second parameter removes the property. It is 
> equivalent to calling remove on the Property object itself. For example, 
> `N.setProperty("P", (Value)null)` would remove property called "P" of the 
> node in N.
	 
Which, upon reflection, makes sense. There are several different versions of `setProperty()` for storing different kinds of values and Java can't figure out which method signature applies when it gets `null` for the value argument.

I added in an explicit cast and everything worked fine. Upon reflection, though,  I might end up using the `remove()` version next time. I think `myNode.getProperty("foo").remove();` might be a bit more transparent for readers.  







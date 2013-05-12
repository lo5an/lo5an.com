---
layout: post
tags : [CQ]
title: Remember to Use .adaptTo()
---

I keep kicking myself for not learning the lesson. I'm writing this down primarily in hopes that I'll remember it BEFORE I start peppering my code with null checks and try blocks. 

I keep using code like 

```
String path = currentNode.getProperty("path"); 
```

which throws an exception if `path` does not exist. I tend to do it without thinking because `Resource` doesn't have a `getProperty()` method or equivalent. But `Resource` does have `adaptTo` and you can do 

```
ValueMap resProps = resource.adaptTo(ValueMap.class) 
```

and get a ValueMap of the properties of the resource. After which you can call

```
String path = resProps.get("path", ""); 
```

which is much nicer. 


The adaptTo() method is generally awesome and can be used for a ton of stuff. I'm not sure why I keep forgetting about it. 

See the CQ docs on [sling adapters](http://dev.day.com/docs/en/cq/current/developing/sling-adapters.html) for details.

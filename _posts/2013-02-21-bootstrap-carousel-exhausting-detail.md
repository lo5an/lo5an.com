---
layout: post
title: Bootstrap Carousel in Exhausting Detail (part 1)
tags:  [bootstrap, js, jQuery]
---

I've been trying to level up my JavaScript chops lately. At the same time, we've been using [Twitter Bootstrap](http://twitter.github.com/bootstrap/) quite a bit at work and I'm likely to be writing a jQuery extension or two in the near future. As a consequence, I decided to do a close reading of some of the Bootstrap JavaScript source. 

This writeup covers the traditional jQuery stuff. In part 2, I plan to eventual circle back and write up the Carousel Data API. 

Here Goes:

## Function Wrapper 

    !function ($) {
    ...
    }(window.jQuery);
	
The major motivation for wrapping our plugin in a function expression like this is to make sure that we're in an execution context where the `$` variable in our code references the `window.jQuery` object that we're expecting. This closure is standard for jQuery plugins, and it occurs to me that it's a pretty good example for explaining the what/why of closures to a beginner. 
    
I'm used to seeing `(function(){ ... })()`, but Bootstrap uses `!function(){ ... }()`. In each case, an anonymous function expression is defined and then immediately invoked. Grouping `()` or negation `!` is used to force expression context.  Without some operator to change the context, we'd have a function declaration, not a function expression, and we'd get a syntax error when JavaScript tries to invoke the function. See [this Ben Alman post](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) for a good rundown on immediately-invoked function expressions.

I think I'm going to start using `!` instead of `()`. I like that you don't need to worry about matching up an extra set of parens.

## Strict Mode
    
    "use strict"; // jshint ;_;
       
Strict mode is an ECMAScript 5+ feature that tightens up some of the looseness in JavaScript. See [this John Resig post](http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/) for more info. Calling it at the top of the wrapper function turns it on for all of the code inside the function.

[JSHint](http://www.jshint.com/) (like [JSLint](http://www.jslint.com/), its grumpy older brother), is a code analysis tool for JavaScript that can save you from a variety of mistakes. Bootstrap uses it, and one of the things it suggests is strict mode. 

## Carousel Object Constructor
    
    var Carousel = function (element, options) {
        this.$element = $(element)
        this.options = options
        this.options.slide && this.slide(this.options.slide)
        this.options.pause == 'hover' && this.$element
            .on('mouseenter', $.proxy(this.pause, this))
            .on('mouseleave', $.proxy(this.cycle, this))
    }
	
This is the Carousel constructor, called later on in the jQuery plugin definition.

It relies on JavaScript's automatic semicolon insertion. It's pretty common in the JavaScript world, but it makes my teeth hurt a little. No return value is explicitly specified because this is a constructor. When the function is called with the `new` keyword, `this` refers to a newly created object that will be returned.

`this.$element = $(element)` gives us a jQuery object for the element we passed to the constructor. Prefixing variable names with `$` to indicate that they are jQuery objects is a common idiom that the Bootstrap code follows. (Oppa Hungarian Style!)

`this.options.slide && this.slide(this.options.slide)` relies on lazy evaluation of the JavaScript conjuction so that `this.slide(this.options.slide)` only gets called if `this.soption.slide` exists and is truthy. This is a pretty common null-checking idiom in JavaScript.

The constructor also wires up the pause behavior of the Carousel. `$.proxy(this.pause, this` and  `$.proxy(this.cycle, this)` make sure that the `pause` and `cycle` get called in the context of the particular Carousel created by the constructor (`this`). See the jQuery docs for [$.proxy()](http://api.jquery.com/jQuery.proxy/).


## The Carousel Object Prototype

Moving on the Carousel object prototype, we see the functions that define it's behavior. 

    Carousel.prototype = {
      cycle: function (e) {...}
      , to: function (pos) {...}
      , pause: function (e) {...}
	  ...
    }

Nothing special here, just object literal notation and the day-to-day weird that is prototype inheritance. 
    
    cycle: function (e) {
        if (!e) this.paused = false
        this.options.interval
          && !this.paused
          && (this.interval = setInterval($.proxy(this.next, this), this.options.interval))
        return this
    }
	
This checks and/or updates the paused state of the Carousel, then starts up cycling through slides by using `setInterval` to repeatedly invoke the `next` function of a Carousel object every `interval` milliseconds. `setInterval` returns a unique interval ID that will be used elsewhere to stop the Carousel from cycling. `e`, if defined, will be an event object. 
    
    , to: function (pos) {
        var $active = this.$element.find('.item.active')
			, children = $active.parent().children()
			, activePos = children.index($active)
			, that = this

        if (pos > (children.length - 1) || pos < 0) return

        if (this.sliding) {
            return this.$element.one('slid', function () {
                that.to(pos)
            })
        }

        if (activePos == pos) {
            return this.pause().cycle()
        }
        return this.slide(pos > activePos ? 'next' : 'prev', $(children[pos]))
    }
	
This function scrolls to a specific slide in the Carousel. It starts with some variable definitions and sanity checking. `$active.parent().children()` returns the list of slides in the carousel, and `children.index($active)` figures out which slide is currently active and visible. 

If the carousel is currently sliding to a new slide, it uses the `one` function to set up a one-off event handler to re-call  `to (pos)`  once a new slide is active and the `slid` event is called.

If the carousel is currently on the slide that it would be directed to, we use `this.pause().cycle()` to reset the interval timer so that we get a full interval at the current slide before moving on.

Otherwise, we use `slide` to move directly forward or back to a particular slide.
    
    , pause: function (e) {
        if (!e) this.paused = true
        if (this.$element.find('.next, .prev').length && $.support.transition.end) {
            this.$element.trigger($.support.transition.end)
            this.cycle()
        }
        clearInterval(this.interval)
        this.interval = null
        return this
    }
    
This function stops the Carousel. `clearInterval(this.interval)` is where the business happens.   
    
    , next: function () {
        if (this.sliding) return
        return this.slide('next')
    }
	
	prev: function () {
        if (this.sliding) return
        return this.slide('prev')
    }

A couple of wrapper functions for moving backwards and forwards. They'll get used to make it easier to set up controls. The heavy lifting is done by the next function. 

### The `slide` function

This handles the work of making the next slide active, moving either forward or backwards as required. A lot happens, possibly too much different stuff.  If we're currently cycling, we pause that, fire off a `slide` event, set the next slide active and deactivate the current one (with visual transitions, if supported), fire off a `slid` event, and then start cycling again. If we're not cycling, then we just do the event-activate/deactivate-event part. 

    , slide: function (type, next) {
        var $active = this.$element.find('.item.active')
          , $next = next || $active[type]()
          , isCycling = this.interval
          , direction = type == 'next' ? 'left' : 'right'
          , fallback  = type == 'next' ? 'first' : 'last'
          , that = this
          , e
  
        this.sliding = true
  
        isCycling && this.pause()
  
        $next = $next.length ? $next : this.$element.find('.item')[fallback]()

`$next` should be a jQuery object representing the next slide. So `$next.length` should be 1. If not, then we need a fallback plan for which slide to go to, and that's provided by  `this.$element.find('.item')[fallback]()`.
  
        e = $.Event('slide', {
          relatedTarget: $next[0]
        })

[According to the jQuery docs](http://api.jquery.com/event.relatedTarget/), an event's `relatedTarget` should be "The other DOM element involved in the event, if any.", so the next slide that we'll hit is a logical choice. The plugin doesn't use this, but it's a nice handle to have for adding in some custom behavior when the `slide` or `slid` events are triggered by the code below. 

  
        if ($next.hasClass('active')) return

If `$next` is already the active slide, then we're done. 
  
        if ($.support.transition && this.$element.hasClass('slide')) {
          this.$element.trigger(e)
          if (e.isDefaultPrevented()) return
          $next.addClass(type)
          $next[0].offsetWidth // force reflow
          $active.addClass(direction)
          $next.addClass(direction)
          this.$element.one($.support.transition.end, function () {
            $next.removeClass([type, direction].join(' ')).addClass('active')
            $active.removeClass(['active', direction].join(' '))
            that.sliding = false
            setTimeout(function () { that.$element.trigger('slid') }, 0)
          })

If transitions are supported, we use them. 

        } else {
          this.$element.trigger(e)
          if (e.isDefaultPrevented()) return
          $active.removeClass('active')
          $next.addClass('active')
          this.sliding = false
          this.$element.trigger('slid')
        }
  
        isCycling && this.cycle()
  
        return this
    }

Otherwise, we just snap through the slides. 
   
## Carousel jQuery Plugin Definition

Most of the other code is defined inside an anonymous function expression and is only accessible from a carousel object. The three things that Carousel makes available in the jQuery namespace are:

* `$.fn.carousel` (a.k.a `$().carousel()`) 
* `$.fn.carouse.defaults`
* `$.fn.carousel.Carousel`


Bootstrap defines one function, `carousel`, that handles both initializing and manipulating Bootstrap Carousels, so it can take either an object containing initial settings, a numeric argument giving a slide to switch to, or a string specifying one of several actions ( `pause`, `next`, etc). 

    $.fn.carousel = function (option) {
        return this.each(function () {
            var $this = $(this)
              , data = $this.data('carousel')
              , options = $.extend({}, $.fn.carousel.defaults, typeof option == 'object' && option)
              , action = typeof option == 'string' ? option : options.slide
            if (!data) $this.data('carousel', (data = new Carousel(this, options)))
            if (typeof option == 'number') data.to(option)
            else if (action) data[action]()
            else if (options.interval) data.cycle()
        })
    }


`$.fn` is an alias for `$.prototype`. This confused and annoyed me the first time that I saw it in jQuery, but I'm completely over it now. Really. I'm told it's that way for historical reasons. 

`function (option) { return this.each(function () { ... })}` is cookbook jQuery for defining a jquery plugin. At this level, `this` is going to be a set of Carousel divs determined by some jQuery selector, so we'll need to loop through and process each one. 

`data = $this.data('carousel')`  will read in HTML5 `data-` attributes from the `carousel` div, or anything previously saved to the DOM object for that div. 

`options = $.extend({}, $.fn.carousel.defaults, typeof option == 'object' && option)` is cookbook jQuery for merging any options that were passed in to the function call with the default settings for the function (defined below). The default settings and any  provided settings are merged into a new object.  

If `data` is undefined, then it means we need to initialize a new carousel before proceeding , save that with the Carousel div that we're proccessing, and keep a reference to it in `data`. At this point, we' can be sure that `data` has a carousel. 

If we got a number, then we cycle to that number.  `data.to(option)`  will call the `to` function on whichever carousel we're currently looking and and switch it to that position.

If we got a string, then we assume it's an action and try to call it on the carousel with `data[action]()`


Otherwise, we just start cycling. 

### Default Options

This is cookbook jQuery for a default options object.

    $.fn.carousel.defaults = {
        interval: 5000
        , pause: 'hover'
    }
	

### Public Interface for the Constructor

By convention, Bootstrap wires up the object constructor `Foo` to `$.fn.foo.Foo`.  This gives us a handle for calling this constructor that can be used from outside the closure in which all of the code for carousel is wrapped. 

    $.fn.carousel.Constructor = Carousel
	
	
	

---
layout: post
title: A Case for CoffeeScript
tags: 
---
![](http://media.tumblr.com/tumblr_mdl8g9UIDY1r9fv8b.png)

# A Case for CoffeeScript

I’ve been writing JavaScript applications for the past few years. I’ve spent a
lot of time learning JavaScript, and I’ve established a pretty solid
development work-flow. We use [CoffeeScript](http://coffeescript.org/) here at
Gaslight, so when I came on-board, I was not at all excited to switch. I’ve
spent the past few months in CoffeeScript, I can honestly say that I was
wrong. I think CoffeeScript is great and here’s why:

### Beauty

I hate to admit it, but JavaScript is just plain ugly. It’s not so much the
brackets, parens and semicolons that bother me. It’s that I have to type
things like `function(){}` and `var that = this` all over the place. I didn’t
notice it so much when I was writing JavaScript, but now when I switch back,
it bugs the crap out of me. CoffeeScript is expressive. It's loops,
comprehensions, splats and string interpolation are much cleaner than the
javascript counterparts. Using CoffeeScript, I can easily write code that
reads well. I found that very difficult to do in JavaScript.

### Whitespace

This had been one of my biggest perceived qualms with the CoffeeScript syntax.
I’ve never liked significant whitespace in languages. In reality, I didn’t
find it to be a problem. I’m really OCD, so I can’t stand reading code that
isn’t properly indented. With JavaScript, I spent a lot of time making sure
that my code was well-styled. CoffeeScript takes some of that away by ensuring
that my indentation is correct. I’ve also found that CoffeeScript is pretty
liberal with its syntax. Things like object literals, loops and conditionals
can be put on one line or multiple.

### Debugging

I spend a lot of time in the Chrome debugger, so the thought of stepping
through code that I didn’t write was a real concern. That ended up not really
bothering me. The best thing about CoffeeScript is that it’s nearly one-to-one
with JavaScript. I can step through the debugger and know exactly where
everything came from, and it’s rare that I get confused about the translation.
With source maps soon to be released, this won’t be a problem at all.

### Linting

When I was writing JavaScript, I always ran my code through a linter. With
CoffeeScript, I don’t need to. I know that the compiled JS will always be
valid. It protects me from those JS got-cha’s like forgetting to put `var` or
missing a trailing comma on a object literal.

## The Downside

CoffeeScript is great, but it’s not all unicorns and rainbows. The biggest
headache is setting up the compile step. A lot of Ruby people start a
Javascript project with Rails just to get this for free with the asset
pipeline. Most people still have to set up the workflow themselves. [Grunt
Coffeepot](https://github.com/rockwood/grunt-coffeepot) is a Grunt project
I’ve been working on to serve compiled CoffeeScript on-demand.

It’s also a pain when you need to convert someone else’s JavaScript into
CoffeeScript. I’m hoping that things like this will get easier as the tooling
matures. The CoffeeScript language is still young, and there’s a lot that more
that can be done to make the workflow easier.

### Yay, now I don’t have to learn JavaScript!

NO! CoffeeScript is an enhancement, not an alternative. It helps to have have
an understanding of JavaScript before switching to CoffeeScript. For example,
if you’re going to use CoffeeScript classes, you should take the time to
really understand Javascript’s prototypal inheritance. Sometimes it makes more
sense not to use classes, and it’s good to have the basic understanding of
native JavaScript so that you have options. Things like the `extends`
operator, and CoffeeScripts’s list comprehensions are doing some really crazy
stuff behind the scenes that you should be aware of. Coffeescript won’t write
good code for you. That’s your job.

## Try it!

I was once a CoffeeScript hater, but these past few months have changed that.
JavaScript pre-compilers are getting more and more popular, and CoffeeScript
is just one the many out there. As the tooling for these pre-compilers
matures, more developers will adopt them. Even if you cringe at the idea of
pre-compiled JavaScript, it’s worth giving it a try.

* * *

[![](http://media.tumblr.com/tumblr_mdl903ZYC41r9fv8b.png)](http://training.ga
slight.co/workshops/3-2012-december-san-francisco-beautiful-front-end-code)
Gaslighters Chris Nelson and Kevin Rockwood will be holding a San Francisco
instance of their [Mastering
Backbone](http://training.gaslight.co/workshops/3-2012-december-san-francisco-
beautiful-front-end-code) training course on Dec 3 - 5. Check it out!

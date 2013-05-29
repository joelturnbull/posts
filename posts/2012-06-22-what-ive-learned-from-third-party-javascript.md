---
layout: post
title: What I've Learned from Third-Party JavaScript
tags:
- javascript
---
**UPDATE 7/31/2012:** As mentioned below, I spent some time at BackboneConf talking with [Alex Sexton](%5Bhttp://alexsexton.com/) about third-party javascript. He had been one of the early technical reviewers for [Third-Party Javascript](http://bit.ly/thirdparty-js) by Ben Vinegar and Anton Kovalyov. Alex had nothing but good stuff to say about this book. I've only read a little of it through Manning's Early Access Program. I think Ben and Anton really know what they're talking about and bring a lot of experience to this subject. I look forward to finishing their book.

# What I've Learned from Third-Party Javascript

I am not the foremost JavaScript developer on our team. In fact, I consider
myself more of a back-end developer than front-end. That's a little ironic
given that I have more than a year's experience developing backbone.js web
client applications. The last couple weeks have thrown me into a new realm of
JavaScript development: third party libraries.

## A New Level of Rigor

We're developing a software serviced used by our clients by including
JavaScript onto their pages. This raised issues I had not considered much on
previous JavaScript projects. The responsibility of injecting Javascript into
an application you don't own brings on a whole new level of rigor:

  * You have to be very careful not to pollute the global namespace and also not to rely on anything from the global namespace. 
  * More than ever, you have to pay attention to the size of your application as delivered to the client's page. 
  * You have to be more diligent about memory management. 

What's the solution? The initial version of the project took a very no-
nonsense approach: do as little as you can get away with using straight-up
JavaScript and the standard libraries shipped with the browsers. That was
probably a good decision given the early stage of the project and the modest
functionality required. However, the new features required a lot more
sophistication.

## Use Good Practices

First and foremost, good software is still good software. I think there's a
lot of temptation to allow your JavaScript to grow in unmanageable ways.
Keeping your code modular is still the right thing to do. Follow the principle
of single responsibility. Create objects that do one thing and are well
tested.

For me, mainly because I'm not a great JavaScript developer, I rely on
CoffeeScript's classes. I don't think they are the most pure way of creating
JavaScript objects; particularly after reading Crockford's _JavaScript: The
Good Parts_. However, they are easy for me to understand and implemented well.
Coming from a Ruby background, using CoffeeScript classes and Jasmine specs
maps well to my experience.

## Get Small

What I quickly learned though, is that writing native JavaScript is hard. I've
learned to lean so heavily on JQuery and other fantastically rich libraries,
it was hard for me to fall back to working without them. After some work, I
was able to relearn how to access the DOM natively and even to some extent
events handling. It was a lot of work though and tricky to test.

Fortunately, you don't have to give up on libraries for help just because
JQuery is huge. There are many microframeworks to fill specific niches. Once
we started trying to deal with events on our own it became clear we should be
using some microframework to help. We settled on Bean and Qwery. We also
decided on 140medly for help with ajax requests. I found the site
[http://microjs.com](http://microjs.com) pretty nifty for exploring what's out
there.

## Stay Out of The Way

The question becomes, how do you include these libraries into your application
without polluting the global namespace or colliding with existing versions
that may already be on the window? The answer is that it's hard. I'm not sure
we've got the right solution yet.

We ended up creating a single global on the window for our application. We
then downloaded and edited the libraries we were using to have them insert
themselves into our namespace rather than onto the window. I feel reasonably
confident that our global object won't collide with anything on the client's
page. It would be better if we didn't add ourselves to the global space at
all, but it's a compromise with simplicity I'm willing to make. Editing vendor
JavaScript isn't that cool, but for expediency I'm okay with it for now too.

## Stay Tuned In

This week though, I was fortunate enough to go to Backbone Conf in Boston, MA.
I spent some time talking with [Alex Sexton](https://github.com/SlexAxton)
about require.js. At this point, I feel like I should try to rebuild my little
library with require.js and see how it feels. Being a Rails guy, it's pretty
easy to lean on the asset pipeline (Sprokets), but I'm not sure that's really
the best solution. I'll post some more once I experiment a little.

In the meantime, Happy JavaScripting!

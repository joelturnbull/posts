---
layout: post
title: Web apps are dead, long live web apps
tags: 
---
I was at rubyconf today talking to a guy who hates building web apps.  I think
a lot of developers feel this way, even a lot of web developers.  I know I
did, to some degree, even though I would tell you (and believe that) deploying
software as a web application is the best way to go for most cases.  What was
interesting to me in talking to the guy were some of the reasons he gave:

  * Web apps are just way too chatty.  Every time I want to do anything it's a round trip to the server to figure out what to do next
  * Javascript is just such a horrible language.  It was some guys experiment that got shoved into the browser and now I have to use it.  I don't have any choice in what language to use

These are rough paraphrases; hopefully the speaker, should he read this post,
will forgive me.  But I think these are common reactions.  And if you read no
further in this post, here's the TL;DR: they **no longer apply**.  These
reactions are based on the way we've built web apps for the past 10 years or
so.  But now it is time to stop.

The chatty question first: the problem he alludes is one of the most limiting
features about the way web apps used to be built.  Because the first version
of the web is built around each user action resulting in an http
request/response cycle, server frameworks were architected around this idea.
The server framework would handle each user action and send back a wad of html
and javascript that would (hopefully) tell the browser what to do next.
Building interesting, usable applications this way is frustrating, to put it
mildly.

But of course AJAX came along to rescue us from all this.  So what, right?
AJAX is not new, it's been mainstream for 5 years, at least.  But believe it
not, it's full potential is really just now starting to be realized.  The full
potential is that it frees us from tying user actions to a request/response
cycle.  But to realize this potential, we need to completely change the way we
build web applications.  As in, throw away almost everything you knew,
including (most of) your server side framework.  As in, if you're building a
web application with the existing "state of the art" approach, you are a
writing instant legacy code.

So what do we do?  It's time to re-tool: that server side framework is on the
wrong tier.  We need to move it to the client.  Backbone.js is the best client
side MVC framework I've used, but there are certainly others.

The next question: javascript.  While I disagree with this guy's vehement
hate, I can certainly understand the frustration.  Javascript has some
excellent features, but it's not my favorite language.  And the fact that I'm
forced to use it makes me want to demand: FREEDOM! in a Mel Gibson painted
half-blue and being sliced open kind of way.  I want to choose what language
to use, and I want it to let me write expressive, beautiful code.

And now I can, with coffeescript.  I really can't overstate the effect this
has had on my approach to web app development.  If I look back honestly, I've
known for maybe years that I'm doing it wrong.  Even though I love my chosen
server side language, Ruby, so very much, I could tell that trying to build
the UI for my web apps in it just wasn't working out.  I just couldn't bring
myself to totally embrace the javascript, though I tried (maybe a bit half-
assedly).  But this changed completely with coffeescript.  I love the code I
write in this language, and I can get behind it in a non-half-assed way.

As you've probably picked up, I'm pretty passionate about this stuff.  Part of
it is that I'm so pumped that I finally have good solutions to problems that
have been vexing me for years.  But I also want to help others get on board.
A lot of folks already are, but if you're not, or just want help getting there
we got your back.  Gaslight Software is offering our training class,
[Beautiful Front End Code with Backbone and
Coffeescript](http://training.gaslightsoftware.com) in San Francisco on
November 7th and 8th.

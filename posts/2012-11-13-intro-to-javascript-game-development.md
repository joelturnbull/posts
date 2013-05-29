---
layout: post
title: Intro to JavaScript Game Development
tags: 
---
![](http://media.tumblr.com/tumblr_mdfocnbzVo1r9fv8b.png)

It's probably not surprising that many of us Gaslighters would consider
ourselves gamers. In fact, as I write this, I'm over-hearing
[@dougalcorn](https://twitter.com/dougalcorn) and
[@st23am](https://twitter.com/st23am) standing at the coffee machine, sharing
their weekend "Eve Online" exploits. As such, it was our great pleasure to
provide a home for a couple of aspiring young game developers this Summer. If
["Indie Game: The Movie"](http://buy.indiegamethemovie.com/) and this brave
new world of the internet has taught us anything, it's that game development
and publishing are no longer confined to the realm of big budget studios. The
tools and channels are available to those with the urge to create.

One of our guests, Vincent Wilson, will be presenting at this week's
[CinciJs](http://cincijs.com) group here at Gaslight. He was kind enough to
write up some of his experiences, cutting his teeth on a popular JavaScript
game development framework.

* * *

Hello, my name is Vincent Wilson and I am a sixteen year old who is highly
interested in software development. I have loved to program ever since I
discovered the art around the age of 11. I have been acquiring skill in
Python, Java, Javascript, Ruby, HTML and CSS since that time.

This summer I worked with Josh Alcorn on creating "The Inferno", a platformer
game in the theme of Dante’s Inferno. Over a period of around two weeks, Josh
and I met up five times at Gaslight to develop the game (or at least part of
it). The work was done using [Impact.js](http://impactjs.com/), a
Javascript/HTML5 Canvas Game Engine.

To begin the project, we took examples out of Jesse Freeman’s book,
[Introducing HTML5 Game Development](http://www.amazon.com/Introducing-HTML5
-Development-Jesse-Freeman/dp/1449315178). Using the ideas contained within
the book, we constructed a simple class that managed the movement of the
player, including running and jumping as well as a little bug testing feature
/ Easter egg, flying (if you are curious, trying pressing F, L, and Y all at
the same time while the canvas is in focus). I completed most of this while
Josh began work on the graphics for the levels we planned to create.

Over lunch on the first day we decided that we wished to create, in episodes,
the nine layers of Dante’s Inferno. We began on the first ring, limbo.

![](http://media.tumblr.com/tumblr_mdfohpPLf61r9fv8b.png)

Impact.js contains a tile based level editor that makes creating levels as
simple as creating the assets, importing the assets into the level editor
(entitled Weltmeister), and then dragging to paint tiles. You can paint
collision tiles to define the collision, as well as placing entities that have
independent .js files into the level.

Impact.js has a fairly robust physics and collision system built into the
game. Physics are handled simply, and moving is accomplished, at least within
our game, by simply setting an acceleration for the player in the player
class's update function while a key is held down, and then setting the
velocity to zero when no keys are held down.

By overriding the engine's "check" class it is possible to modify what happens
when two entities collide. Each entity has a built in check class (which
manages collision) and a kill class (which is called either manually or when
an entity has taken a set amount of "damage"). In the example of the spikes
which can be found throughout the planes of limbo, there is a trigger entity,
which when touched by the player, calls a hurt entity, which then deals a
configurable amount of damage to the entity that collided with the trigger. By
overriding the player's kill class, I was able to spawn a few bloody chunk
entities (subclassed from a particle entity) which are designed to fade away
after a set amount of time.

We decided to make a platformer because of my previous experiences with
similar games, but Impact.js is a very capable engine. The applications that
can be created with it are nearly limited only by your imagination. Among the
games developed with Impact, I have seen dungeon crawlers, role playing games,
shoot em ups, pong look-alikes, and Tetris clones.

Along the way I learned about several important topics, the most prominent
being:

  * Scope creep: Josh and I began with a simple idea; create a fun and functional level in Impact.js. As work on the project continued, the scope began to creep up on us. Fairly soon we had plans for features such as wall climbing and bouncy blocks and several other features which we didn’t have the time to implement. This led to a situation where we spread out our energy over an area that was too large, allowing us to accomplish little compared to focusing our energy on areas that we could complete.

  * The importance of version control in a multi-person project. I have used version control in the past, but it was only after this project that I began to realize the real advantages of using it. We began by using Dropbox to manage inter-computer file exchange but quickly stopped using that in exchange for git because of Dropbox’s inability to efficiently handle file conflicts. Using git along with Github we had no issues managing the files for the project with ease.

My time at Gaslight was time well spent. I would be speaking for both Josh and
myself if I stated that I readily enjoyed my time, and I very much wish to
continue to work in the software development field. You can view what we
completed at: [http://le-server.com/inferno/](http://le-server.com/inferno/)

You can view some of my more recent work with Impact.js here: [http://le-
server.com/linetime/](http://le-server.com/linetime/)"

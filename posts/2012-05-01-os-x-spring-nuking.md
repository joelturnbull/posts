---
layout: post
title: OS X Spring Nuking
tags: 
---
Sometimes you just get fed up with your broke-ass environment and you need to
start over. Despite best intentions, you temporarily lose your vigilence, and
you end up with macports and homebrew on your machine at the same time.
Strange poltergeists creep into your machine and start warning you about
Nokogiri every time you execute rails. Command-T completely refuses to work
for no good reason, and you become suspicious, which'ing and --version'ing
every call. Your home dir has accumulated so much junk, tab completion has
become useless.

Stop the insanity. Nuke it all. And by nuke, I mean delete everything and re-
install OS X. I've always been a fan of starting from scratch, and the best
thing about nuking is, the more you do it, the easier it is. My latest foray
has been my finest to date, thanks to a short consultation with Chris Moore
([@cdmwebs](https://twitter.com/#!/cdmwebs)), a connessuier of nuking the
things.

After moving all my keeper stuff offshore, to Dropbox or whanot, I wiped out
my hard drive and reinstalled Lion ( hold Command-R during restart ). I did
the Mac setup stuff and once I was able to hit Safari, I found myself
installing a few desktop apps, 1Password was needed almost immediately. Shout-
out to SizeUp, apparently I can't live without it either.

I didn't import all of my old 1Password data back in, because, surprisingly,
my 1Password was also a horrific mess. So my strategy is to pull things from
offshore as I realize I need them, which is rarely. More often than not, I
find myself just resetting all of my online passwords, it's easier than
importing and it doesn't seem like a terrible practice.

My next step was to start using Time Machine. I've stopped using it in the
past because I was doing it wrong. At this point in the process, it seemed
pretty essential to be able to roll back because installing code is like
adding salt, it's very easy to do it, not so much the reverse.

The big exciting next step, on Chris's suggestion, was to install [Cinderalla
for Mac OSX](http://www.atmos.org/cinderella/) ( previously Cider ). I was
really hoping this was going to work out, and it did. It only required Xcode,
so I installed the latest.

Cinderella is a gem, that utilizes homebrew and chef to install all of the
things. Namely: Ruby, Python, Node, Erlang, MySQL, PostgreSQL, Redis, Riak,
Elastic Search, Memcached, and MongoDB. Do I need all of those things? No. Do
I need one third of them? Almost. Have I worked with some of these things in
the past, and do I not wholly reject using any of them in the future? Yes. Do
I respect the idea of some of these things and fantasize about someday
becoming the master of them? Yes. Would I like to just set this thing to run
for a couple hours and play games on my iPhone? Yes. Carry on.

So I immediately encountered some sad stuff on attempting to install the gem.
The issue and fix are outlined [here](http://www.ruby-forum.com/topic/191688).
Look for the post by cpeterson, in which you install "Command Line Tools" via
an Xcode menu.

Once that was cleared up, it took a while, but with two commands Cinderella
installed and set up all of those things, not to mention homebrew and rbenv,
and put it all in ~/Developer. Quite tidy.

Cinderella finished, I cloned one of our Rails projects, did some obligatory
setup and was running it without issue. Validated, I've just implemented
Chris's final pro-tip, which was to version all of my dot files. I've just
commited and pushed my vim, bash, and git configs.

I love how the last step is already preparing me for the inevitable, I can
hardly wait to nuke again. When you've come to terms with the fact, like me,
that you are a messy piggy of a developer sometimes, or you accept the fact
that you're still trying to get a handle on Unix best practices, just resolve
to become really good at cleaning your pen.

---
layout: post
title: ! 'CocoaPods and You: A Primer for the Uninformed'
tags:
- cocoapods
- ios
- xcode
- objective-c
---
I'd been meaning to actually build something worthwhile with iOS since I first
heard about the SDK. A few months ago, we started work on a client project
that was going to start out life as a mobile web application and evolve in to
a native app. [Peter](https://twitter.com/pkananen) was interested in the
project and often said that we would be able to deliver faster by just giving
up and going native. Since the app used quite a bit of GPS functionality, we
decided to give it a day and spike something out.

As a Rubyist, I'm a bit spoiled. A couple years ago,
[Carlhuda](https://github.com/carlhuda) began working on a project that had a
bit of a rough start but quickly became a standard link in my toolchain. Of
course I'm talking about [Bundler](http://gembundler.com).

I'd heard about [CocoaPods](http://cocoapods.org) and had experimented with
iOS before, but never taken the time to set things up and give it a try. On
this spike, we gave it a shot. There was a bit of fumbling at first, mostly
because of my own ignorance of linker flags and header search paths. Once we
were set up, we were off and running.

Our experience with CocoaPods has been pretty great. It takes care of much of
the "busy work" involved in using external libraries in your Xcode
applications. Recently, though, I've heard some iOS developers berate
CocoaPods, so I thought I'd take a few minutes and explain just how much
you're getting for free when you "give up and use pods".

## The Old Way

I'll use the quick and easy setup from [Sam
Soffes](https://twitter.com/samsoffes)' [SSToolkit](http://sstoolk.it) page as
a reference. You can see that it isn't trivial. Or quick. Or easy.

![sstoolkit_img](http://cdmwebs.com/s/SSToolkit-20120822-231013.jpg)

Nine steps. Nine! Before you get to do any of the fun stuff! Plus, now you've
downloaded old code. Want to update? Download the new zip, extract it, add the
right files (did they change? are there new files?), click Build and wait
patiently for the spinning wheel of death. Maybe not quite that drastic, but
still more work than I'd like to do.

#### Wait, wait! You can use…

Yeah, I know. I can eliminate the download of the zip/tarball step with
everyone's favorite: [git
submodules](http://www.kernel.org/pub/software/scm/git/docs/git-
submodule.html)!

I kid. Submodules have come a long way, too. They're actually really awesome
these days, but I **still** have to do all the stuff after I get the library
on my filesystem.

## The Better Way

Now let's do the same thing with CocoaPods. Keep in mind, though, that the
setup is a one-time cost. Once you've got a Podfile, adding a new library is
as easy as adding a new line and running `pod install`. If you're the
screencast watching type, go no further than
[NSScreencast](http://nsscreencast.com/episodes/5-cocoapods) and watch right
now. That one's free, but they're well worth the subscription fee.

  1. First, install CocoaPods. If you've never messed with your Ruby installation, it's as easy as `sudo gem install cocoapods`. Shouldn't take too long.
  2. Just this first time, run `pod setup` and CocoaPods will clone the spec repo to a folder in your $HOME folder. Podspecs are the files CocoaPods uses to learn how to find, install and configure libraries.
  3. Next, create and edit a Podfile. It's case sensitive, so make sure to call it that. Here's what we'll put in there:
{% highlight python %}

    platform :ios
    xcodeproj MyProject.xcodeproj

    pod SSToolkit
{% endhighlight %}

  4. That's it. Run `pod install` and watch the magic. The first time you install, CocoaPods will create a new Workspace that sets up your project and the CocoaPods project properly. **Make sure you open the Workspace file going forward!**
  5. Ready to update the code? As of now, you'll have to delete the library you want to update in the Pods folder and run `pod install` again. It [looks like](https://github.com/CocoaPods/CocoaPods/pull/447) we're getting some `update` and `outdated` subcommands soon, though!

Here's what CocoaPods just did for you:

  * Cloned the repo (or downloaded the zip archive) of SSToolkit.
  * Linked the Pods project to your existing app's target.
  * Added the necessary files or directories to the Header Search Paths.
  * Using ARC and the library isn't? That's handled, too. -fno-objc-arc is set on all the included library's files!
  * **BONUS**: If you've installed [appledoc](http://gentlebytes.com/appledoc/), it's **generated documentation from the library** and it's ready for you to search and use in Organizer!

That's quite a bit of awesome. Now go write all the code! If you'd like to
read a bit more, check out this post on [Ray
Wenderlich](http://www.raywenderlich.com/12139/introduction-to-cocoapods),
too.

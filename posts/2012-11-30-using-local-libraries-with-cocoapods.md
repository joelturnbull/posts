---
layout: post
title: Using Local Libraries with CocoaPods
tags:
- cocoapods
---
![](http://media.tumblr.com/tumblr_meb68rJMHl1r9fv8b.png)

We're big fans of [CocoaPods](http://cocoapods.org) here at Gaslight. We like
having [dependencies managed for us](http://blog.gaslight.co/2012/08/23
/cocoapods-and-you-a-primer-for-the-uninformed.html), [source
fetched](http://blog.gaslight.co/2012/08/23/cocoapods-and-you-a-primer-for-
the-uninformed.html) and [compiler and linker flags set
automatically](http://blog.gaslight.co/2012/08/23/cocoapods-and-you-a-primer-
for-the-uninformed.html).

We've been using a proprietary library called
[SensibleTableView](http://sensiblecocoa.com/overview/) for building out
TableViews quickly in our project. Since it's proprietary, we can't just give
Cocoapods a podspec and fetch it automatically, but I didn't want our project
cluttered with dependencies. So, here's how we made it work.

## Enter Local Podspecs

CocoaPods allows the use of [local
podspecs](https://github.com/CocoaPods/CocoaPods/issues/178). That means you
can use a local path to a directory on your computer and write a Podspec in
that directory and use CocoaPods just like you'd expect. Here's what our
Podfile looks like:

{% highlight python %}
platform :ios, 5.1
inhibit_all_warnings!

pod SensibleTableView, :local => "vendor/frameworks/STV 3.1.3 Pro/Source Code/SensibleTableView/"
pod STV+CoreData, :local => "vendor/frameworks/STV 3.1.3 Pro/Source Code/STV+CoreData/"
{% endhighlight %}

Of note, `inhibit_all_warnings!` is a pretty handy little setting to hide all
the noise in libraries you're using from CocoaPods. It's nice to remove a bit
of noise you don't need to focus on at the moment.

Here's the Podspec for SensibleTableView:

{% highlight python %}
Pod::Spec.new do |s|
  s.name = SensibleTableView
  s.version = 3.1.3
  s.platform = :ios
  s.ios.deployment_target = 5.0
  s.prefix_header_file = SensibleTableView/SensibleTableView-Prefix.pch
  s.source_files = SensibleTableView/STV-Core/*.{h,m}
  s.requires_arc = true
end
{% endhighlight %}

It's path is `vendor/frameworks/STV 3.1.3 Pro/Source
Code/SensibleTableView/SensibleTableView.podspec`, if you're wondering.

That's it. Cocoapods is great and you should be using it, too.

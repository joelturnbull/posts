---
layout: post
title: Forms and JSON in iOS
tags:
- ios
- objective-c
- ipad
---
Over the last six months we've been building an application for a client in
the agricultural industry. There was a need to use portions of the application
on an iPad literally out in the field. The app also needed to be location-
aware, allow offline access, and to be able to quickly render geographical
data.

These factors made the decision to write an iOS app targeted to the iPad
pretty simple. But in an iOS application where the user is collecting a lot of
data, two challenges became apparent quickly.

  * We needed an easy way to build forms around complicated object structures. 
  * We needed a way to easily talk to the RESTful JSON services we exposed in our Rails app.

We utilized two third party iOS frameworks to help meet these two challenges,
and with mixed results. What follows is a review of those two frameworks,
Sensible Table View and RestKit.

## Sensible Table View

Most user activity in the app consists of recording field observations in the
context of a larger report. Each report can have several observations, and
each observation can have many different subcomponents. Essentially, we needed
a way to support forms several layers deep, with lots of different data type
controls. Thankfully, UINavigationController was a natural fit for the report
workflow itself, as users would often be navigating into and out of detail
views. However, as many have experienced, UIKit really lacks a decent form
builder. Specifically, UITableViewController does not naturally orient around
an object, or collection of objects. While there are a few form-building
frameworks, none appeared nearly as complete as
[SensibleTableView](http://www.sensiblecocoa.com), a paid framework.

STV is able to build a table view representation of an object, including
subviews for that object's relationships. By default, it will introspect your
attribute types and display them with the corresponding UITableViwCell data
type. This alone is extremely useful. STV also integrates with Core Data,
provides many default UITableViewCell styles, and even allows for out of the
box editing for object and attribute collections. The licensed product
includes the source code, which is a good thing. STV does a lot of work for
you, and at times I found myself not understanding what has happening or
wondering why something wasn't working the way I expected it to. To be honest,
I spent a lot of time debugging source code.

The complexity of the framework required non-trivial rampup time, and it took
a day or two of development time to get a handle on how to use STV in our
application. STV does provide for decent code reuse, as class definitions used
to create a table view are easily used in multiple screens. This was helpful
in our application. Still, decisions need to be made about how many levels of
views you'd like STV to create, and at times not fully owning the code for the
view three levels deep from your object can be painful. Expect to set lots of
tags on objects and implement delegate methods to customize as needed.
Overall, I think we saved development effort by using SensibleTableView, but I
also think we could've done almost as well by just using a formbuilder such as
[IBAForms](https://github.com/ittybittydude/IBAForms) or
[QuickDialog](https://github.com/escoz/quickdialog), and then creating
explicit detail view controllers as opposed to letting STV generate these
views. At the end of the day, STV seems to almost do too much. This may save
some time in the short term, but it's not a lightweight framework and it will
touch many parts of your application. Thankfully, you are not forced to use
every feature of STV, and can use only the parts you find most useful, such as
UITableViewCell styles.

## RestKit

Consuming JSON in Objective-C can be accomplished in a few different ways.
Starting in iOS 5.0, Apple provides NSJSONSerialization, which can be used to
parse HTTP requests. For simple scenarios, that works fine. However, in a data
structure with several layers of nested associations, JSON mapping code
quickly becomes tedious. A framework such as AFNetworking eliminates some of
the HTTP boilerplate in this approach, but still doesn't solve difficult
object mapping problems. We decided to instead use RestKit.

RestKit provides a relatively simple way to talk to RESTful services, mapping
response payloads to Objective-C objects without a lot of boilerplate, and
serializing objects back to JSON. To map objects with RestKit, class mapping
definitions are typically added to the application-wide RestKit object
manager. In addition to attributes, RestKit lets you define relationships
between your models, and complicated JSON structures can easily be converted
to Objective-C objects, and vice versa. This is really useful and powerful.
RestKit provides a simple router that allows you to associate HTTP verbs and
an Objective-C object mapping with a particular URL, so talking to your
RESTful services is quite simple. RestKit also handles a lot of HTTP concerns
such as caching, handling status codes, and dealing with HTTP headers. It,
too, is not a lightweight framework but the problems it solves seem to be
worth the impact it has in your application.

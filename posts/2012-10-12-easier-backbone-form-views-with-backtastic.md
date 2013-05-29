---
layout: post
title: Easier Backbone form views with Backtastic
tags: 
---
Over the last couple of years, we at Gaslight have built a lot of web
applications that consist of a backbone.js front end talking to a rails
backend.

Backbone.js is light-weight and relatively un-opinionated, and this is a large
reason it's been so successful.  But this also means that there is still a
fair bit of manual work to do on the backbone.js side.  Particularly if you're
building a form-heavy app and are used to rails, some of this work can feel
quite tedious. In no particular order, some of the non-trivial bits that are
left to you:

  * building form markup
  * moving data back and forth from your views to your models
  * dealing with errors from the backend and rendering appropriate error messages
  * validation (client-side or server-side)

[Backtastic](https://github.com/gaslight/backtastic) is a framework I started
to try and extract some of the code I found myself needing on several projects
and automate away as much as I could of this effort.  The easiest way to
explain what it's about is by example.  Let's say you have a person view
within your backbone.js app that persists to a Person model in rails:

![Edit Person](https://img.skitch.com/20121009-rsypmaxjifq69winnrxbiyw8b9.png)

With Backtastic, here's what the backbone view code would look like:

[https://gist.github.com/3868572.js?file=edit_person_view.coffee](https://gist
.github.com/3868572.js?file=edit_person_view.coffee)

The first thing you'll notice is that there's nothing in this code to deal
with the form, either rendering it or populating it's values onto the model.
That's because backtastic takes care of this for us: we get this functionality
because our view extends from Backtastic.FormView.  Notice also the form
submit event is bound to save.  This method is inherited from Backtastic.View,
and as you might guess, calls save on the model after it has been populated
with it's values from the form.

Now let's look at the view template:

[https://gist.github.com/3868572.js?file=edit_person_view.hamlc](https://gist.
github.com/3868572.js?file=edit_person_view.hamlc)

Here we see the view helper methods that build form elements.  Again, we get
these methods from Backtastic.FormView.  They do more than just render form
element markup, though.  What they're actually doing is creating subviews, or
smaller views that render within our larger form view.  Each view is
responsible for:

  * rendering form element markup
  * listening to form element events and updating the model
  * listening for validation errors from the model and displaying them

There are form element subviews and corresponding helper methods for text
fields, checkboxes, select boxes, and date pickers. Expect more to come along
soon, and pull requests are of course welcome.

The observant reader might also notice that we're using twitter bootstrap.
This gives us nice layout for our forms, and gives us some help with
displaying validation errors as well.

## Validation in Backtastic

Backbone, out of the box, doesn't really give us much help in the way of
validation. We have a validate method on Backbone.Model, and are left to
implement it ourself. Returning anything non-null from this method indicates
failure, but it's up to us what exactly it returns and what it means.

Backtastic gives us a bit more help, particularly if you are using rails at
the back end. First, if you respond with validation errors from rails as json,
Backtastic will decode this json and send it to any response handlers that are
listening to "error" events on your Backbone models.  And conveniently, all
the form field subviews register themselves as listeners to their model's
error event and will display errors for their own field. We leverage twitter
bootstrap's css to provide nice styling for error messages.

But what about doing validation on the client side? It's worth pointing out
that client side validation by itself is never good enough. Because you always
need validation on the servier side, Backtastic gives you a way to easily
bring the validations from your rails models to your backbone models.

Here's how it works: Backtastic ships with a rake task,
backtastic:validations:build, which will reflect on all ActiveRecord models
and generate a file representing their validations in a format backtastic can
understand.  Here's what this looks like in the example app:

[https://gist.github.com/3868572.js?file=rails_validations.coffee](https://gis
t.github.com/3868572.js?file=rails_validations.coffee)

Somewhere in the setup of your client side app, you can then pass this
validation data to backtastic by calling Backtastic.applyValidations. Here's
what this looks like:

[https://gist.github.com/3868572.js?file=example.js.coffee](https://gist.githu
b.com/3868572.js?file=example.js.coffee)

The first arg is the namespace in which to find backbone model classes and the
second arg is the validation data. The net effect is that we now have
validations executing on the client side when I make changes to a field. Right
now, this works for a subset of validations: presence and format so far.
Expect this list to grow as Backtastic matures.

## See for yourself

The best way to dig in to [Backtastic](https://github.com/gaslight/backtastic)
further is to poke around the example app. All the code in this post come
straight from this app, which you can get to by cloning backtastic on the
githubs and running the rails app in the example directory.

## Backtastic needs you!

It's still early times for
[Backtastic](https://github.com/gaslight/backtastic), and it's a great time to
jump in. We'd love to see people contribute more types of form field views and
additional validations, just to throw out a couple ideas. These both should be
pretty easy to jump in with by following the "monkey see, monkey do" approach
from the form fields and validations that already there.

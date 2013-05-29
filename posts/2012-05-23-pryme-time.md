---
layout: post
title: Pryme Time
tags: 
---
I've been trying to expand my ruby toolset. The other day I had thirty minutes
to try out ruby-debug and failed. I couldn't get it operational, and I ended
up [here](http://blog.wyeworks.com/2011/11/1/ruby-1-9-3-and-ruby-debug) in a
sad state of affairs. How could the ruby debugger be languishing in the latest
ruby versions? I think last might I may have figured out why. The why is
[Pry](http://pry.github.com/).

## A Live State and Code Explorer

Calling up Pry reminded me of my sweet, sweet, beloved Smalltalk. If you're
not familiar, Smalltalk is a magical land of rainbows and hot-air balloons,
where objects dance and sing, and you can munch bite-sized clouds of cotton-
candy code all day long. Developing in Smalltalk, exploring the state and
protocol of live objects is simple and embedded and constant. It's all about
digging in and discovery, and Pry lets you do that ( sans mouse to boot ).

## A Pry Story

I'm learning cucumber. I writing step definitions in this DSL and it feels a
little like a foreign land. In my step definitions, I have objects like "page"
available to me and I don't even know what they are. And I wouldn't care,
until I'm trying test the contents of a Google calendar embedded in an iframe,
complicated.

So I want to explore "page". I put the line below in my step definition:

{% highlight python %}
{% endhighlight %}

Then, running cucumber invoked the Pry REPL:

{% highlight python %}
pry(#<Capybara::Session>)>
{% endhighlight %}

So it turns out "pages" are Cabybara::Sessions. What methods can I call on
them?

{% highlight python %}
pry(#<Capybara::Session>)> ls
Capybara::Session#methods: all  app  attach_file  body  check  choose  cleanup!  click_button  click_link  click_link_or_button  click_on  current_host  current_path  current_url  document  driver  evaluate_script  execute_script  field_labeled  fill_in  find  find_button  find_by_id  find_field  find_link  first  has_button?  has_checked_field?  has_content?  has_css?  has_field?  has_link?  has_no_button?  has_no_checked_field?  has_no_content?  has_no_css?  has_no_field?  has_no_link?  has_no_select?  has_no_selector?  has_no_table?  has_no_unchecked_field?  has_no_xpath?  has_select?  has_selector?  has_table?  has_unchecked_field?  has_xpath?  html  inspect  mode  reset!  reset_session!  response_headers  save_and_open_page  save_page  select  source  status_code  text  uncheck  unselect  visit  wait_until  within  within_fieldset  within_frame  within_table  within_window
self.methods: __binding_impl__
instance variables: @app  @document  @driver  @mode  @scopes
locals: _  _dir_  _ex_  _file_  _in_  _out_  _pry_
{% endhighlight %}

( This actually looks like a Smalltalk class definition to me. )

Browsing the protocol…"body" looks about right:

{% highlight python %}
  pry(#<Capybara::Session>)> body
    => "<!DOCTYPE html>
<html><head>
{% endhighlight %}

That's what I needed to get to. So in three quick steps I figured out:

  1. the class of "page"
  2. what methods I could call on "page"
  3. the current state "page"

## Coding Through Discovery

And that's everything I wanted to know. The beautiful thing is that never did
I have to scour through API documentation to figure out the the simplest use-
cases for this class. The output is clean and the environment is optimized for
discovery.

I'm only scratching the surface here. As a runtime-invoked REPL and debugger,
it's certainly doing the trick. But I've gathered another huge part of what
Pry provides is good tools for source and documentation browsing. Pry's
tagline is "Get to the code".

## Pry and Rails

You can use Pry in place of rails console by loading your rails app's
environment into Pry like:

{% highlight python %}

  pry -r ./config/environment
{% endhighlight %}

I'm really interested in exploring the ability to call pry from within the
context of a running Rails app. I'm not sure if this is a thing, it would be
so helpful to pop up a pry REPL from, say, a controller action. Believe me,
I've tried.

Oh, and pry remembers my command history between sessions, *cough* irb.

## Conclusion

I can't speak to ruby-debug, maybe it does similar things, but I suspect the
reason it failed to install for me is that everyone's discovered Pry. You
should too. Thanks @p9k for re-kindling my interest in this.

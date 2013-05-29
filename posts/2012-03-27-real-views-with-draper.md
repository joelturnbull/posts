---
layout: post
title: Real Views with Draper
tags: 
---
I recently latched on to a tweet thread that was originated by Michael Guterl
(@mguterl), in which he expressed his frustration with view helpers and erb in
rails. The frustration was echoed by others, and being somewhat new to Rails,
I can't say that I've personally felt much pain first hand, but's my
impression from the the get-go, is that erb templates seem to get ugly
quickly, and helpers aren't properly object oriented.

On the first count, I can't much fault erb for getting ugly. Those little are
powerful, you can't really blame the tool for providing you all the rope you
need to hang yourself with. I'm kind of intrigued by the idea of logic-less
templates though, in a masochistic kind of way, but maybe that's an
investigation for another blog post.

On the lack of object orientation, I'm intrigued. This seems wrong, but what
is the pain point here exactly?

During the course of the thread, Chris Nelson (@superchris) mentioned he had
been working with the Draper gem on a project and it had so far seemed like a
win. The idea of Draper is to provide real ruby class between the template and
the model where presentation concerns can reside. A view class, rather than a
helper. I decided to give it a try.

My starting point was to seek out an erb template on a production project that
was getting particularly ugly with logic, and see if Draper could help me
clean it up. I wanted to see how much I could do with helpers, and then if an
OO view could improve upon that.

### Getting Started

I watched the Draper RailsCast. Installing the gem, I got a generator that I
could feed an existing model class. It generated a decorator class associated
to that model in a new app/decorators/ directory. The class looks like this.

{% highlight python %}
class CallReportSummaryDecorator < ApplicationDecorator
   decorates :call_report_summary
{% endhighlight %}

The Decorator is a design pattern that "attach(es) additional responsibilities
to an object dynamically". Wrapper, or Presenter is another term. Our Draper
decorator is a view. It adds a layer of view related code, and delegates
everything else to the model it wraps.

I instantiate the new class in the controller, that here used to return a
collection of CallReportSummary objects. CallReportSummary(s) calculate and
compile some metrics, and the index page displays a table of these metrics
with a row for each summary.

{% highlight python %}
def index
  @current_tab = "Statements"
  @summaries = 
    @users.collect { |user| 
      CallReportSummaryDecorator.new( CallReportSummary.new(user, @date_range))}
end 
{% endhighlight %}

The controller now returns a collection of those objects wrapped in
decorators, but the app behaves as it always has. That's because the decorator
is automatically delegating all calls to the established model through method
missing. This is kind of an interesting divergence from the gang of four
implementation of the decorator, which get's it's methods by subclassing the
object it decorates.

### Usage

So how do we use it?

Consider the following piece of code that renders user data from a report
summary object into a table field:

{% highlight python %}
<td><%= summary.display_name %><%= ghost_icon(summary.user) %></td>
{% endhighlight %}

With a helper we could clean it up a little like:

{% highlight python %}
<td><%= display_user(summary) %></td>
{% endhighlight %}

But that's where things get screwy with helpers. Maybe a functional programmer
can set me straight on this someday, but it seems more suitable for
display_user to be method of the summary, as opposed to a function that acts
on it.

{% highlight python %}
<td><%= summary.display_user %></td>
{% endhighlight %}

Regardless of your preference there, the fact remains that there's no
intuitive way to structure gobs of helper code, in the way that standard Ruby
OOP can provide.

So, if you don't like the helper, your next option is to put the display_user
method on the model, where it doesn't belong. Views of a model are different
than the model, and you should have the capacity to view a model in different
ways. Draper provides that capacity where Rails doesn't. With decorators, you
can leave the model alone, and implement display_user in a view that wraps the
model. Then, you can turn around and implement display_user again as many
different times, in as many different views.

The Draper decorator has access to the model it decorates through @model, and
to all helper functions through the h. proxy. That includes ones you've
defined, and standard Rails ones like link_to and content_tag.

So the implementation in the decorator just becomes:

{% highlight python %}
def display_user
  @model.display_name + h.ghost_icon( @model.user )
end
{% endhighlight %}

I guess the lesson I learned here is that Draper, to me, is not so much about
cleaning up templates. That can be done with helpers and partials. So I
decided to investigate more the use of Draper as a tool for good OO design.

### Good Design

That user data rendering code was taken from this view that renders a table
with a row for each report summary returned by the controller.

{% highlight python %}
<table>
  <thead>
    <tr>
      <th>User</th>
      <th>Clicks</th>
      <th>Calls</th>
      <th>Convs</th>
      <th>Cost</th>
      <th>Cost/Click</th>
      <th>Cost/Call</th>
      <th>Click-To-Call</th>
      <th><input type="checkbox" name="check_all" id="check_all" value="all"/></th>
    </tr>
  </thead>
  <tbody>
    <% @summaries.each do |summary| %>
    <tr>
      <td><%= summary.display_name %><%= ghost_icon(summary.user) %></td>
      <td><%= format_number(summary.total_clicks) %></td> 
      <td><%= format_number(summary.total_calls) %></td>
      <td><%= format_number(summary.total_conversions) %></td>
      <td><%= format_microcents(summary.total_customer_cost) %></td>
      <td><%= format_microcents(summary.average_customer_cpc)  %></td>
      <td><%= format_microcents(summary.cost_per_call)  %></td>
      <td><%= format_percentage(summary.calls_per_click)  %></td>
      <td><input type="checkbox" name=send_summary[] id=send-<%= summary.id %> value="<%= summary.id %>" class="send_summary" /></td>
    </tr>
  <% end %>
  </tbody>
</table>
{% endhighlight %}

Each report row contains a checkbox that when checked and submitted, will
render that single report summary object into an email statement and send it
off to that particular client. The data rendered in that email statement is
different from the data rendered in the above administrator's view:

{% highlight python %}
<table>
  <thead>
    <tr>
      <th>Total Amount</th>
      <th>Impressions</th>
      <th>Clicks</th>
      <th>Cost/Click</th>              
      <th>Calls</th>
      <th>Convs</th>
      <th>Cost/Contact</th>
      <th>Click-to-Contacts</th>
      <th>Average Position</th>
    </tr>
    <tr>
      <td><%= format_microcents(@summary.total_customer_cost) %></td>
      <td><%= number_with_delimiter(@summary.total_impressions.to_i) %></td>
      <td><%= format_number(@summary.total_clicks) %></td>
      <td><%= format_microcents(@summary.cost_per_click) %></td>
      <td><%= format_number(@summary.total_calls) %></td>
      <td><%= format_number(@summary.total_conversions) %></td>
      <td><%= format_microcents(@summary.cost_per_call) %></td>
      <td><%= format_percentage(@summary.calls_per_click) %></td>
      <td><%= number_with_precision(@summary.average_pos, :precision => 2) %></td>
    </tr>
  </thead>
</table>
{% endhighlight %}

Hence, a scenario in which a particular model is displayed in two different
ways. The driving idea here is that the template should not also be the view.
The template should be concerned primarily with styling and markup, not with
choosing the data to be displayed. We have a view decorator now that can
determine that. With that in mind, my Draper-fied tables became:

{% highlight python %}
<table>
  <thead>
    <tr>
     <th>User</th>
      <% table_view.table_headers.each do | table_header | %>
        <th><%= table_header %></th>
      <% end %>
     <th><input type="checkbox" name="check_all" id="check_all" value="all"/></th>
    </tr>
  </thead>
  <tbody>
  <% @summaries.each do |summary| %>
    <tr>
      <td><%= summary.display_user %></td>
      <% summary.attributes.each do |attribute| %>
        <td><%= summary.render_attribute(attribute) %></td>
      <% end %>
      <td><input type="checkbox" name=send_summary[] id=send-<%= summary.id %> value="<%= summary.id %>" class="send_summary" /></td>
    </tr>
    <% end %>
  </tbody>
</table>
{% endhighlight %}

And the email:

{% highlight python %}
<table>
  <thead>
    <tr>
    <% table_view.table_headers.each do | table_header | %>
      <th><%= table_header %></th>
    <% end %>
    </tr>
    <tr>
    <% summary.attributes.each do |attribute| %>
      <td><%= summary.render_attribute(attribute) %></td>
    <% end %>
    </tr>
 </thead>
{% endhighlight %}

Note that the replacement looping code is identical between the two views. The
difference is in how table_headers and attributes are defined in the Draper
decorators that wrap the summary before it's either 1) displayed to an admin
screen or 2) rendered into an client email.

Previously, our decorator was called CallReportSummaryDecorator, the name
provided by the decorator generator. My inclination, now that we have two
wrappers decorating the same model, is to rename it more appropriately to what
it is, i.e. CallReportSummaryAppTableView. And then, to have a corresponding
CallReportSummaryEmailTableView.

Since each table header matches up with a piece of data, to me it makes sense
to define a hash in each decorator that provides the configuration for each
table.

In CallReportSummaryAppTableView:

{% highlight python %}
def column_attribute_map
  { 
    "Clicks" => :total_clicks,
    "Calls" => :total_calls,
    "Convs" => :total_conversions,
    "Cost" => :total_customer_cost,
    "Cost/Click" => :average_customer_cpc,
    "Cost/Call" => :cost_per_call,
    "Click-To-Call" => :calls_per_click,
  }
end 
{% endhighlight %}

In CallReportSummaryEmailTableView:

{% highlight python %}
def column_attribute_map
  {
    "Total Amount" => :total_customer_cost,
    "Impressions" => :total_impressions,
    "Clicks" => :total_clicks,
    "Cost/Click" => :cost_per_click,
    "Calls" => :total_calls,
    "Cost/Call" => :cost_per_call,
    "Click-to-Contacts" => :calls_per_click,
    "Average Position" => :average_pos,
  }
end
{% endhighlight %}

table_headers and attributes are methods that access that configuration:

{% highlight python %}
def table_headers
  column_attribute_map.keys
end

def attributes
  column_attribute_map.values
end
{% endhighlight %}

The identical implementations of those two methods in the two view classes
calls for a refactoring, a superclass, so I create a
CallReportSummaryTableView. Those methods can be defined once in that class
and inherited. render_attribute is in the same boat, it's implementation is:

{% highlight python %}
def render_attribute(symbol)
   send(symbol)
end
{% endhighlight %}

Formatted attributes that are shared between the two views, like
:calls_per_click, can be implemented in the superclass view like:

{% highlight python %}
def calls_per_click
  h.format_percentage(@model.calls_per_click)
end
{% endhighlight %}

Formatted attributes specific to each leaf view class can implemented there.

Note that the Draper gem generates an ApplicationDecorator class where general
formatting methods, like format_percentage, can be defined. Using it, it's
likely you could bypass application-specific helpers entirely.

{% highlight python %}
def calls_per_click
  format_percentage(@model.calls_per_click)
end
{% endhighlight %}

### Conclusion

I don't know if I performed any mind blowing feats here, but I feel good about
this. It's a bit cleaner, and my view code, previously in helpers, has a nice
OO home now where I've already started to do some refactoring and code-
sharing. In the template, I'm able to call display-related methods on the
model, which feels more natural than invoking a detached helper method. But I
don't have to implement those display concerns in the model itself, where they
don't belong.

All in all, I'm feeling confident that Draper is a crucial ingredient to good
design. This feels less like a gem that provides some functionality, and more
like an integral, missing piece of Rails to me.

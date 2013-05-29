---
layout: post
title: Rails Rumble Recap
tags:
- rails
- startups
---
Last weekend I participated the [2012 Rails Rumble](http://railsrumble.com/).
The Rumble is programming competition where teams of up to 4 people have 48
hours to build a web application with Ruby on Rails.

My teammate [@joshowens](https://twitter.com/joshowens) had actually won the
contest in 2007 with his recipe building application "Tasty Planner". In the
years since, I got the impression that the field had grown quite a bit. When I
heard that there were 500 registrations this year, I was wondering what I was
getting myself into.

After the fact though, my attitude is that this competition is totally
winnable. If you know how to play the game, a decent idea and good planning
beforehand can take you a long way.

## ShareBelt

The idea for the application was hatched by Josh. It's a tool for encouraging
sharing on website or blog. It displays a single, prominent share option onto
a web page, tailored to each visitor based on where they came from. That
message and share button floats in a bar at the top or bottom of any page.

It's called "[Sharebelt](http://sharebelt.com/)". If a user arrives at your
ShareBelt-enabled blog from Twitter, they might see a message at the top of
the page: "Hi I see you came from Twitter, why not [Retweet] this?", with a
button allowing them to do so.

![](http://media.tumblr.com/tumblr_mcfc4blx6P1r9fv8b.png)

Our initial prototype would recognize visitors from, and allow sharing to
Twitter and Facebook. Our backend administration application would capture
statistics, and report on daily Sharebelt impressions, clicks, tweets, shares,
and likes for our users.

## ShareBelt Implementation

There was a pretty obvious separation in the technology required to make
ShareBelt happen, so splitting the work between two developers was pretty cut
and dry.

The ShareBelt itself is implemented on the client-side with Javascript. A
ShareBelt account holder receives a snippet of html code to paste into their
website, anywhere they want the ShareBelt to pop in. The snippet contains an
id that represents the site of the account holder in the ShareBelt system, and
a script tag that loads the main Javascript code from a location on the
ShareBelt server.

The ShareBelt backend is the Rails piece. It provides routes for the data
capturing, and an administration interface for ShareBelt clients. Data is
captured when users take actions like tweeting or liking. The administration
interface allows ShareBelt users to set up and see reporting and analytics on
any number websites.

## Technical Challenges

As the main Javascript developer, my first challenge was wrestling with cross-
domain Ajax requests. The problem occurs any time javascript code running on
site attempts to call out to a location outside the current domain. Browsers
will prohibit these kinds of Ajax requests, but the workaround is so common
that it makes the restriction quite trivial.

Although browsers will restrict the Ajax call, they will happily load and run
Javascript at a location set in the "src" attribute of a script tag. The
strategy is then to, instead of calling out with Ajax, to call out with a
script tag. When you append the script tag into a document, it gets fetched
and executed.

{% highlight python %}
injectScript: (d) ->
    script = d.createElement("script")
    script.setAttribute("src", "http://endpoint.com")
    d.head.appendChild(script)
{% endhighlight %}

In order to do something with what that call returns, the endpoint needs to
return JSONP. Executed JSONP calls a method on the client-side that has been
defined by you, most likely in the same piece of Javascript that injected the
script tag. For example, JSONP returned from the server might look something
like:

{% highlight python %}
executeResponse({"some": "JSON", "data": "here"}). 
{% endhighlight %}

That piece of code is returned from your endpoint, and evaluated on the
client-side where you have previously defined the executeResponse method. It's
a little tricky, [Wikipedia](http://en.wikipedia.org/wiki/JSONP), [Dino
Esposito](http://www.devproconnections.com/article/aspnet2/ajax-cross-
domain-142169), and my partner [@joshowens](http://twitter.com/joshowens) are
all good resources on this subject.

That hurdle surpassed, I was able to build enough infrastructure to load some
appropriate HTML and Javascript from the ShareBelt server based on where the
visitor had come from, and capture an impression on the backend.

The bulk of that returned code is publicly-provided snippets for loading
Facebook and Twitter sharing widgets. So, at this point in the project, I
could render the belt and widgets, and people could do the sharing. But how
was I going to capture and record the actions that users took with these
third-party tools?

Luckily, this didn't turn out to be as much of a challenge as I expected. I
found that both Twitter and Facebook platforms provided a pretty full featured
system for triggering callbacks on events. Twitter's is called [Web
Intents](https://dev.twitter.com/docs/intents) and allowed me to trigger code
on a click of the Tweet button, and the Tweet itself. Facebook's Like and
Share callbacks I configured via the [FB.Event.subscribe()](http://developers.
facebook.com/docs/reference/javascript/) API. Read Facebook's [Javascript
SDK](http://developers.facebook.com/docs/reference/javascript) page to get
started with that.

What kind of code did I want to execute with these callbacks? More cross
domain Javascript requests, of course! Once you recognize the script injection
pattern, you see it everywhere. In fact, both of the publicly-available
Twitter and Facebook snippets are themselves using script injection to fetch
and render their sharing widgets ( and do who knows what else ).

## 48 Hours

Time did not turn out to be as big of a challenge as I expected with respect
to coding. I don't think my code is completely horrible, but I did take some
shortcuts. The scope of the minimum viable product we laid out were
manageable.

I did feel some urgency and so I didn't test drive. It felt ok, until it
didn't. At some point the discomfort of cowboy coding overtakes the speed
benefit you get from it. A little more than halfway through the competition,
about 10 o'clock on Saturday night, I spent my last couple of hours putting in
[Jasmine](http://pivotal.github.com/jasmine/) and refactoring the bulk of my
Javascript code into a classes that I could test. I learned things. It became
apparent from this experience why it's bad to access the document object
directly from within a class. I couldn't test the code in the absence of a
document object. Mocking and passing the document into the class broke
dependency, and allowed me to take control. It reminded me of what I like so
much about coding Javascript. Objects are so easily mockable and re-shapable.

![](http://media.tumblr.com/tumblr_mcfc9zDlms1r9fv8b.png)

Time was a factor in design and marketing.
[@nerdyllama](http://twitter.com/nerdyllama), our designer was not able to get
much direction from us, and was not able to iterate as quickly as we could. We
failed to effectively plan for setting up demonstrations of the product in
action, and we even failed to put Sharebelt on the Sharebelt landing page! Our
ranking definitely suffered as a result of poor planning in the presentation
of our application.

## New-to-Me Tools

I'm used to Heroku, but the Rumble was hosted on Linode, and we did deploys
with capistrano. We were using [Campfire](http://campfirenow.com/) to keep in
constant contact, and one of the first things Josh did was to set up
[capistro-campfire](https://github.com/technicalpickles/capistrano-campfire).
Whenever a team-member did a deploy, campfire posted a link to the git commit
so we could see the new code and diffs. With large teams or large projects,
this might be overkill. But in a small team working on small overlapping
pieces of code on a tight schedule it was really, really handy.

The other cool tool was the [NVD3](http://nvd3.com/) charting library. The
interface reminded me a lot of [Highcharts](http://www.highcharts.com), I
can't say I prefer one to the other. The series plotting made a little more
sense to me out of the box, and NVD3 might have a slight edge on the
presentation as well. I've been impressed by both, but if you've had any
issues with Highcharts give NVD3 a try.

## Rumble Regrets

As I mentioned before, a lack of a plan for presenting the product is my
biggest regret. We have a working application that touches on some pretty
interesting and relevant technologies, but we left it up to the judges to
figure out how to use it. The irony is that most of this most essential
planning work could have been done legally in the weeks leading up to the
competition. Live and learn!

## Conclusion

When the clock struck 8PM on Sunday night, I took a deep breath. The feeling
was surreal. The whole weekend had happened, I had learned a ton of new stuff,
and I had a pretty tangible product sitting in front of me. I had a strong
feeling of satisfaction and accomplishment. I would definitely do this again.

Rumble applications have a history of becoming real products. As for
[Sharebelt](http://sharebelt.com/), we plan to get it in front of people and
continue development. Is it going to make a million-dollars? I have no clue.
But thanks to the motivation and focus provided by the competition, it's more
than an idea, which is where 99% of products begin and end. Rumble on!

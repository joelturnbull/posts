---
layout: post
title: How we Cuke
tags:
- cucumber
- testing
---
![image](http://media.tumblr.com/8d35ec1a62451e7aef632cd087089e2a/tumblr_inlin
e_mlgepq1NjI1qz4rgp.png)

Two styles of step writing in Gherkin sparked a discussion about Cucumber
practices here in the office. [@mguterl](https://twitter.com/mguterl) has been
doing a lot of research, reflection, and has been taking a lot of good notes
from [The Cucumber Book](http://pragprog.com/book/hwcuc/the-cucumber-book). We
this in mind, we recently sat down as a group to attempt to define some
Gaslight specific Cucumber conventions and best practices.

Here are examples of the two styles that sparked the discussion:

{% highlight python %}
Scenario: User has questions
      Given I belong to a group "Sales Reps"
      And the group "Sales Reps" has a question
      When I login
      Then I should be prompted with the question for "Sales Reps"
{% endhighlight %}

and

{% highlight python %}
Scenario: User has questions
      Given I belong to a group with a question
      When I login
      Then I should be prompted with the question
{% endhighlight %}

The assumption is that each style better serves a certain contingent. The
first contingent being developers of the system. The other being the non-
technical clients or owners of the system.

The second example is customer facing. It's in plain english. It's easy to
read and reason about. However it prohibits reuse, and the steps cannot exist
in isolation. Implementers will need to introduce instance variables in order
to tie together the Given and Then steps. As a developer this is hard, our
tendency is eliminate coupling and always promote code reuse.

It follows from software design practice that steps written in this way create
more work for the developer. However, I the more I think about this, the more
I start to disagree.

## Why are we doing this?

The goal of any kind of acceptance testing should be to keep stake-holders
engaged with the project. It's a way for non-technical people to communicate
the functionality of the system in a practical way. It creates a document that
describes exactly what the system should do, and simultaneously verifies that
it does it.

Stake-holder involvement is the most important factor in a software project's
success. Therefore, every effort should be make to write Gherkin in a way that
communicates with the stake-holder and keeps them engaged. Don't bore the
stakeholders!

If there were no benefit to be gained from stake-holder involvement, there
would be absolutely no need for native Ruby speakers to write anything but
RSpec. The whole idea of acceptance testing, and the tools that are built
around it, exist to create this beneficial relationship. It only makes sense
to optimize that aspect.

Besides, is writing more step definitions really so painful to a developer? Is
it more painful than having a client who is unhappy or unable and unwilling to
communicate?

## What are we communicating?

The other problem with the first example is that it doesn't really communicate
to either contingent, especially if they are coming into the project cold.
"Sales Rep" is an implication. As such, there is another mental step to take
there. What does the role "Sales Rep" represent? Oh yeah, a role that provokes
a question on login. So just say that.

The tendency to write steps in the way of the first example is a result of
trying test too much through the UI.
[@too_mitch](https://twitter.com/too_mitch) gave the example of a signup form
with many required fields, each doing a validation that will prevent the
signup and display some kind of message. E.g. "Name is invalid", "Email is
invalid", "Username is invalid". The tendency is to write a step like:

{% highlight python %}
I fill in "email" with "invalid_email_address"
{% endhighlight %}

And then to reuse this step for each required field. However, this
proliferation of scenarios is going to simultaneously bore the consumer of the
feature, and slow down your test suite. The correct place to test these
validations is in RSpec unit tests. In Cucumber we should write:

{% highlight python %}
I fill in the form with invalid data 
{% endhighlight %}

Test that. Validate that a message appears, sign in is prohibited, and move
on!

Another way to quickly bore and confuse Gherkin consumers? Insert unnecessary
incidental details into a step definition. If we are testing authentication we
might write things like:

{% highlight python %}
When I log in as "dave@gmail.com" with password "l0ck3d!"
{% endhighlight %}

Because that's the data the login form accepts. But does it matter? Compare to
this:

{% highlight python %}
When I log in as "dave@gmail.com"
{% endhighlight %}

Clarity is important. See how many incidental details can be removed, while
retaining the scenario's meaning. Is it important who we are? Are we testing
anything about Dave specifically? How about this:

{% highlight python %}
When I log in
{% endhighlight %}

Every effort should be made to pare down steps. When in doubt go with plain
English. Get the scenario itself down the the magic number of three steps,
i.e. "Given", "When", "Then". Even the "Given" step can be extraneous.
Eliminate the incidental!

## Where should the programming be done?

[@mguterl](https://twitter.com/mguterl) likened a step definition to an
programming interface. We only need to have an idea of what the step does, we
don't care about the implementation ( how it is done ). But, because steps can
act like functions that pull and apply parameters, the trap is often to move
logic into them, and to use them like methods.

Cucumber is not a programming language. At every point the tendency should be
to move logic and variables into the implementation and out of the steps
themselves. Even the first scenario example introduces coupling between steps,
it just does it on the Gherkin side instead of the Ruby side. Even though
Cucumber supports it, do not call steps from steps in the implementation, call
Ruby methods from steps.

In Cucumber, a developer should resist the temptation to always reuse. Don't
always sift through the step definition files to locate the existing step that
already does the thing you are trying to do. One should write the step with
the language that makes sense for the scenario.

On the other hand don't fall into this trap:

{% highlight python %}
When I sign in
When I login
When I authenticate
{% endhighlight %}

This is bad. You still need to work towards ubiquitous language. There is no
increase of communication or context that rationalizes three different steps
for the same login action.

It's a balance thing. Remember, programming concepts are not our first
priority here. The first priority with Cucumber is communication. Sometimes
there are better ways to communicate the same functionality, depending on the
context. In these cases, it's best to avoid shoehorning the language of an
existing step into a feature, just for the sake of re-use.

## Is there a good way to organize step definitions?

Because you're dealing with a big mess of globally applicable step
definitions, it can be painful to locate any one implementation. Grepping
seemed to be my goto method for locating step definitions, but I just verified
that ctags in vim does the trick quite nicely. Still, it's never clear exactly
how to organize steps and features, or where to find them when you come onto a
project.

If there's a Gaslight convention, it's to organize step definitions by domain
object or concept, and features by user role. In this way, it's easier to
answer the question "what are the things this user can do"?

## What else?

Our favorite new pattern here at Gaslight is the Page Object. Page Objects are
a way to in step implementations to encapsulate step code into classes that
provide an interface for creating test objects and interacting with the UI.
The results are vastly simplified step definitions and better code
organization.

Page Objects are probably a topic that warrant their own blog post. Until then
I invite you to read up on them [here](http://www.cs.colorado.edu/~kena/classe
s/5828/s12/lectures/17-intermediatecucumber.pdf).

## Conclusion

Working with Cucumber and writing Gherkin are skills unto themselves, with
best practices that can seem antithetical to those that define good software
engineering. That is because the real goal of these tools is not testing.
Cucumber and Gherkin are about communication and stakeholder engagement, and
should be implemented with those goals in mind, first and foremost.

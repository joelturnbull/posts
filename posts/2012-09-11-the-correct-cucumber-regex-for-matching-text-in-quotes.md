---
layout: post
title: The Correct Cucumber Regex for Matching Text in Quotes
tags:
- cucumber
---
What's the correct regular expression to use for matching text within quotes
of a Cucumber step? I argue that It's not the one suggested by Cucumber
itself.

If I run a step missing step:

{% highlight python %}
When I click "Edit"
{% endhighlight %}

Cucumber will suggest:

{% highlight python %}
When /^I click "(.*?)"$/ do |arg1|
  pending # express the regexp above with the code you wish you had
end
{% endhighlight %}

The practice for most is to copy that suggestion into their step definition
file, and implement the step.

Often I've run into trouble when I've needed to create a slightly more
specific version of that step in a succeeding test:

{% highlight python %}
When I click "Edit" for "Jack"
{% endhighlight %}

Running the step, I'll get:

{% highlight python %}
no link or button Edit" for "Jack found (Capybara::ElementNotFound) 
{% endhighlight %}

The regex defined in our initial step is a valid match for this step,
unintentionally. The regex matches everything between a quote character, and
the last quote character in the string. In this case 'Edit" for "Jack'.

As proof, attempting to implement the new step using Cucumber's recommended
format yields:

{% highlight python %}
Ambiguous match of "I click "Edit" for "Jack"":
  features/step_definitions/running_stuff_steps.rb:12:in `/^I click "(.*?)"$/
  features/step_definitions/running_stuff_steps.rb:16:in `/^I click "(.*?)" for "(.*?)"$/
{% endhighlight %}

Both regexes are valid matches for that step.

So we need a regular expression to uniquely match each step. The problem regex
says "match everything between the first and last quote characters". We need
it to say "match text between a quote and the next quote, regardless of where
the end of the line is". Or, in regular expression language, not this:

{% highlight python %}
/^I click "(.*?)"$/
{% endhighlight %}

This:

{% highlight python %}
/^I click "([^"]*)"$/
{% endhighlight %}

This isn't rocket science, but I've witnessed a few smart people scratching
their heads on this one, not wanting to spend the mental energy to switch from
whatever feature they are working on to dissect regular expressions.

Is there any good reason to default to the former over the latter? Is a pull
request warranted?

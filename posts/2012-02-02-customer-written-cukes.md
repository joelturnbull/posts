---
layout: post
title: Customer Written Cukes
tags:
- cucumber
- rails
- rules engine
---
### …and why they're awesome. Or not.

On one of our more recent projects, we were given an entire suite of customer
written [Cucumber](http://cukes.info/) features. We've toyed with Cucumber in
the past, but this was the first time we've actually implemented Cukes on a
customer project. I thought I'd take a few minutes and explain some of the
completely awesome side effects as well as the downsides.

#### First: The awesome.

The cukes were very well written. The last time I experimented with Cucumber,
web steps were still in vogue and I was ready to start where I had left off.
Fortunately, [Chris Nelson](https://twitter.com/superchris) was there to keep
me from going down that path. He explained that the cukes were supposed to be
written at a much higher level and that the implementation of the steps were
better suited to handling the interactions with the specific inputs. It's a
good thing he did, too, or else I would have made a mess of steps that weren't
going to be solid or reusable.

Second, the cukes contained some business logic in [AST
tables](http://cukes.info/cucumber/api/ruby/latest/Cucumber/Ast/Table.html)
that turned out to be really super awesome when the time to implement the
logic came around. The logic revolved around meeting some criteria then
recommending a possible solution or enhancement.
[Chris](https://twitter.com/superchris) and
[James](https://twitter.com/st23am) were pairing on the project at the time
and James mentioned that the tables looked a lot like [decision
trees](http://en.wikipedia.org/wiki/Decision_tree) in disguise. The dynamic
duo spent some time looking at Ruby implementations of tree logic then decided
that the rules weren't quite as complex as the existing solutions and moved
ahead writing their own.

That turned out to be a really, really big win for this project.
[Chris](https://twitter.com/superchris) and [James
Smith](https://twitter.com/st23am) created a base class to handle the rule and
conditional attributes, then for each specific case, we create a subclass to
match the specific answers to the persistence layer.

**Sample feature, with table**
{% highlight python %}
Feature: Replace common areas lighting with high efficacy fixtures
  So that I know which energy efficiency improvements my property would benefit
  from, as a property owner/manager, I see a list of recommended measures for
  my property after completing the property questionnaire.

  Scenario Outline: Replace common areas lighting with high efficacy fixtures
    Given I have completed the property questionnaire
    And the predominant lighting type of permanently installed lighting fixtures in common areas is <lighting_type>
    And on average, the common area lighting fixtures were last replaced <last_replaced>
    And common area lighting fixture replacement <planned_in_scope> part of the planned retrofit scope

    When I view the recommended measures for the property
    Then the system shows that the "Replace common areas lighting with high efficacy fixtures" measure is <result>

  Examples:
    | planned_in_scope | lighting_type         | last_replaced | result          |
    | is               | LED                   |               | recommended     |
    | is               | Flourescent           |               | recommended     |
    | is               | Screw-in CFL          |               | recommended     |
    | is               | Screw-in incandescent |               | recommended     |
    | is               | Dont know            | >= 10 yrs ago | recommended     |
    | is               | Dont know            | < 10 yrs ago  | recommended     |
    | is not           | LED                   |               | not recommended |
    | is not           | Flourescent           |               | not recommended |
    | is not           | Screw-in CFL          |               | recommended     |
    | is not           | Screw-in incandescent |               | recommended     |
    | is not           | Dont know            | >= 10 yrs ago | recommended     |
    | is not           | Dont know            | < 10 yrs ago  | not recommended |
{% endhighlight %}

**The base class:**
{% highlight python %}
module RecommendedMeasure
  class Measure

    attr_reader :property

    def initialize(property)
      @property = property
    end

    def to_s
      "This is a sample. You should replace this."
    end

    private

    def build_rules(data)
      keys = data.shift
      keys.pop
      data.map do |datum|
        criteria = {}.with_indifferent_access
        result = datum.unshift
        keys.each_with_index {|key, index| criteria[key] = datum[index] }
        Rule.new(self, criteria, datum.last == "recommended")
      end
    end

    def rules_file
      self.class.name.underscore + ".rules"
    end

    def rules_text
      File.read(File.join(File.dirname(__FILE__), "..", rules_file))
    end

    def rules_data
      rules_data = Cucumber::Ast::Table.parse(rules_text, "", 0).raw
    end

    def match?
      self.build_rules(rules_data).each do |rule|
        return rule.result if rule.applies?
      end
      false
    end
  end
end
{% endhighlight %}

**A subclass:**
{% highlight python %}
module RecommendedMeasure
  class ReplaceToilets < Measure
    def to_s
      "Replace toilets"
    end

    def section
      "efficiency"
    end

    def gal_flush
      property.efficiency_responses[:toilet_gpf]
    end

    def last_replaced
      installed_before?(:toilet_year, 1992) ? "< 1992" : ">= 1992"
    end
  end
end
{% endhighlight %}

#### The not-so-awesome.

One of the negatives we've encountered is the cone of focus can sometimes
become too narrow. As a developer, heads down working on the cukes, some of
the details of the implementation can be missed. For example, a few of the
features were passing for me, but that's because I'd manipulated the state of
the data being tested in the step definition. I'd forgotten that when the
client was doing physical acceptance those actions wouldn't be available to
them. I had to implement a scenario to allow the client to change the data so
they could produce the correct result. It's a trivial step, but an important
one to remember nonetheless.

In this particular case, the test was that a program expired after a certain
date. We'd already constructed an administrative section of the application,
but because I made the changes programmatically in the cuke, my tests passed.
I delivered the feature for testing and got a message in Campfire a few
minutes later that made complete sense: I can't test this one because I can't
change the date!

### Cukes are a win!

Overall, I'm impressed with the process of developing features using customer
written cukes. Our client gave us an excellent starting point and it's
something we're trying to teach existing clients now as well. They aren't for
everyone, but when the client has domain knowledge and is able to convey that
in clearly written steps, it's a big, big win.

I've started using cukes in my pet projects as well. The steps are easy enough
to write and if you're already doing integration specs with something like
Capybara, it seems like a worthwhile step.

Another real benefit is the generated output.
[Relish](https://www.relishapp.com/) is a site for open source projects tested
with Cucumber to host their docs. Some of Cucumber's [own
documentation](https://www.relishapp.com/cucumber/cucumber/docs/tag-logic) is
generated with Cucumber! You can also find
[Rspec](https://www.relishapp.com/rspec) there as well.

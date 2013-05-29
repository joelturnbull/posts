---
layout: post
title: PaidSimply Lean Canvas
tags:
- lean canvas
- lean startups
- paidsimply
- intrapreneurism
- problem
- solution
- key activities
- value proposition
- unfair advantage
- channels
- customer segments
- cost structure
- revenue streams
---
## The Challenge

Last month’s meeting of the [Cincinnati Lean Startup
Circle](http://www.meetup.com/Cincinnati-Lean-Startup-Circle/) was probably my
favorite thus far. The turn out was larger than normal but the reason for my
enthusiasm was the level of engagement of the entrepreneurs who showed up.
There were many interesting ideas discussed openly which was refreshing.

For me, a highlight of the meeting was the announcement of [Ryan
Walker](https://twitter.com/rywalker)’s newest venture,
[Engagement.io](http://engagement.io/), a product intended “to help busy app
owners or app developers get a system in place to not only track the
engagement level of each of their users, but provide a tool to react to user
activity and inactivity.” Better still, Ryan intends to blog weekly about his
progress. If you’re interested in reading along simply visit [](http://engagem
ent.io/)[](http://engagement.io/)[[http://engagement.io/](http://engagement.io
/)](http://engagement.io/).

## Challenge Accepted

For the past year I’ve wanted to do the same with my pet project,
[BudgetSketch](http://budgetsketch.com/), but <insert favorite excuse here>.
Recently at Gaslight we’ve begun to formalize how we’re building internal
projects. The team didn’t hesitate a moment when I asked permission to blog
about the customer development process we’re following on our newest product,
[PaidSimply](https://app.paidsimply.com/). So I’m happy to be matching Ryan’s
commitment and intend to blog weekly about our progress with PaidSimply.

## Intrapreneurism at Gaslight

Gaslight was founded as a consultancy specializing in custom application
development. We frequently write software for internal use and occasionally
this software exhibits signs of external value, AKA “a business opportunity.”
We’ve been working to diversify our revenue streams and adding products to the
mix meets that goal nicely. The challenge will be to grow the products branch
of our company without negatively impacting our consulting business. Thus far,
when there have been conflicts (mostly scheduling) we have erred on the side
of favoring consulting over product development.

To truly be lean we’ll have to discover a means of increasing our velocity or
“rate of learning” if you will. The plan is to secure commitment to product
development within Gaslight by presenting a clear path to sustainability. [Ash
Maurya](http://www.ashmaurya.com/)’s [Lean
Canvas](http://www.ashmaurya.com/2012/02/why-lean-canvas/) approach fits that
need very nicely. We selected the Lean Canvas over Alex Osterwalder’s original
[Business Model Canvas](http://www.businessmodelgeneration.com/canvas) because
we felt that the former is well suited to early stage startups and the latter
better suited to those in later stages. It will be a simple step to move from
the Lean Canvas to the Business Model Canvas for those products that reach
maturity.

## PaidSimply

Like many consultancies we invoice clients frequently via
[Freshbooks](http://www.freshbooks.com/) but we and our clients were feeling
some pain regarding collecting on those invoices. Chief among these was that
clients wishing to pay via credit card were forced to do so via PayPal which
currently caps single transactions at $10,000. With minimal effort we felt we
could significantly reduce that pain with [Stripe](https://stripe.com/), a
service that allows us to collect payment via credit and debit card without
transaction limits.

To test our solution we wrote a simple app that ties Freshbooks and Stripe
together using their APIs such that once payment is received via Stripe the
associated client invoice is marked as paid within Freshbooks. Not only did
the test succeed in significantly reducing our pain but several clients who
paid invoices using our solution mentioned their interest in using it for
their invoicing needs. [PaidSimply](https://app.paidsimply.com/) was born.

## PaidSimply’s Lean Canvas

Following the process detailed in Ash’s book, [Running
Lean](http://www.runningleanhq.com/) we have documented our assumptions on
PaidSimply’s Lean Canvas and present it below for your feedback in the
comments below. We have thoughts as to the riskiest parts of the canvas but
we’re eager to hear unbiased thoughts from our readership. So we won’t mention
them at this point but by all means let us know what you think by commenting
below.

[![PaidSimply Lean Canvas for July 24,
2012](https://dl.dropbox.com/u/1275806/gaslight/paidsimply-wiki/lean-
canvas-20120724.png)](https://dl.dropbox.com/u/1275806/gaslight/paidsimply-
wiki/lean-canvas-20120724.png)

## Next Steps

As mentioned, we plan to blog weekly about our experiences building
PaidSimply. Our goals for this week include the following:

  * launch the landing page for beta [signups](https://app.paidsimply.com/) (which should be completed if you’re reading this!)
  * run a Google AdWords campaign to recruit 10 beta users outside our sphere of contacts
  * walk through the existing app and write stories (using [Cucumber](http://cukes.info/)) to document an MVP for use by our beta users

We felt we could easily sign up 10 beta users from our clients and their
contacts but felt we’d benefit from having users from diverse business
domains. The Cucmber stories will be used not only to develop the app but to
demonstrate to the Gaslight team that the MVP is achievable and capable of
generating revenue quickly (the best form of validation!). This should
increase buy-in by the team and secure the thumbs up needed to proceed with
the project, or this will be a very short series of blog posts! The plan is to
have all the steps above completed by next weeks blog post. Stay tuned.

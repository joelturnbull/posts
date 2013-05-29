---
layout: post
title: The Web is a Hypermedia API
tags:
- api
---
If you’re struggling to wrap your head around Hypermedia APIs, the first thing
you need to understand is that you actively use one every day. Unless you were
just handed a printout of this article, you are using one right now.

### The Twilight Zone

Imagine that you live in an alternate dimension. You want to do some online
shopping, and you have just received the latest Amazon.com client. You fire it
up and do a product search for “Three’s Company DVDs”. All of the product data
comes down the wire. We have the names, brands, and prices of the twenty most
relevent “Three’s Company” related products. After the client gets the data,
it goes to work! It builds all of the urls and applies them onto all of the
product data that came down, so that you can click on the DVD set that you
like. Wait, no! The client doesn’t do that!

To anybody with a passing amount of knowledge about how to build a website,
the situation above seems ridiculous. In this dimension, we have a web browser
that knows how to read a representation of data called HTML, and render and
activate the links that the server provides in that document. The server
defines where the resources reside, so naturally, the server provides the
links to those resources.

### We Didn’t Know Any Better

My alternate universe attempted to describe a scenario in which the web that
we know and love operates like an API that I designed last year. I think a lot
of people design APIs in this way, i.e. they do what they need to do right
now. They provide some locations on their server that allow a third party to
read or write to some resources. That doesn’t sound so bad, right? An API
totally needs to do those things, but it needs to do a little bit more. APIs
are by definition publicly available, they invite themselves into a realm of
of other people’s stuff that you no longer have total control over. As such,
some forethought needs to be given to design up front. APIs that only provide
the resources, and not the locations to next resource, are taking the short
view and are introducing big problems for the future evolution of your system.

The fatal flaw will smack you soon enough. When you rolled out the API, you
promised all of the people, in your meticulously crafted documentation, that
the resource they want is available at location x. They, in turn, built
clients around that promise, and now you badly, badly need to change that
location to y. Go ahead and update that documentation and see how much it
helps. Best case scenario is that you built the client, and now you have to
get everybody to update it before you can get back to doing anything
productive on your end. To quote [Steve
Klabnik](https://twitter.com/#!/steveklabnik), “People are really good at
screwing themselves over in the long term”.

### Robots Like to Surf The Web Too

So what’s the solution? Let’s bring it back to the web to understand how we
can address this. Grandma has a home page with a link people can click on to
see pictures of all of her perfect grandchildren. Six months later Grandma
finds a cheaper site for hosting those pictures and wants to put them there
instead. As Grandma’s faithful webmaster, you update the link on her homepage
to point to the new photo sharing site. Does the whole world need to update
their web client now? Of course not, the link to the next resource is defined
and provided by the server in the HTML it sends down, and that link just
changed. I think the best short description of an API I’ve found is in Ryan
Tomako’s article [“How I Explained REST to My
wife”](http://tomayko.com/writings/rest-to-my-wife). Paraphrasing, an API is
an interface for machines to use the web just like people do. APIs that don’t
employ hypermedia are basically jerks that are saying to clients, “I know
where to go next, but I’m going to make you guess how to get there anyway.”
With a Hypermedia API you force the client to know the location of exactly one
resource, “Home”.

So applying Hypermedia to APIs provides solutions, but also really transforms
the very nature of the interaction into something much cooler. APIs whose
resources link to other resources become “engines of application state”. Every
resource exposes it’s current state, and the paths that you can take to
create, interact with, and shape it. When an API describes to you how to
interact with it, clients can be designed in a static way, reactively handling
changes from the server.

### Hypermedia APIs in Practice

So how do you go about acheiveing this? Unfortunatley, I have little practical
experience to share. I understand a few things. People who talk about REST,
talk about nouns and verbs. All APIs provide the nouns, Hypermedia APIs
provide the verbs too. In addition to the data the APIs traditionally provide,
at every step of the way they need to describe, with links, 1) where to start
2) where you are, and 3) where you can go next.

There are definitely some best practices around things like content negotation
and media types that you can read up on, but I gather from [Chris
Moore](https://twitter.com/#!/cdmwebs) that it’s still the Wild West for
Hypermedia design. Apparently making more effective use of HTTP Headers is
good design, and you want to give some thought about your content format,
because types like JSON and XML are not implicitly hypermedia aware, i.e. they
have no baked in concept of links.

### Acknowledgments

While I’m talking about what I do know, maybe it’s a good time to mention
where I learned it. The impetus for all of this was [this
talk](https://vimeo.com/44520801) given by Steve Klabnik at the most recent
[Cincy.rb](http://cincyrb.com/) meetup, just a couple weeks ago. Steve’s done
a ton of writing and speaking around this subject, just google him or pick up
his evolving book, [“Designing Hypermedia
APIs”](http://designinghypermediaapis.com/). The talk is an overview of what
Hypermedia APIs are, why they’re important, and their philosophical
implications. Thankfully, Steve gives us permissions to stop worrying and
fighting about REST and focus on cool stuff. For the Rails folks, Steve
recommends two gems, [active_model_serializers](https://github.com/josevalim/a
ctive_model_serializers/) and [rails-api](https://github.com/spastorino/rails-
api)

I also highly recommend this [video presentation](https://vimeo.com/20781278)
by Jon Moore, in which he actually demos a static client. If this link between
Hypermedia APIs and the web as we know it is still nebulous, this video will
solidify it for you. I really like the way Jon explains things and boils down
the dissertation language, translating a block of Fielding into “You do stuff
by reading pages and then either following links or submitting forms.”.

For some deep reading, I’ve heard people recommend [“Building Hypermedia APIs
with HTML5 and Node”](http://www.amazon.com/Building-Hypermedia-APIs-
HTML5-Node/dp/1449306578). Don’t worry if you’re not a node programmer. I also
want to give a shout to [Chris Moore](https://twitter.com/#!/cdmwebs/), who’s
been our resident Hypermedia API pusher, and [Sean
Allen](https://twitter.com/#!/SeanTAllen), who’s been harping to me about REST
since before harping about REST was cool.

### Conclusion

If you need inspiration in understanding hypermedia APIs, look no further than
the web. The architecture and protocols for the web were designed by really
smart guys who already figured this stuff out a long time ago and can be
applied universally. Once I started viewing APIs through the lense of the web
I know and love, it actually seems completely wrong to design one in any other
way.

# Why not Pivotal Tracker?

We've been using Pivotal Tracker for a very long time. At least once
we've made a serious attempt to switch to something else. The time has
come for another attempt. Our goal is to provide a more complete,
transparent, and malleable insight into our process with better
metrics. That's a lot in one sentence, so let's take a look at what
that really means.

First, I think Tracker was originally envisioned as a tool by
consulting developers trying to get clients to understand agile. It
makes a lot of assumptions about how to do that. Originally, I was
attracted to the simplicity of ticket states and the obviousness of
how to move through them. You brainstorm into the ice box. Prioritize
stories into the backlog. Estimate each story. Start a story. Finish a
story. Deliver a story. Then the customer can either accept or reject
a story. Before Tracker I was using Bugzilla. The simplicity of
Tracker was genius.

A significant portion of work no longer follows this simple workflow.
I might guess about half of our stories deviate somewhere. First,
ideas in the ice box need more work before they can be prioritized for
development. We go through several iterations of wireframes and
planning before the story can be developed. How can we track the
progress of this work in Tracker? There are several workarounds, but
that's what they are. Ultimately the only way to track this is to
deviate from what Tracker was designed to do.

Likewise, once development has begun there is a back and forth between
the developers and designers on many stories. User facing stories need
the UI built while the functionality is being built. Finally, design
should review all user facing work before it's delivered to the
customer. How do you track this in Tracker? There are not enough states.

We can summarize this by saying Pivotal Tracker doesn't not provide a
smooth integration between UX, UI, and developers. Our process more
complicated than the five state machine Tracker gives us. Often each
project has different complexities in the process based on the context
of the domain, the client's capabilities and infrastructure, and the
team we have working on it.

Second, Pivotal Tracker is heavily vested in velocity. What is
velocity? In Tracker it is a moving average of the total estimated
stories accepted in an iteration. The problem here is in the
estimating. I used to think I was terrible at estimating. I used to
think if I just were smarter or more experienced or tracked things
better I'd be able to estimate more accurately. I now believe
estimating is nearly impossible. There's only a very small number of
cases when we can actually estimate. There's been a wealth of
information coming out lately from well-known sources validating this.

If velocity is based on estimation; and estimation in inherently
flawed; what does that mean? Pivotal Tracker is constantly calculating
velocity. Each past iteration has it's velocity shown on the iteration
header. The page header boldly shows the current velocity. The backlog
is broken up into estimated iterations based on current velocity. But
that velocity is flawed. The estimated iteration boundaries are a
distraction. We have to regularly have conversations with customers
about velocity.

The idea is good. It's trying to give the customer an idea when
features might be finished and the ability to do long range planning.
But if short term estimates are problematic, long term estimates are
even more so. We need better metrics to better plan.

Lastly, Pivotal Tracker is obviously project based. This may be a
problem that's unique to our niche industry, but maybe not. We run
several moderately sized projects concurrently. It is difficult for
any one individual to understand what's going on across these multiple
projects. It's difficult for one person to see where they might be
able to step in and help when they have slack time. It's also
difficult for leadership to easily see where holes and bottlenecks
occur. We would like to see some global board that shows us the state
of each project and where to better get our team members working
together.

This is a fairly long laundry list of things wrong with Pivotal
Tracker. It's doubtful we could have gotten to where we are as
Gaslight without Pivotal Tracker. Without Tracker we would have had to
use some other tool that had more baggage to work around. Tracker has
been the simplest thing to do agile development for the last five
years. However, these are problems we've found over the last year or
so, as we've been trying to integrate more design into our development.

Our intention is to replace Tracker with some form of Kanban. David
Anderson's book, Kanban, is an easy read that highlights how, as a
methodology, it can address all of these concerns. Exactly how we end
up implementing Kanban isn't quite clear. In the coming weeks it
should become more clear and will warrant more writing. Stay tuned!

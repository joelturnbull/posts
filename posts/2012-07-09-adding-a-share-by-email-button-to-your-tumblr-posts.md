---
layout: post
title: ! 'Adding a Share by Email Button to Your Tumblr Posts '
tags:
- tools
- tumblr
- social media
---
Apparently email is still a valid way of sharing things. It may still be the
best way of sharing something with one person. Every social media app has an
api for "share" buttons, but there doesn't seem to be a whole lot of code out
there to steal in the way of email sharing buttons. So if you have a Tumblr
blog like this one, and can run some Ruby, steal this script if you like:

  

[https://gist.github.com/3074150.js?file=generate_email_button_html.rb](https:
//gist.github.com/3074150.js?file=generate_email_button_html.rb)

  

The output is html that you can paste into your custom Tumblr theme. An anchor
tag, with a mailto action, that wraps a button image. You'll need to replace
the value of "image_url" in the script ( the one here is locked down by s3 ).
The rest may work out just fine for you.

The email copy is uri-encoded and passed as the value to the "body" parameter
in the mailto query string. The interface to the mailto scheme is pretty
limited. I quit in my attempt to get any multipart html/text email stuff going
on. The RFC states "The mailto URL is primarily intended for generation of
short text messages that are actually the content of automatic processing
(such as "subscribe" messages for mailing lists), not general MIME bodies."
So, yeah.

I'm also pretty aware that I'm hacking the mailto scheme by putting a space
where the "to" value should go. The idea is that an email client will pop up
in a draft mode with the "to" field empty, so that the article sharer can fill
it in. I got the result I wanted, but the execution seems shady.

Leave comments. I'm interested in better ways to do this, or ideas around how
to acheive richer email messages from a share by email button.

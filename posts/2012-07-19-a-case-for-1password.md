---
layout: post
title: A Case for 1Password
tags: 
---
![](http://media.tumblr.com/tumblr_m7euvjwG8Q1r9fv8b.png)We had a discussion
this morning about password management and what is secure. I felt compelled to
write something about what I believe to be the best mix of simplicity and
security. In short, use [1Password](https://agilebits.com/onepassword).

So the primary argument against 1Password is that it's not secure enough. The
main alternative presented was having a small handful of passwords memorized.
Each password is of differing levels of security: one easy to remember and
type for sites you don't care about progressing up to one very secure password
you only use on a handful of sites.

There are two problems with this strategy that I can see. First, sharing
passwords between sites is a bad idea. All sites will eventually have their
password databases stolen. Any other assumption feels too risky to me. The
trick is that organizations are not required to divulge when they have
security breaches. You could have your super-strong password cracked at your
bank and not know it. So you don't know your other accounts are vulnerable
until the site discloses.

So when your bank's password db is stolen you're going to have to go change
your password on all the other sites that share your super-strong password.
Here's the second problem: you may not remember which sites are using this
super-strong password.

So how secure is 1Password? Agile Bits has written a nice article on the
[Agile Keychain
Design](http://help.agilebits.com/1Password3/agile_keychain_design.html) that
describes all their security decisions. The short of it is 1Password is using
AES-128 as implemented open source by Apple in their OpenSSL CommonCrypto
library.

After doing a little research (which means reading the [Wikipedia Page on
AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard)) I do not
believe there are any known direct breaks on AES that are realistically
feasible. Of course, this makes me sound like a security expert, which I am
not. There are several known attacks, but the attacks still require a crazy
amount of time.

The real risk of attacks come from "side-channels", which means attacking the
way the algorithm is implemented in software. There have been several
successful attacks against AES side-channels. However, since the attacks are
against the implementation and not the algorithm, libraries are able to patch
security releases to mitigate if not alleviate the risk.

What does 1Password buy you over other password management strategies? In
short, your list of passwords and which sites you use them on is available on
all of your devices: Mac desktop, iPhone, iPad, even Windows desktop and
Android. By storing your encrypted keychain in Dropbox, you can painlessly
sync your passwords across all of these devices. Having a single, very secure
password to access the keychain provides solid security in a convenient
format.

This will allow you to generate unique, secure passwords for each site you
visit. All of the sites that you feel require a secure password can have one
that is unique. If you choose to share weaker passwords between low-risk
sites, 1Password will also allow you to easily search how many sites are using
that shared password.

I guess in the end, I don't really care how you choose to manage your
passwords. If you feel what you're doing is secure enough for your risk
tolerance, then I have nothing to add. The point of this post is that I find
1Password to be both convenient and secure. If you're not satisfied with your
password management strategy I suggest you give it a try.

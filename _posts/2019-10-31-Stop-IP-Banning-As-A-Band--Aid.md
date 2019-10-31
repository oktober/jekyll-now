---
layout: post
title: Stop IP Banning As A Band-Aid
categories: [Cybersecurity]
description: Starbucks kept telling me my username and password were incorrect. But they were correct... Why couldn't I access my account and what was Starbucks doing behind the scenes?
---

Let me tell you a little story.

I received a Starbucks gift card. I enjoy coffee so it’s nice to receive a gift card for it. I opened their mobile app to add the gift card. And that's when my frustrations began.

<!--more-->

You can’t add gift cards in their mobile app. Fine. Annoying, but fine. I can deal with that.

So I walked over to my computer and pulled up their website. I entered my username and password. What I get back is an error message - my username or password is incorrect.

![Screenshot of error saying 'Sign in unsuccessful. The email or password you entered is not valid. Please try again.'](/images/sign-in-error.jpg)

I could log in with my username and password on the mobile app. Why wasn’t it working here?

I work with computers. I know weird things happen sometimes, so I tried again. Same error.

Okay... I reset my password. Then I entered my username again and the new password. Nope, same error.

Now I've started to think something isn't right. **I know I’ve entered the correct username and password, so what’s going on?**

Luckily, I have Burp Suite which lets me watch the data being set to Starbucks’ website and the data I’m getting back. I pulled it up and got it running. I entered my username and password again and submit. 

Everything looked correct. So that wasn’t the problem - and that error message was both confusing and unhelpful.

**Next, I looked at the response coming back from Starbucks’ server. It was a 403 Forbidden response.** That means I didn’t have permission to access my account page. Weird, right? 

What’s more, it says my “fingerprint” or "reputation" was denied.

![Screenshot of JSON response from server showing {"type":"reputation","messageId":"requestDeniedError"}](/images/request-denied-error.jpg)

What’s a fingerprint (or reputation), you might ask? It’s when the company compiles a couple of different pieces of information about your account and system. They might use your IP address, your specific browser and version number, browser plugins you have installed, and other info. They compile this into a mathematical representation of you they call a “fingerprint”. It’s a way of tracking you across the web and another tool companies use to identify you as the legitimate owner of the account.

Whatever information Starbucks was gathering on me, then compiling into my “fingerprint” or "reputation", was flagged as being banned from accessing account information - whether the account had the correct username and password or not.

**Here’s my hypothesis:**

Starbucks is having a problem with malicious hackers brute forcing their log in page (*trying numerous combinations of passwords to try and “guess” the correct one*) and/or credential stuffing (*taking legitimate usernames and passwords from a data breach, then using them at other websites*). This was becoming a thorn in Starbucks’ side, so they started banning IPs.

Why do I think this?

Because I use a VPN. I was using a VPN when I was denied access.

What’s a VPN - you might ask? It’s a private network created between me and my VPN provider. Instead of my web requests going directly to the websites, they’re first sent to my VPN Provider. From there, they make a request to the website for me and pass the data between us. It can help provide security over public or untrusted networks and can prevent websites from gleaning my location. VPNs have security risks of their own, but they come in handy.

So, being connected to this specific VPN server was a problem. **I switched to a different VPN server.** Then I logged into my Starbucks account, again.

**Problem solved.** No errors or anything weird going on. I accessed my account and added my gift card.

**What a pain in the ass.** First, the gift card was only $5 - so not worth all that work. But how confusing was this entire thing? **The error message was misleading.** There was no information telling me what was going on. **And I’m a legitimate user, not a malicious hacker, just trying to access my account.**

***And I was able to easily get around their IP ban.***

If I was able to do it so easily, what’s to stop malicious hackers? It might buy Starbucks a tiny bit of time. But once a malicious hacker gets wind of what’s going on, they’ll switch to a different VPN server, or even provider, and continue their attacks.

**IP banning is a heavy-handed way of delaying the problem without actually fixing it.**

Instead, **why not implement 2FA?** I, for one, would LOVE to have that on my Starbucks account. Or maybe just limit log in attempts? When users first sign up, test their password security strength and require a minimum level of security and/or check their password against known data breaches. [NIST shares some great advice](https://pages.nist.gov/800-63-3/sp800-63b.html){:target="_blank"} on this very topic.

But IP banning is just sloppy. And it’s ineffective.

I see this used as a “solution” to many WAF (*Web Application Firewalls*) out there too. Yes, you’ll probably prevent some malicious bots from accessing your site, but only for a while. Eventually those IPs will be tossed aside. Once they are, they’re available for reuse. These may now identify legitimate users who would otherwise want to innocently view your website.

We need a better way.

**Moral of the story - _don’t use IP banning_**. At best, it’s a band-aid solution. At worst, it blocks legitimate customers from using your services. **And make error messages appropriate**. They don’t have to reveal any sensitive information, but don’t make them outrightly misleading. That’s just lazy coding.

And yes, I did finally get my coffee. Thanks for asking.

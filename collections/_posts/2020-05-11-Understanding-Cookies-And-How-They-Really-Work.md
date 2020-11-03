---
layout: post
title: Understanding Cookies and How They Really Work
tags: tech cybersecurity featured
description: Cookies are complicated, but still commonly used for keeping users logged in to a site. Learn a little more about the nuances behind setting cookies and what you're really doing when you set that attribute.
thumbnail: /post-thumbnails/understanding-cookies-and-how-they-really-work-thumbnail.jpg
thumbnail-creator-link: https://unsplash.com/@commonboxturtle
thumbnail-creator: Conor Brown
thumbnail-site: https://unsplash.com/
thumbnail-site-name: Unsplash
image-alt: Hand with black gloves pushing a caramel into a warm cookie
---
A cookie is a piece of information that allows the web server to create some sort of “connection” with individual users.

To understand cookies, first we must understand something vital about HTTP - it’s a stateless protocol. The browser asks for data with an HTTP Request and as soon as the web server returns with an HTTP Response, it’s done. It immediately forgets who that browser (and who that user) was. It acts like it has amnesia - “I’ve never seen that browser before in my life!”

This stateless protocol is not useful if you want a shopping cart or to have users “logged in”. So, cookies were developed.

<!--more-->

If the web server wants a user to maintain a connection to their site (like being “logged in”) , it will send a cookie in the HTTP Response (using the Set-Cookie header). The browser saves that cookie for as long as the web server says it should (or less if the browser needs that space for memory or just thinks the web server is being a little too greedy with the date). Now every time the browser needs data from that web server, it sends the cookie back with every HTTP Request. 

When the web server gets that cookie, it knows who this user is, to keep them “logged in”, and to show them their shopping cart.

Essentially, this is what happens:

  1. Browser sends an HTTP Request to a website you’ve just logged in to
  2. Web server responds with the web page and includes a Set-Cookie in the HTTP Header, like this:

    HTTP/1.0 200 OK
    Content-type: text/html
    Set-cookie: theme=light
    
  3. Browser creates a file for that cookie with those parameters
  4. Every time you visit that website from now on, until the cookie expires, the browser will send the cookie in the header of the HTTP Request, like this:

    GET /index.html HTTP/1.1
    Host: staciefarmer.com
    Cookie: theme=light



It’s an imperfect system, but it works.

## Cookie Defaults
If you’re a developer who needs to set cookies for users, you need to know how cookies behave by default.

If  you have no attributes, and simply set a cookie, it will:

  - Only be stored during that session (usually this is defined as when the browser is closed)
  - Only be sent when the user is visiting that specific domain (e.g. not a subdomain)
  - Only be sent when the user is visiting a page within that path (e.g. / and anything within root such as /blog, /account, etc)

If you want to change these defaults, the web server needs to set some attributes with the cookie.


## Cookie Attributes
Let’s go through them.

### Expires
    Set-Cookie: cookieName=cookieValue; Expires=Wed, Jan 31 2021 22:23:01 GMT;

The Expires attribute tells the browser, “Instead of deleting this cookie at the end of the session, but please store it until this date and time”. 

The server can ask the browser to store the cookie until that date, but ultimately it’s up to the browser. In general, the browser will keep the cookie until this date, but if the browser needs to free up some space or it thinks the Expires date is way too generous, it might get rid of it - so be realistic when setting your Expires date.

### Max-Age
    Set-Cookie: cookieName=cookieValue; Max-Age=300;

The Max-Age attribute tells the browser how many seconds to please keep the cookie before deleting it. Like before, the browser can delete the cookie before this time to free up memory or if it doesn’t think the cookie should be kept that long.

Some older browser didn’t acknowledge the Max-Age attribute. **If you are dealing with backwards-compatibility, most developers will set BOTH the Expires attribute and the Max-Age attribute.** 

With these two, it’s an either/or situation. Whichever one you set (either Expires or Max-Age), the browser will use. If you set both, the browser will use the Max-Age attribute and ignore the Expires attribute. 

As a general guideline, just use the Max-Age attribute unless you need both for backwards-compatibility.

### Domain
    Set-Cookie: cookieName=cookieValue; Domain=staciefarmer.com;

The Domain attribute tells the browser what domain (server) to send this cookie back to when making an HTTP Request. 

For example, if the Domain attribute is not set and the browser received this cookie from staciefarmer.com, it will only attach this cookie to HTTP Requests for staciefarmer.com. It won’t attach the cookie to a request for blog.staciefarmer.com or any other subdomain.

However, if the cookie was sent from blog.staciefarmer.com, then it will only be attached to requests for blog.staciefarmer.com. It won’t be attached to requests to staciefarmer.com, attack.staciefarmer.com, or even articles.blog.staciefarmer.com.

**By using the default behavior of the domain (e.g. not setting the attribute), *cookies are more secure*.** They will only be sent with requests to the original domain that set them, no one else.

But what happens when you set the Domain attribute for a cookie?

Let’s say I set the attribute to "Domain=staciefarmer.com"*. Now any request to staciefarmer.com or ANY of its subdomains (such as blog.staciefarmer.com, attack.staciefarmer.com, etc) will get this cookie attached. It is immediately less secure.

**Setting the attribute as domain=staciefarmer.com or domain=.staciefarmer.com is considered identical and acts the same.*

*To be clear, you cannot set the domain for another website. I cannot set my cookies to be "Domain=Yahoo.com" and they can’t do that for my site. The browser will just ignore the Set-Cookie request.*

The Domain attribute can also be used for subdomains. I could set the attribute to be "Domain=blog.staciefarmer.com" and the cookie won’t be sent to requests for staciefarmer.com. It will, however, be sent to any subdomains of blog.staciefarmer.com such as attack.blog.staciefarmer.com, lotsa.subdomains.blog.staciefarmer.com, etc. 

You still have the same problem before and how well do you trust your subdomains?

In general, you should NOT trust your subdomains completely and you should NOT set the Domain attribute of your cookies, if you can avoid it. The default behavior in this case is more secure.

What if I need to use it? Can I set the domain from a subdomain to a higher level domain? Yep.

For example, let’s say you have a login.example.com and you want to set a cookie for the entire site (example.com). You can do this. Subdomains can set cookies for their own domain or any domain higher. If login.example.com sets the cookie to be "Domain=example.com", now the cookie will be shared with login.example.com, example.com, and any other subdomains for it (like blog.example.com, cart.example.com, etc).

Maybe that’s something you want to do, so it’s good to know how the scope and hierarchy works. 

**In general though, it’s best to stick with the default behavior when it comes to the cookie domain and avoid using this attribute, if you can.**

### Path
    Set-Cookie: cookieName=cookieValue; Path=/blog/;

Much like the Domain attribute, the default behavior of the cookie Path is more secure.

How does it work?

The Path attribute tells the browser to only send the cookie when making an HTTP Request to this directory or its subdirectories. For example, a cookie declared without a Path will only be sent to exactly the path that created it, and its subdirectories. If a cookie is sent from staciefarmer.com, the path will default to / (root). It will then be sent to any path under staciefarmer.com/ such as staciefarmer.com/blog, staciefarmer.com/testing, etc.

If a cookie is sent from staciefarmer.com/blog/, the Path will default to blog/. Now, it will be sent to any path under blog/, such as staciefarmer.com/blog/cybersecurity, staciefarmer.com/blog/musings - but it won’t be sent to any other path not under blog. For example, it WON’T be sent to staciefarmer.com/ or staciefarmer.com/about. 

Here’s the kicker, to include the cookie with every path under the current one, you must include the ending /. For example, if the cookie is sent from staciefarmer.com/blog (notice no ending forward slash), the path will default to / (root). That’s because this is not considered a “directory” unless it has the ending forward slash (/).

Okay, that’s all the default behavior. What about using the Path attribute? What does that do?

Really, the only benefit with Path is you can set it to be different than the default.

For example, if the cookie is sent from staciefarmer.com/blog, I can set the "Path=/about/". The cookie will only be sent to staciefarmer.com/about/ and its subdirectories (such as staciefarmer.com/about/me/, staciefarmer.com/about/allofyou/, etc.)

Except…(there’s always an exception, right?) this can be worked around using iframes.

Let’s say the cookie is sent from staciefarmer.com/blog/ and the Path is specified as “/blog/“ ("Path=/blog/").

I, as an attacker, control staciefarmer.com/attack/ and I want to get that cookie. I create a hidden iframe in my page that loads staciefarmer.com/blog/. Now, I can read the “/blog/“ cookie and do nefarious things with it.

Point being, don’t use the Path attribute for security. Know how it works, including its quirks, but don’t rely on this attribute at all to keep your cookies secure.

### Secure
    Set-Cookie: cookieName=cookieValue; Secure;

The Secure attribute tells the browser to only send this cookie over a secure connection - usually meaning HTTPS. 

Unlike the last two, this is a good attribute to set. 

Overall, you should be using HTTPS everywhere on your website and you should only send cookies over a secure connection. As a precaution, you should also set the Secure attribute for all your cookies so they will ONLY be sent over HTTPS.

Even with all that, a cookie with the Secure attribute is not 100% safe. Nothing ever is.

Keep in mind this advice from the [MDN  Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie){:target="_blank"}:

> *“However, confidential information should never be stored in HTTP Cookies, as the entire mechanism is inherently insecure and doesn't encrypt any information.”*

While we try to ensure encryption by setting the Secure attribute, **confidential information should never be stored in cookies**. Instead, store anonymized data such as large, random nonces, and set realistic expiration dates - all while setting the Secure attribute on your cookies and using HTTPS everywhere.

### HttpOnly
    Set-Cookie: cookieName=cookieValue; HttpOnly;

The HttpOnly attribute is very useful and, like the Secure attribute, should be used. 

This attribute ONLY allows the cookie to be accessed through an HTTP Request, meaning it CAN’T be accessed using Javascript.

This is a great defense against XSS attacks (specifically for stealing or modifying cookies) and the HttpOnly attribute should be used on your cookies.

### SameSite
    Set-Cookie: cookieName=cookieValue; SameSite=Strict;

SameSite is relatively new, but it is supported on all major browsers now. 

This attribute has 3 options:

-	**None** - meaning it’s disabled
-	**Lax** - (default for most browsers) meaning it won’t send cookies for resources (like images) but it will send cookies when a user follows a link to a different site
-	**Strict** - Preferred setting, meaning it won’t send cookies if the HTTP Request originated from a different website (e.g. if the user clicks a link on example.com’s site to go to staciefarmer.com, staciefarmer.com cookies will not be sent in that HTTP Request)

Why not use Strict all the time? It would be great, but it’s kinda inconvenient.

Let's say I’m logged into Amazon and their login cookie has the attribute "SameSite=Strict" set. 

In a separate tab, I’m searching DuckDuckGo for a new laptop. It comes up with a laptop listed for sale on Amazon that looks interesting. 

I click on the link and it directs me to Amazon, but I’m not logged in. Why? Because the link I clicked on came from a different site (DuckDuckGo), so it didn’t attach my login cookie to that request. 

If I now click anywhere else on Amazon’s site, I will be logged in - because that new Request originated from Amazon, not DuckDuckGo, and the cookies were sent.

Now, let’s say Amazon’s login cookie has "SameSite=Lax" (or it’s nonexistent and my browser defaults to Lax). Using the same scenario above (clicking the Amazon link on DuckDuckGo’s page - while being logged into Amazon on a different tab), I would be directed to Amazon and still be logged in. 

That’s the main difference between SameSite’s settings - Strict and Lax.

So, yes, Strict is more secure, but inconvenient. That’s probably why most browsers default to "SameSite=Lax".

But a Lax setting won’t stop most of the CSRF (Cross-Site Request Forgery) attacks while a Strict setting will. Neither is foolproof (you should still use Anti CSRF tokens and require logged in users to confirm an action), but Strict is will do a lot more to prevent these attacks.

## You Made It!
Phew, you got all the way to the end. This was a long one, but now you hopefully have a better idea for how cookies work, and their weird little quirks. Use them wisely and understand exactly how they’ll work and how they’re vulnerable when building your web apps.

---

## Sources and Info:
- [IETF RFC 6265 for HTTP Cookies](https://tools.ietf.org/html/rfc6265 ){:target="_blank"}
- [MDN Web Docs - Set-Cookie HTTP Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie){:target="_blank"}
- [Browser compatibility for Cookie Attributes](https://github.com/mdn/browser-compat-data/blob/master/http/headers/set-cookie.json){:target="_blank"}

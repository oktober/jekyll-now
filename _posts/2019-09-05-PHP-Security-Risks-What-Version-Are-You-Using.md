---
layout: post
title: PHP Security Risks - What Version Are You Using?
categories: [Cybersecurity]
description: What version of PHP are you using? Do you know all the vulnerabilities in your PHP version and what mitigations do you have in place? Why does it even matter??
---

We all know we have to keep our devices updated. Our phones need an update about once a month (somtimes more). Software/App updates can even happen weekly. Our routers and other internet-connected devices need updating every so often too.

**But what about your PHP code?**

<!--more-->

Yes, we should all be on the latest version. We know this. But it’s often not a reality, especially with legacy code.

So keeping our codebase updated is tough, but do you know exactly which vulnerabilities exist in your current version of PHP? How about the libraries you’re using? **Do you think about this or keep chugging along hoping nothing in your codebase is ripe for attack?**

Let’s be honest, most of us cross our fingers and hope for the best. Code debt is awful too, but nobody has time to deal with it. Reviewing a whole codebase then researching the possible vulnerabilities? Yeah, that’s probably never happening.

But what kind of risk are we leaving ourselves, and our company, open to?

Let’s take a look, shall we?

PHP Version 7.3.6 has 2 currently known vulnerabilities (as of 9/19). That’s not too bad. Considering the current version is 7.3.9, that’s not looking too bad. But what if we go back a little further…

PHP Version 7.3.0 has 21 known vulnerabilities - a little bit rougher…
Version 7.2.0 has 34 known vulnerabilities while 7.2.5 has 50. These are all within PHP 7. 

What if we went back a little to PHP 5? Yes, many codebases still use it. 

Version **5.6.12 has 73 vulnerabilities** and **5.2.3 has 146 vulnerabilities!!!**

Okay, yeah, I’ll admit this data is cherry-picked. I’m specifically looking to see which version have many vulnerabilities. Not every version of PHP is this awful. But there is a pattern. The further back you go, the higher the average number of vulnerabilities. 

**Older code almost always has more known (and even unknown) vulnerabilities.**

Do you have to update to the absolutely newest version of PHP right now? No, of course not. Should you? Maybe. Should you at least be on 7? Yes, definitely.

Version 5.2.3 has at least 9 vulnerabilities that are highly critical. Criminals can do a lot of damage with highly critical vulnerabilities. If you’re on 5.2.3, do you know which of those 146 vulnerabilities your codebase is susceptible to? I think we both know the answer to that.

**When you stay on old codebases, you inevitably allow uneccesary, and possibly highly damaging, vulnerabilities.**

This is not a problem with an easy solution. I know that. But it’s worth reminding ourselves of how important updating is - whether it be our devices or our codebases. Most CEOs understand the important of patching computers quickly. But anywhere there’s outdated code, we open ourselves up to unnecessary vectors of attack. It may not be as vital as getting all Windows systems updated, but it’s more important than we treat it.

So avoid this unnecessary risk. **Write good, and secure, code by keeping your codebases (and libraries too!) up to date.** Attacks are happening all the time. We don’t want to make it that easy for them to get in. ;)

---

[Want to see all the currently known vulnerabilities in PHP?](https://www.cvedetails.com/version-list/74/128/1/PHP-PHP.html){"target="_blank"} Yeah, you do.

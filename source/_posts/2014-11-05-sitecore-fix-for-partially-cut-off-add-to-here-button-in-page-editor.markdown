---
layout: post
title: "Sitecore Fix: Partially-cut-off 'Add to here' button in Page Editor"
date: 2014-11-05 22:40:39 -0600
comments: false
categories: 
---

Ever since we first installed Sitecore 7.2 rev. 140228 (first release), we have noticed an odd visual bug in Page Editor.  We have since uninstall/reinstalled to be at the latest release: 7.2 rev. 140526.  However, we still see the issue. There appears to be a small rectangular "knock-out" overlaying the bottom edge of the "Add to here" button.  I was able to track it down to the "webedit.css" file in the root /Website folder where Sitecore is installed.

{% img /images/add-here-bug.png [100%] [auto] [Page Editor Bug [Page Editor Bug]] %}

In the webedit.css file, line 604, I changed "height: 11px;" to "height: 24px;" and that fixed the issue.

{% img /images/add-here-bug-fixed.png [100%] [auto] [Page Editor Bug Fix [Page Editor Bug Fix]] %}

I had first emailed Sitecore support about this bug, but they replied saying they weren't able to reproduce the issue with a clean install of 7.2. I find that kind of odd, since I have also noticed the bug during a demo of Sitecore 8 at the Milwaukee Sitecore Meetup group. The presenter was demoing the Experience Editor (previously Page Editor), and lo and behold, there it was!

So it's obviously an issue that exists, although I'm not sure what release(s) it's getting introduced in.
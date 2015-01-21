---
layout: post
title: "Sitecore Fix: Partially-cut-off 'Add to here' button in Page Editor"
date: 2014-11-05 22:40:39 -0600
comments: false
categories: [Sitecore, Add to here button, Page Editor]
description: How to fix 'Add to here' visual bug in page editor
keywords: Sitecore, Add to here, Page Editor
---

Ever since we first installed Sitecore 7.2 rev. 140228 (first release), we have noticed an odd visual bug in Page Editor. We have since uninstall/reinstalled to be at the latest release: 7.2 rev. 140526. However, we still see the issue. There appears to be a small rectangular "knock-out" overlaying the bottom edge of the "Add to here" button. I was able to track it down to the "webedit.css" file in the root /Website folder where Sitecore is installed.

{% img /images/add-here-bug.png [100%] [auto] [Page Editor Bug [Page Editor Bug]] %}

In the webedit.css file, line 604, I changed "height: 11px;" to "height: 24px;" and that fixed the issue.

{% img /images/add-here-bug-fixed.png [100%] [auto] [Page Editor Bug Fix [Page Editor Bug Fix]] %}

### UPDATE (11/6/14)

I contacted Sitecore support about this bug, and they weren't able to reproduce the issue with a clean install of 7.2. I thought that was kind of odd, since I have also noticed the bug during a demo of Sitecore 8 at the Milwaukee Sitecore Meetup group. The presenter was demoing the Experience Editor (previously Page Editor), and lo and behold, there it was!

After sending more information to Sitecore support, it turns out that the issue was/is caused by using "border-box" instead of "content-box" on some of their UI elements. We are using Bootstrap, which via its styles, was applying "content-box" to the element with the ".scInsertionHandleCenter" class.

The two recommended solution(s) from Sitecore are:

1. Remove box-sizing: border-box for * or div from site.min.css if possible.
2. Add the following style in your site's design CSS:
```
	.scInsertionHandleCenter 
	{
	  box-sizing: content-box !important;
	  -webkit-box-sizing: content-box !important; 
	  -moz-box-sizing: content-box !important; 
	 }
```
---
layout: post
title: "Team Development for Sitecore (TDS), GitHub, TeamCity, and Line Endings, Oh My!"
date: 2015-01-20 22:57:57 -0600
comments: false
categories: [Sitecore, GitHub, TeamCity, TDS, Gotchas]
description: 
keywords: Sitecore, GitHub, TeamCity, TDS, Team Development for Sitecore, Line Endings, CRLF, Gotchas
---

> Length of field content does not match the content-length attribute

Our development team has been running into a lot of situations lately where TDS items in our project are showing up as changed, when nobody had changed anything. The message we were seeing was something like: "Failed to load version 1 for language en
Length of field content does not match the content-length attribute. Field name: Value, field id: {C8B4641D-C20D-4424-84B3-5ED37AF9CD33}"

## First Try (fix content-length in *.item files)
After some Googling, we found an article that mentioned the error we were seeing (https://theagilecoder.wordpress.com/2013/03/02/tds-so-youre-deploying-sitecore-to-a-new-environment/) and we were able to fix the issues manually as they came up, by editing the *.item files and updating the content-length value(s). We did this for awhile, but the issue kept coming back, and we were all getting pretty frustrated.

## Git 'er Done! (make .gitattributes smarter)
After reaching out to Hedgehog Development, who makes the TDS product, we confirmed (as we had suspected) that the issue had to do with line endings getting changed at some point in the process. We were directed to a really helpful article by Sean Kearney (http://seankearney.com/post/Using-Team-Development-for-Sitecore-with-GitHub) which tells you to add a line to your .gitattributes file that says "*.item -text" . So all will we swell then, right? Well, if you have any previous files that still contain mixed LF and CRLF (which are what cause that error), you'll want to normalize those to all use CRLF only (if on Windows, in our case). Sublime Text has a nice plugin called "Line Endings Unify" that will let you batch update files to normalize the line endings.

## Final Piece (TeamCity plugin setting)
So updating our .gitattributes file seemed to fix our issues. That is, until we added TeamCity into the mix. We didn't have problems locally anymore, but when TeamCity was checking out the latest files from our GitHub repo, it was failing to build the TDS package. It was failing with the same error as before. After more head bashing and another email or two to Hedgehog Development, we learned that we needed to enable the "convert line endings to CRLF" in the Git VCS plugin settings for TeamCity (https://confluence.jetbrains.com/display/TCD8/Git+%28JetBrains%29).

## The Aftermath
It's only been a week since we implemented this fix, but so far we haven't seen this error come up again. We have a bunch of developers working constantly during the day on project features - branches are getting pull-requested, merged, built, deployed, and so far, no issues with line endings. We can only hope the worst is behind us.

## Sources
* https://theagilecoder.wordpress.com/2013/03/02/tds-so-youre-deploying-sitecore-to-a-new-environment/
* http://seankearney.com/post/Using-Team-Development-for-Sitecore-with-GitHub
* https://confluence.jetbrains.com/display/TCD8/Git+%28JetBrains%29
* The great support team at Hedgehog Development (support@hhogdev.com)

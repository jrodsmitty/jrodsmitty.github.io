---
layout: post
title: "Submitting a Module to the Sitecore Marketplace"
date: 2014-12-23 20:33:56 -0600
comments: false
categories: [Sitecore, Sitecore Marketplace, Marketplace, Community, Contributions, Facebook]
description: The Process of Creating a Sitecore Module For the Marketplace
keywords: Sitecore, Sitecore Marketplace, Community
---
<!-- more -->
> I recently released my first module on the Sitecore Marketplace. It's a super-simple, highly portable Facebook Like Button MVC View Rendering (https://marketplace.sitecore.net/Modules/Facebook_Like_Button.aspx).

## I Started By Setting Some Ground Rules ##
For this exercise, I laid out a couple ground rules for myself, so I could actually finish something. Like many developers, I add some features, refactor, add some more stuff, set it aside awhile, forget about it, and then when I come back to it, it's not relevant anymore and I scrap it. These are the rules I set for myself.

### 1. Choosing something easy (but annoying) to implement every time it's needed ###
I often get asked to include a Facebook Like button in the footer of a site. Sure, it's a copy and paste thing, but it's still mind-numbing work, and it's still not editable by a content author. It would be nice to have it editable (and translatable for a multi-site setup so you can have a different "like" url for different languages, for instance).

### 2. Following the KISS principle ###
Since there was so little logic with the solution for this, I really didn't feel it warranted putting the logic in a Controller or storing the field values in a Model, which would have generated an output .dll. This decision had the side benefit of allowing a developer to easily modify the code if needed, since  all the logic lives in the .cshtml file itself in the /Views folder.

### 3. Setting a deadline ###
Due to many half-done and forgotton pet projects in my past, I chose to give myself a tight deadline so I would be forced to manage scope. I work well against tight deadlines at work, so why would I tend to be so extreme the opposite for personal projects? I chose a limit of 8 hours or less for this.

### 4. Submitting to the Marketplace ###
The submission process was easy. You give a name to your module, add a description, some screenshots, documentation as needed, upload your .zip package file, and you're done. Then a Sitecore employee tests your module, and once they confirm it works as advertised, they approve and publish it. Not too complicated.

### Completed ###
At the end of the day, that module does only one thing really well, but it met all the criteria I had laid out. I got my feet wet, and know what to expect next time.
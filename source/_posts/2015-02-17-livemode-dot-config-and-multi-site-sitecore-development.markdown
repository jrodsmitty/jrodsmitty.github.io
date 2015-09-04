---
layout: post
title: "LiveMode.config and Multi-Site Sitecore Development"
date: 2015-02-17 21:26:21 -0600
comments: false
categories: [Sitecore, Live Mode, Multi-Site, Development]
description: 
keywords: Sitecore, Live Mode, Multi-Site, Multisite
---
<!-- more -->
## But MOM, I don't WANNA click Publish!
If you don't want to bother publishing your local Sitecore site every time you make a change (especially when developing), there's a nifty file named __LiveMode.config.example__, located in __C:\inetpub\wwwroot\yoursitecoresite\Website\App_Config\Include\ __. It comes with the sitecore install, and for a normal single-site Sitecore instance, simply removing the ".example" from the filename will let you view your site using the __master__ database instead of the live __web__ database. This has many advantages as a developer, namely, time-savings. Every keystroke and mouseclick counts!

## What about in a multi-site instance?
When you are running two or more sites concurrently in one Sitecore instance, the recommended approach is to define each site's host header and other information in a __SiteDefinition.config__ file (a much better approach then directly editing the Web.config file). This file also specifies the database a site should read from. Note the __database="master"__ below:
{% codeblock %}
<site name="yoursitecoresite" patch:before="site[@name='website']"
            virtualFolder="/"
            physicalFolder="/Sites/yoursitecoresite"
            rootPath="/sitecore/content/yoursitecoresite"
            startItem="/home"
            database="master"
            domain="extranet"
            allowDebug="true"
            cacheHtml="true"
            htmlCacheSize="10MB"
            enablePreview="true"
            enableWebEdit="true"
            enableDebugger="true"
            disableClientData="false"
            hostName="local.yoursitecoresite.org"
            enableFallback="false" />
{% endcodeblock %}

It turns out that when working in a multi-site setup in one instance of Sitecore, the SiteDefinition.config file takes precedent, and the LiveMode.config file is completely ignored. So in our project(s), we don't even bother to deploy LiveMode.config to a development environment. It's redundant.

## Cool! So does this mean I can just use Live Mode in production, and skip having to hit publish?
I __DO NOT__ recommend running your live site off of the master database! You should isolate your live content from the "in-progress" editing content to avoid major workflow headaches, if it's more than just you making changes. Plus, using master/web is the recommended approach from Sitecore, anyhow.

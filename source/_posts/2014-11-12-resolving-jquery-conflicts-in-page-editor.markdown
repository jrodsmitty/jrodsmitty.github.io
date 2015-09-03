---
layout: post
title: "Resolving JQuery Conflicts in Page Editor"
date: 2014-11-12 19:49:03 -0600
comments: false
categories: [Sitecore, JQuery, Page Editor]
description: Resolving JQuery Conflicts in Page Editor
keywords: Sitecore, JQuery, Page Editor
---
<!-- more -->
In the current project I'm working on, we are using Bootstrap's "collapse" functionality (http://getbootstrap.com/javascript/#collapse) for some of our components. Our requirements dictate that all accordions on the page be in the collapsed state, so in "Edit" mode of Page Editor, we'd like them all expanded for ease of editing. The easiest way to accomplish this is to check for the PageMode, and execute some JQuery code to .collapse('show') all accordions on the page.

This seems like a fairly simple task (assuming we know JQuery decently well), but we run into some issues because our page's layout is using JQuery 2.1.1, but Sitecore loads JQuery 1.10.2 when using Page Editor, so we run into a JQuery version conflict, and in our case, all our calls to $(element).on('click', function() {..}) fail, because 1.10.2 doesn't support the .on() method.

## There are a couple ways to deal with this:

1. Give up using a different version of JQuery, and just include and use version 1.10.2 throughout the site.
2. Utilize JQuery.noConflict() and some other tricks to get things to work with version 2.1.1.

We really wanted to use version 2.x of JQuery since our project requirements let us ignore (for the most part) older browsers, and all the code-bloat in JQuery 1.x that handles older browser quirks for browsers like Internet Explorer has been removed in v2. So for our situation, we chose to go with option 2.

Page Editor loads its version of JQuery after we do during the page rendering (ours is within the head tag), so by the time we call our JQuery code, the global "JQuery" object now represents the older 1.10.2 version instead of 2.1.1, so our code throws the error "Uncaught TypeError: undefined is not a function"

### Here was our solution:
_NOTE: We're using an MVC view for our component_

{% codeblock %}
@using Sitecore.Mvc;

<div class="panel panel-default">
    <div class="panel-heading collapsed" data-toggle="collapse-next" aria-expanded="false">
        <h4 class="panel-title">
            @Html.Sitecore().Field("Heading")
        </h4>
    </div>

    <div class="panel-collapse collapse">
        <div class="panel-body">
            @Html.Sitecore().Placeholder("collapsiblecontent")
        </div>
    </div>
</div>

@if (Sitecore.Context.PageMode.IsPreview)
{
    <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
}

<script>
    var $jq2 = jQuery.noConflict();
    $jq2(function() {
        if (IsPageEditorEditing()) {
            $jq2('[data-toggle=collapse-next]').next().collapse('show');
        } else {
            $jq2('[data-toggle=collapse-next]').next().removeClass('in');
            $jq2('[data-toggle=collapse-next]').on('click', function() {
                $jq2(this).next().collapse('toggle');
            });
        }
    });
</script>
{% endcodeblock %}

1. LINE 17: To combat this "re-defining" of JQuery version, we first re-include our version of JQuery if we're in Preview Mode.
2. LINE 23: We add our code using a re-scoped variable for the global "jQuery" object.
3. LINE 49: In active "editing" mode, we force all accordions to stay expanded, otherwise they function normally.

Although it feels dirty re-including jquery like this, if you really need to be using a different version of JQuery other than what Sitecore includes, this is (at least one) workable way to accomplish that.
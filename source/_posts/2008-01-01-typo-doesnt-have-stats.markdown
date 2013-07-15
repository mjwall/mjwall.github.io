---
author: mjwall
comments: false
date: 2008-01-01 17:14:00+00:00
layout: post
slug: typo-doesnt-have-stats
title: Typo doesn't have stats
wordpress_id: 15
alias: /2008/01/typo-doesnt-have-stats/
categories:
- typo
---

Now that I have used [typo](http://www.typosphere.org) for a couple of days now, I realized it is missing page stats.  Maybe it is vanity, but if I am going to spend time posting information, I want to know when someone is looking at it.  So I looked on the mailing list and didn’t see anything current that would help.  My first thought, right on, now I have a chance to write my first open source plugin.  Then I came to my senses and decided to google around.

I found a post on [Juixe TechKnow](http://www.juixe.com/techknow/index.php/2007/02/04/rails-google-analytics-plugin/) about a plugin from [Graeme](http://woss.name/2006/05/09/google-analytics-plugin/).  Looked pretty good and I have been wanting to try google analytics.  I also found this [plugin](http://www.artweb-design.de/projects/ruby-on-rails-plugin-google-analytics) which appears to be an updated version of Graeme’s.  I tried the blue egg edition, but was getting errors about a missing Liquid constant.  Didn’t feel like adding [liquid](http://code.google.com/p/liquid-markup/) so I went back to the first plugin.

Install and configuration was simple.  Signed up for [google analytics](https://www.google.com/analytics/home/), installed the plugin, modified my environment.rb.  Now I am collecting stats.

I did run see this [discussion](http://article.gmane.org/gmane.comp.web.typo.user/605/match=stats) on the mailing list about gathering stats from feeds.  I have already signed up for [feedburner](http://www.feedburner.com/fb/a/home) and replaced the syndication sidebar with a static one.

Should I be concerned?  Everything has been too straightforward.

The only issue I see is that the plugin uses the older version of google analytics tracking, urchin.js.  Last month, google launched a new script, ga.js.  See [here](http://www.google.com/support/googleanalytics/bin/answer.py?hl=en&answer=69588) for more info.  Maybe I’ll update the plugin.  I also want to check to see if typo is caching the javascript file or downloading it every time.

BTW, Happy New Year.

---
author: mjwall
comments: false
date: 2008-03-05 14:30:00+00:00
layout: post
slug: rails-shared-hosting-and-a-little-merb
title: Rails Shared Hosting and a little Merb
wordpress_id: 13
alias: /2008/03/rails-shared-hosting-and-a-little-merb/
categories:
- merb
- rails
---

It has taken me almost 2 months to put this post together, but at the time, DHH’s [article](http://www.loudthinking.com/posts/21-the-deal-with-shared-hosts) on shared hosting support for Rail was very timely.  I use shared hosting to run this blog and have had some headaches. [This](http://www.rubyinside.com/no-true-mod_ruby-is-damaging-rubys-viability-on-the-web-693.html) is another great post on the subject.  Lots, and I mean lots, of interesting comments.  Part of those 2 months was getting through those comments.

So what can we do?  [Rubinius](http://rubini.us/) / [mod_rubinus](http://brainspl.at/articles/2008/02/12/what-do-you-want-to-see-in-mod_rubinius) seem really interesting.  Long term, this may be a great solution.  More immediately, I am intrigued by [Rack](http://rack.rubyforge.org/).  Rack is just a webserver abstraction and would not solve the lack of threading in Rails though.  But there is a Rack adapter built into [Merb](http://www.merbivore.com/), which is thread safe.  I will be spending some time looking into Merb and I’ll document my investigation here.

So what headaches have I had with shared hosting.  Only a few, because this site doesn’t get much traffic.  Here are some details about my setup _(warning, here comes an ad)_.  I have the “Small” plan from [asmallorange](http://refer.asmallorange.com/10815) (ASO).  The price is very reasonable and the features are good:  SSH access, Rails support, unlimited mail accounts, unlimited mysql databases, unlimited subdomains and many other features.  Support has been great, they have installed every gem I have asked for within minutes without a hard time, even [Camping](http://code.whytheluckystiff.net/camping/) but that is a story for another time.  ASO also has a reseller plan so you can sell off part of your hosting and make a little money.  Ok, back to the post.

So what problems have I had with shared hosting?  Like most shared hosts ASO uses FastCGI to support Rails.  Mongrel is too expensive in a shared hosting environment.  My problems have been with FastCGI and limits ASO has in place.  They allow 5 instances of FastCGI to run concurrently.  Again, no problem with the amount of traffic I get.  However, idle instances die after 4 minutes.  So at some point, all my instances die and the next request has to startup FastCGI and then serve the page.  It can take 2 minutes, which is too long.  There are some reaper and spinner scripts in Rails, but I don’t have enough permissions on my server to run them.

Another problem I had came from wanting to mimic my production environment as closely as possible on my laptop.  I tried in vain to install Apache with FastCGI.  Yes, it is windows and I was using Cygwin, but after 2 weeks of off and on install and configuration failures, I gave up.

If FastCGI is running, I have had no complaints with performance.  You can judge that for yourself.  I don’t fault ASO either.  The restrictions they place on FastCGI make sense for their business model.  Shared hosting for Rails is hard, and I think they have done a good job accommodating the community so far.  I would recommend them if you want to support for multiple languages, if you are looking for really inexpensive hosting, and if you understand what you are getting into.  Certainly they are good as [Dreamhost](http://blog.dreamhost.com/2008/01/07/how-ruby-on-rails-could-be-much-better/)
All the other Rails apps I work on are run with mongrel clusters.  Currently, I think that is best option for production sites with a decent or even large amounts of traffic.  I’ll be interested to see if Ezra recommends anything else in his new [book](http://brainspl.at/articles/2008/02/21/at-long-last-my-deploying-rails-applications-book-is-done)

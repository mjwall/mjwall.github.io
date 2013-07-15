---
author: mjwall
comments: false
date: 2008-08-18 23:44:06+00:00
layout: post
slug: cruisecontrolrb-git-and-rails-21
title: Cruisecontrolrb, git and rails 2.1
wordpress_id: 25
alias: /2008/08/cruisecontrolrb-git-and-rails-21/
categories:
- cruisecontrol
- rails
- ruby
---

I really love cruisecontrol.rb in a team environment.  The ability to run tests against every commit is great, and it is such fun being the "build police".


> Excuse me sir, can I see your license to commit please?


We can have some fun with that one.


> This license expired just before the dotcom bubble




> You have a languague restriction here that says you can commit only while wearing vb.net




> I need to see some proof of build collision insurance.


But I digress. Sorry, back to the program.

As rails grows, I have some questions.




  1. Where the heck is code for cruise control?


  2. Can I use git?


  3. Will it run against rails 2.1 since I am security conscience and have updated after the recent [warnings](http://www.ruby-lang.org/en/news/2008/06/20/arbitrary-code-execution-vulnerabilities/) and [threats](http://www.ruby-lang.org/en/news/2008/08/08/multiple-vulnerabilities-in-ruby/)?


Let's do some research.  The code used to be at [rubyforge](http://rubyforge.org/projects/cruisecontrolrb/).  Looks like the code is still there.  However, according to this [post](http://rubyforge.org/pipermail/cruisecontrolrb-users/2008-August/000560.html), they are moving to github.  The link in that post has an extra *, but sure enough, you can find cruisecontrol.rb at [http://github.com/thoughtworks/cruisecontrol.rb](http://github.com/thoughtworks/cruisecontrol.rb).  Cool, now I know where to get it.

What about git support?  A [google search](http://www.google.com/search?hl=en&q=cruisecontrolrb+git&btnG=Google+Search), turns up [benburkert](http://github.com/benburkert/cruisecontrolrb/tree/master)'s github branch.  Looks promising.  Searching git hub for [cruisecontrol.rb](http://github.com/search?q=cruisecontrolrb) shows several forks, but these are all from rubyforge git.  I want it straight from thoughtworks.  Digging a little in the code shows it is already in the thoughtworks master.  There is even a mercurial adapter.  Let's give it a try.  Sure enough, it works just fine with


    cruise add projectname -r file:///home/me/src/gitproject -s git


Run


    cruise help add


for all the options.

Great, 2 for 2.  Now what about support for Rails 2.1?   Unfortunately, it is not there.  Cruisecontrol.rb currently runs with rails 1.2.3, and a simple version change in environment.rb doesn't fix it.  (Didn't figure it would but I had to try it right?)   For all of us who have updated to ruby 1.8.7, we are stuck for now.  Ruby 1.8.7 only runs rails 2.1.  My [last](http://www.mjwall.com/2008/08/multiple-versions-of-ruby-with-stow/) post about multiple version of ruby doesn't really help either.

I did find this post answered by the team about porting cruisecontrol to [merb](http://www.nabble.com/Re:-Plans-to-upgrade-to-Rails-2.1--td18115378.html).  That sounds interesting indeed.

Until then, who is working on a rails 2.1 version of cruisecontrol.rb?

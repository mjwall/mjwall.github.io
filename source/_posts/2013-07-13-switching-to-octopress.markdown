---
layout: post
title: "Switching to Octopress"
date: 2013-07-13 22:54
comments: true
categories:
  ruby
  octopress
---
Switching to Octopress, see <http://octopress.org/>.  I haven't blogged
in a long time, but I wanted to start again.  Got some emacs stuff I'd
like to get out.  Anyway, here are some notes on my conversion.

* Theme is Greyshade, see <https://github.com/shashankmehta/greyshade>.
  My color is #0020C2, see
  <https://github.com/shashankmehta/greyshade/wiki/Sites-using-Greyshade>.
  I really like the mobile format baked into the theme.

* Exported old wordpress content with
  <https://github.com/thomasf/exitwp>.  Worked great, after I cleaned up
  the xmllint errors.  Looks like there was a few formatting issues,
  but I don't care enough to fix them.

* Found alias generator plugin for Jekyll at
  https://github.com/tsmango/jekyll_alias_generator.  Just copied the
  alias_generator.rb into the plugins directory and added something
  like the following to the yaml section of every old post.  Allows
  the old links to still work, in case anyone had linked to me.

  `alias: /original/link/from/wp/blog`

* Thought about migrating all the old comments over and using
  something like <http://disqus.com/>.  In the end, I decided not
  to enable comments at all.  Hit me up on twitter or google+ if you
  really have something to say.

So far, this has been really easy.  I like it a lot.  Nice to be using
ruby again.

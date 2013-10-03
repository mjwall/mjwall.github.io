---
layout: post
title: "Emacs 24 AmpleZen Theme"
date: 2013-10-03 12:01
comments: true
categories:
  emacs
---
Last week I came across a theme for Emacs called Ample at
<https://github.com/jordonbiondo/ample-theme>.  I loved the colors and
general feel of the theme and starting using it immediately.  It
worked great and I really like it.  The only issues I had was that
[rbenv.el](https://github.com/senny/rbenv.el "rbenv.el")
in the modeline was an ugly red and the theme didn't work in the
terminal.  So I started poking at the theme code.  It is the first time I
have ever opened up a theme and looked at tweaking it.

Along the way, I started looking at other themes.  I kept coming back
to zenburn-emacs at <https://github.com/bbatsov/zenburn-emacs>. I liked
the way the code was organized and the little touches like
automatically starting up
[rainbow-mode](http://elpa.gnu.org/packages/rainbow-mode.html
"rainbow-mode"), which I had never used.

So I decided to try and combining the colors from Ample and the code
from Zenburn into one theme.  My result is available at
<https://github.com/mjwall/ample-zen>.  Zenburn had a bigger palette
than Ample, so I added in some colors.  I removed packages that I
don't use, and added some, like rbenv, that I do.  For me, the
modeline in the active window is important, so I tried to make that
stand out.  It appears to work OK running Emacs in the terminal as well.

Here is a screenshot

![Ample Zen Screenshot](https://raw.github.com/mjwall/ample-zen/master/ample-zen.png
"Ample Zen Screenshot")

All the credit goes to Jordon Biondo for his colors in Ample and
Bozhidar Batsov for his code in Zenburn-Emacs.  Ample-Zen is currently
available in Marmalade and a pull request has been merged to MELPA.
Read more about installation at <https://github.com/mjwall/ample-zen#installation>

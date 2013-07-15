---
author: mjwall
comments: false
date: 2009-01-01 21:51:37+00:00
layout: post
slug: groovy-plugin-patch-for-running-grails-11-beta2-on-intellij-801
title: Groovy plugin patch for running Grails 1.1 Beta2 on Intellij 8.0.1
wordpress_id: 96
alias: /2009/01/groovy-plugin-patch-for-running-grails-11-beta2-on-intellij-801/
categories:
- grails
- groovy
- intellij
- java
---

My last [attempt](http://www.mjwall.com/2008/12/intellij-801-issues-with-grails-11-beta2/) at running Grails 1.1 beta2 on IntelliJ 8.0.1 didn't work out so well.  I ran the EAP version for a while, but had some issues building grails itself, specifically trying to run unit tests.

So, I did some digging through the [bug tracker](http://www.jetbrains.net/jira/browse/GRVY) and [subversion](http://svn.jetbrains.org/idea/Trunk/bundled/groovy/) for the [plugin](http://plugins.intellij.net/plugin/?id=1524).  I also spent some time learning about IntelliJ [plugins](http://www.jetbrains.com/idea/documentation/howto_03.html?Where%20to%20Begin?) and reading newsgroups about what others [have](http://www.jetbrains.net/devnet/thread/279026#279026) [tried](http://www.nabble.com/Intellij-IDEA-Jetgroovy-and-Grails-1.1-Beta-1-td20817716.html).  The result is a patched version of the plugin that seems to be working.  Here is the [file](http://www.mjwall.com/files/jetgroovy-21543plus21697.zip) if you want to try it.  Unzip and replace the contents in your INTELLIJ_HOME/plugins/Groovy directory.  Again, use at your own risk and backup your existing plugins/Groovy directory.

If you are interested, here are the details.




  * Running on a Mac with java 1.5


  * Check out code from http://svn.jetbrains.org/idea/Trunk/bundled/groovy


  * Revert back to revision 21543.  21544 breaks something.  Revision 21538 fixed the initial issue I saw, where the grails-1.1-beta2 library was not recognized, as reported in [GRVY-1933](http://www.jetbrains.net/jira/browse/GRVY-1933).


  * Add in revision 21697, which fixes [GRVY-1943](http://www.jetbrains.net/jira/browse/GRVY-1943) so the app can run.


  * Setup IntelliJ 8.0.1 build 9164 with the [dev](http://www.jetbrains.com/idea/download/index.html#kit) package


  * Configure IntelliJ to build the plugin.  [These](http://www.jetbrains.net/confluence/display/GRVY/How+to+build+plugin) instructions helped.  Groovy module configured to use Groovy-1.6_RC1.  RT module configured to use groovy-1.5.7, cause 1.6 didn't work.


  * Build grammer with ant task, run Make and then run 'Prepare All Plugin Modules for Deployment'.


  * Shutdown IntelliJ.


  * Remove INTELLIJ_HOME/plugins/Groovy directory.  Unzip groovy.zip in INTELLIJ_HOME/plugins directory


  *

  * Restart IntelliJ


Hope it works for you and Happy New Year.

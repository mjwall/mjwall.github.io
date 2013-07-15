---
author: mjwall
comments: false
date: 2008-12-30 03:20:11+00:00
layout: post
slug: intellij-801-issues-with-grails-11-beta2
title: 'Intellij 8.0.1 issues with Grails 1.1 beta2 '
wordpress_id: 79
alias: /2008/12/intellij-801-issues-with-grails-11-beta2/
categories:
- grails
- groovy
- intellij
- java
---

Grails-1.1-beta2 does not work correctly with the jetgroovy plugin in intellij.  I am able to add a global library for Grails 1.1 beta2, but it not saved in the project facet.  The most apparent problem for me is that the 'Run grails target' shortcut is not available.  See [this](http://www.jetbrains.net/devnet/thread/278936?tstart=0) thread and this [bug](http://www.jetbrains.net/jira/browse/GRVY-1933?page=com.atlassian.jira.plugin.system.issuetabpanels:all-tabpanel) for more details.  According to the details, it is fixed.  However, I don't see an update to the jetgroovy plugin.  I am running version 8.0.1 of Intellij on Mac OSX.

So I tried the [EAP](http://www.jetbrains.net/confluence/display/IDEADEV/Diana+EAP) 9572 version to see if it included the fix that was reported in Jira.  The bundled jetgroovy plugin appears to have the update.  Yippee.  Glad I didn't have to build it myself according to the [wiki](http://www.jetbrains.net/confluence/display/GRVY/How+to+build+plugin),

Here is the interesting part that you may care about.  I zipped up the plugins/Groovy folder and replaced the old version in Intellij 8.0.1.  It seems to have fixed it for the stable version as well.  [Here](http://www.mjwall.com/files/jetgroovy.zip) is the file until JetBrains has something better.  Use at your own risk, perhaps making a backup of the old plugins/Groovy folder.

**UPDATE** It appears there are issues with copying the plugin back to the 8.0.1 release.  Loading the file inside grails-app just hangs.  However, the EAP version is working just fine so far.  Let me know if you have success getting it work in the stable version.

**UPDATE 2** See [this](http://www.mjwall.com/2009/01/groovy-plugin-patch-for-running-grails-11-beta2-on-intellij-801/) for a patched version that works better.

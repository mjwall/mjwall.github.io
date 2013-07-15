---
author: mjwall
comments: false
date: 2008-08-16 22:48:24+00:00
layout: post
slug: multiple-versions-of-ruby-with-stow
title: Multiple versions of ruby with stow
wordpress_id: 49
alias: /2008/08/multiple-versions-of-ruby-with-stow/
categories:
- ruby
- stow
---




What is the best way to support multiple versions of ruby?




Some options:




  * [Multiruby](http://www.zenspider.com/ZSS/Products/ZenTest/#rsn) looks like it may do what I want.  I already have it installed, because I am autotest-addicted (first coined by [Dr Nic](http://drnicwilliams.com/2008/02/19/one-stop-javascript-unit-testing-for-rails2/).


  * Michael Greenly posted [this](http://blog.michaelgreenly.com/2007/12/multiple-ruby-version-on-ubuntu.html), which would work since I am running ubuntu.


  * A buddy recommended [GNU Stow](http://www.gnu.org/software/stow/manual.html).







I chose to use stow.  Call me old fashion, but I really prefer doing things be hand.  I didn't like the thought of having to type ruby1.8.6 vs ruby1.8.7 proposed by Michael Greenly, but I did pick up a few things from his article.  I am interested in the testing against multiple version like multiruby provides.  However, the main reason I want different ruby installs is because different clients have different environments.  So here is how I did it.  For reference, I am using Ubuntu 8.0.4.









  1. **Remove all ubuntu ruby packages.**


Use


    dpkg --list | grep -i ruby


to find all the packages you have installed.  Then use


    sudo apt-get remove <em>packagename</em>


to remove those packages.  We are going to build all of these by hand.



  2. **Make sure you have the ubuntu build packages**


Run the following to make sure you have packages necessary to build ruby with


    sudo apt-get build-dep ruby1.8


This will install stuff like autoconf and automake if you don't already have them.



  3. **Setup your filesystem for stow.**


Let's first make a directory under /usr/local named stow with


    sudo mkdir /usr/local/stow


We will install everything here and then use stow to create symlinks in /usr/local/bin. Also, change the directory permission for /usr/local/stow so you can install stuff there as your user.  This has the added benefit of alerting you if you configure wrong or try to overwrite something.  We will only need to sudo to run the stow command.  Run the following to see what groups you are in


    groups username


Hopefully there will be a dev or adm or something similiar.  I will use the adm group.  Run the following to change the group and permissions on /usr/local/stow.


    sudo chgrp -R adm /usr/local/stow


and


    chmod -R 775 /usr/local/stow






  4. **Install stow**


Download the stow package with from ftp://mirrors.kernel.org/gnu/stow/stow-1.3.3.tar.gz.  Then


    tar -zxf stow-1.3.3.tar.gz


to extract everything.  Go into the directory and then run the following.


    ./configure --prefix=/usr/local


Then install it with make and sudo make install

I considered installing stow under the stow directory but figured it was overkill



  5. **Install ruby1.8.6**


Get the code with from ftp://ftp.ruby-lang.org/pub/ruby/ruby-1.8.6-p287.tar.gz.  Unzip and cd into the directory.  Run


    ./configure --prefix=/usr/local/stow/ruby-1.8.6-p287


and then


    make && make install


You shouldn't need to sudo if you have the permissions correct.  Run


    make install-doc


if you want ri and rdoc, but it takes a while.



  6. **Install ruby1.8.7**


Much like the 1.8.6 version, get the code with ftp://ftp.ruby-lang.org/pub/ruby/ruby-1.8.7-p72.tar.gz Unzip and cd into the directory.  Run


    ./configure --prefix=/usr/local/stow/ruby-1.8.7-p72


and then


    make && make install


Again run


    make install-doc


if you want.



  7. **Update your path**


Make sure that /usr/local/bin is on your path.  Since we don't have ruby installed anywhere else, you can put it at the end.  You could put it earlier if you have other versions installed.


    echo $PATH


to see if you already have it.  If not, go to your .bashrc and add it where ever PATH is exported.



  8. **Run stow**


Lets set up ruby 1.8.7.  Change to the /usr/local/stow directory and run


    sudo stow ruby-1.8.7-p72


All the symlinks are created for you.



  9. **Install rubygems, gems and then stow again**


Running


    ruby -v


should show you it is version 1.8.7.  You will need to install rubygems now. Get it from http://rubyforge.org/frs/download.php/38646/rubygems-1.2.0.tgz and then extract it into a directory. Change into that directory and run


    ruby setup.rb


Now rubygems is installed in the ruby directory, but you will need run stow again.  Change to /usr/local/stow and run


    sudo stow ruby-1.8.7-p87


Running the command


    which gem


should show you have it installed.  You can now install gems like mongrel, rails etc.  When you finish installing gems, be sure to run stow again so it will symlink the executables.








That's it. Switching to ruby 1.8.6 is easy.  Go to /usr/local/stow and run







    stow -D ruby-1.8.7-p22 && stow -R ruby-1.8.6-p287


The -D removes all the symlinks.  Run the following to see all the options for stow


    stow -h


Follow the instructions above to setup rubygems and all the gems again.  Remember to rerun stow so the executable are symlinked.  After you install the gems the first time, switching is simply a matter of stow -D and then stow -R.  Pretty clean and easy.




A great thing about stow is I can use it for other packages as well, like mysql or php.  I'll note that ruby doesn't have a


    make uninstall


so it is ugly if you want to remove the packages.  With stow, all you have to do is remove the directory.




Notes:
I tried to cover most commands I used.  If you need more unix reference, try something like   [this](http://www.ee.surrey.ac.uk/Teaching/Unix/index.html) or [this](http://freeengineer.org/learnUNIXin10minutes.html).  Google is your friend.





In addition to the articles linked above, I also read the following when writing this post [one](http://blog.danieroux.com/2005/08/07/using-gnu-stow-to-manage-source-installs/) and [two](http://www.linux.com/feature/127393)

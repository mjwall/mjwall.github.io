---
layout: post
title: "How I use Emacs"
date: 2013-10-04 08:57
comments: true
categories:
  emacs
  bash
---
Been meaning to write a post about my current Emacs setup for a while to
explain how I work with Emacs on both Mac and Linux. I am going to
call this my *Emacs Workflow*.  I have been using this
setup for over a year now with very few tweaks and it serves me well.
I am currently using Emacs 24, but this setup worked fine for earlier
versions.

First, let me explain how I work and what I was looking for out of an
Emacs Workflow.  I spend most of my time on the command line.  That
is either a terminal or eshell running in Emacs, with really no rhyme
or reason for which.  Although longer running things like tailing logs
and stuff over ssh tends to crash Emacs so I typically do not do those
things in eshell.  Having used vim for a long time, I got used to
quickly opening a file, making an edit, and then closing it.  But I do
find it helpful to have all the currently open files in Emacs available
in buffers. Emacs daemon seemed to fit both of these, but I
didn't like starting it up in my init.el or on login.

Avdi Grimm wrote an article at
http://devblog.avdi.org/2011/10/27/running-emacs-as-a-server-emacs-reboot-15/
about how he launches Emacs and that got me started.  I hadn't used
emacsclient very much before this.  The `-a ""` trick was exactly what
I wanted to start the daemon.  Avdi uses this script to launch
emacsclient and create a new frame.  By default, the terminal waits
for you to close Emacs, but you can pass in -n to the ec script and
return control back to the terminal immediately.

### My 'Emacs Workflow'
My workflow is a little different.  When a file is opened in the
Windowed or GUI version of Emacs, I want to work on it and leave it open.
Often times I am heading back to the terminal to run a command against the
newly edited file, like `rake test` or `mvn package`.  That mean the
terminal launching emacsclient shouldn't wait.  When there is a GUI
version of emacs already running, I want to use that instead of
opening a new frame.  When a GUI Emacs is open but minimized, I want
to maximize it and then open the file there.

For quick edits, I want to open the file quickly in Emacs in the
current terminal, make my edit, and then close it.  Therefore, the
terminal needs to wait for me to finish.

#### Sidebar
> This method for quick edits is how I did all my git commits before I
> took the time to learn magit.  If you haven't used magit, I highly
> recommend you take the time to learn it.  See
> <http://magit.github.io/magit/magit.html>.  This is why I
> `export editor=et` in my ~/.bashrc.

### Tools
So what I ended up with is 2 scripts, which I call *ec* and *et*,
following Avdi's lead.  The former opens emacsclient in the GUI and
returns control immediately to the shell. The latter opens emacs in
the current terminal and waits. Because both scripts are backed by the
same daemon, all open files are available as buffers in both cases.
Both script will starts the daemon if it is not
open.  The `ec` script has some extra code to switch focus as
described in my workflow. Here are the scripts, which what I hope are
useful comments.

#### ec
```bash
#!/bin/bash

# This script starts emacs daemon if it is not running, opens whatever file
# you pass in and changes the focus to emacs.  Without any arguments, it just
# opens the current buffer or *scratch* if nothing else is open.  The following
# example will open ~/.bashrc

# ec ~/.bashrc

# You can also pass it multiple files, it will open them all.  Unbury-buffer
# will cycle through those files in order

# The compliment to the script is et, which opens emacs in the terminal
# attached to a daemon

# If you want to execute elisp, pass in -e whatever.
# You may also want to stop the output from returning to the terminal, like
# ec -e "(message \"Hello\")" > /dev/null

# emacsclient options for reference
# -a "" starts emacs daemon and reattaches
# -c creates a new frame
# -n returns control back to the terminal
# -e eval the script

# Number of current visible frames,
# Emacs daemon always has a visible frame called F1
visible_frames() {
  emacsclient -a "" -e '(length (visible-frame-list))'
}

change_focus() {
  emacsclient -n -e "(select-frame-set-input-focus (selected-frame))" > /dev/null
}

# try switching to the frame incase it is just minimized
# will start a server if not running
test "$(visible_frames)" -eq "1" && change_focus

if [ "$(visible_frames)" -lt  "2" ]; then # need to create a frame
  # -c $@ with no args just opens the scratch buffer
  emacsclient -n -c "$@" && change_focus
else # there is already a visible frame besides the daemon, so
  change_focus
  # -n $@ errors if there are no args
  test  "$#" -ne "0" && emacsclient -n "$@"
fi
```

#### et
```bash
#!/bin/bash

# Makes sure emacs daemon is running and opens the file in Emacs in
# the terminal.

# If you want to execute elisp, use -e whatever, like so

# et -e "(message \"Word up\")"

# You may want to redirect that to /dev/null if you don't want the
# return to printed on the terminal.  Also, just echoing a message
# may not be visible if Emacs then gives you a message about what
# to do when do with the frame

# The compliment to this script is ec

# Emacsclient option reference
# -a "" starts emacs daemon and reattaches
# -t starts in terminal, since I won't be using the gui
# can also pass in -n if you want to have the shell return right away

exec emacsclient -a "" -t "$@"

```

### Github repo
These files can be found in dotfiles repo at
<https://github.com/mjwall/dotfiles>.  There are also instructions on
how I install Emacs on a
[Mac](https://github.com/mjwall/dotfiles#on-macosx) and
[Linux](https://github.com/mjwall/dotfiles#on-linux).  Also in this
repo is my ~/.emac.d configuration.   I keep everything together to
make it as easy as possible to get setup on a new machine and keep
multiple machines in sync.

#### Warning
> If you are on a Mac, it is important to get the newer version of
> Emacs and emacslient on the path correctly.  What has worked for me is
> referenced in the mac
> [gist](https://gist.github.com/mjwall/3fe935a8becb60dd3c4c). Likely
> there are other/better ways.

### Bonus, executing elisp
Another way I use these scripts is by passing in -e to execute
arbitrary elisp code. For example, I have an alias setup in my bashrc
to launch magit. Because it is using the same script, it takes
advantage of launching the daemon if necessary and changing
focus. Here is what it looks like:

```bash
alias magit='ec -e "(magit-status \"$(pwd)\")"'
```
So in the terminal, I run `magit` and it launches Emacs and runs
magit-status on the current directory.  This was inspired by a
similiar tweet somewhere, but takes advantage of the rest of the `ec` script.

### Stopping the Daemon
The last piece of this was a shell script to stop the daemon, which is
used for example when I need to reload Emacs configs.  Sometimes
shutdown on my Mac hangs while waiting for Emacs to close, so I tend
to call this `es` script beforehand.  The script looks like this

#### es
```bash
#!/bin/bash

# simple script to shutdown the running Emacs daemon

# emacsclient options for reference
# -a Alternate editor, runs bin/false in this case
# -e eval the script

# If the server-process is bound and the server is in a good state, then kill
# the server

server_ok() {
  emacsclient -a "false" -e "(boundp 'server-process)"
}

if [ "t" == "$(server_ok)" ]; then
  echo "Shutting down Emacs server"
  # wasn't removing emacs from ALT-TAB on mac
  # emacsclient -e "(server-force-delete)"
  emacsclient -e '(kill-emacs)'
else
  echo "Emacs server not running"
fi

```
Likely there is a good way to fix this hanging, but it doesn't bother
me so I haven't dug deeper.

### Wrap up
If you are still reading this, you may be thinking _"This all makes me
want to execute arbitrary elisp in a shell script for other
things"_. If so, and you looked at
<https://github.com/mjwall/dotfiles/blob/master/bin/ed.el>, you would
see the following example of how to do that
```scheme
#!/usr/bin/env emacs --script
(print "Hi mike")
(require 'server)
(print (server-running-p))
```
Imagine the possibilities.  Go through a git repo and change all tabs
to spaces. I haven't really though of anything useful to do with this,
but thought it was interesting.

If you are not still reading this, you probably stopped because you
thought all this was overkill.  Maybe you are right.

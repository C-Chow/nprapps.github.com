---
layout: post
title: "How to Setup Your Mac to Develop News Applications Like We Do"
description: "(Almost) everything you always wanted to know about working from the command line, but were too afraid to ask"
author: Gerald Rich
email: grich@npr.org
twitter: gerald_arthur
---

*Updated Sept 13, 2013, to correct some details.*

*Hey guys, I'm [Shelly Tan](https://twitter.com/Tan_Shelly), the latest News Apps intern. You'll be hearing more from me later, but for now, just know that I'm still in awe of everything at NPR and have to resist keeling over in my rabid fangirling; Gerald wasn't kidding about the shiny offices and awesome food. Anyways, back to Gerald.*

I joined the News Apps team a week ago in their shiny new DC offices, and in-between eating awesome food and Tiny Desk concerts, we've been documenting the best way to get other journalists setup to build news apps like the pros.

The following steps will help you convert your laptop to hacktop, assuming you're working on a new Mac with Mountain Lion OS X 10.8 installed. Each Mac operating system is a little different, so we're starting from scratch with the latest OS.

## Chapter 0: Prerequisites
### Are you an administrator?
We'll be installing a number of programs from the command line in this tutorial, so that means you must have administrative privledges. If you're not an admin, talk with your friendly IT Department.

Click on the Apple menu > System Preferences > Users & Groups and check your status against this handy screenshot.

![Are you an admin?](/img/posts/c0_admin.png)

### Update your software
Click on the Apple menu > Software Update. Continue installing and rebooting until there is nothing left to update.

### Install Xcode and the Xcode's command line tools
We don't use Xcode everyday to develop, but it does give us some tools that you'll need to install. This part can take a while, so feel free to grab a cup of soup while this is downloading.

* Get [Xcode](https://developer.apple.com/xcode/) from the app store.
* Get the Xcode command line tools by going to Xcode > Preferences > Downloads and checking the "Install" button next to the command line tools.

![Install Xcode's command line tools](/img/posts/c0_xcode.png)

## Chapter 1: Install Homebrew

Start by opening up your terminal application. All Macs come with an app called "Terminal." You can find it under Applications > Utilities. Double click to open that bad boy up, and let's start with installing Homebrew.

[Homebrew](http://brew.sh/) is like the Mac app store for programming tools. You can access Homebrew via the terminal, ([like all good things](http://www.amazon.com/Beginning-was-Command-Line-Neal-Stephenson/dp/0380815931)). Inspiration for this section comes from Kenneth Reitz's excellent [Python guide](http://docs.python-guide.org/en/latest/starting/install/osx.html).

Install Homebrew by pasting this command into your terminal and then hitting "enter." 

	ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"

It will ask for your password, so type that in and hit "enter" again. Now, paste this line to test Homebrew.

	brew doctor

This will test your Homebrew setup, and any tools you've installed to make sure they're working properly. If they are, Homebrew tell you

	Your system is ready to brew.

If anything isn't working properly, follow their instructions to get things working correctly.

**Note**: If there are two lines inside any of the code blocks in this article, paste them separately and hit enter after each of them.

Next you'll need to go in and edit  `~/.bash_profile` to ensures you can use what you've just downloaded. `bash_profile` acts like a configuration file for your terminal.

**Note**: There are many editors available on your computer. You can use a pretty graphical editor like [SublimeText2](http://c758482.r82.cf2.rackcdn.com/Sublime%20Text%202.0.1.dmg) or you can use one built-in to your terminal, like [`vim`](http://www.vim.org/docs.php) or [`nano`](http://www.nano-editor.org/dist/v2.2/nano.html). We'll be using `nano` for this tutorial just to keep things simple.

Open your `bash_profile` with the following command.

	nano ~/.bash_profile

Then copy and paste this line of code at the very top. This lets Homebrew handle updating and maintaining the code we'll be installing.

	export PATH=/usr/local/bin:$PATH

Once you've added the line of code, you can save the file by typing pressing control + O. Doing so lets you adjust the file name. Just leave it as is, then hit enter to save. Hit control + X to exit. You'll find yourself back at the command line and needing to update your terminal session like so. Copy and paste the next line of code into your terminal and hit enter.

	source ~/.bash_profile

You'll only need to source the `bash_profile` since we're editing the file right now. It's the equivalent of quitting your terminal application and opening it up again, but `source` lets you soldier forward and setup Python.

## Chapter 2: Install Virtualenv

Virtualenv isolates each of your Python projects in their own little sandboxes, keeping your installed software neat and tidy. Your Mac comes pre-packaged with the most stable version of Python, but you'll need to tell your `bash_profile` to use it first. Edit the file again with `nano` and add this line:

	export PATH=/usr/local/lib/python2.7/site-packages:$PATH

Update your session again

	source ~/.bash_profile

Next, you'll need to install `pip`. Like Homebrew, it's sort of an app store but for Python code.

	sudo easy_install pip

We use `sudo` to install this software for everyone who might use your computer. `sudo` lets you install things as the admin.

	sudo pip install virtualenv virtualenvwrapper

**Note**: `virtualenv` is the actual environment that you'll be using, while `virtualwrapper` helps you access the environment and its variables from your `PATH`.

Edit your `~/.bash_profile` file again,

	nano ~/.bash_profile

and add this line below the line you just added:

	source /usr/local/bin/virtualenvwrapper_lazy.sh

**Sanity Check**: Double check your `~/.bash_profile` file, and make sure you've properly saved your `PATH` variables.

	less ~/.bash_profile

It should look like this:

	export PATH=/usr/local/bin:$PATH
	export PATH=/usr/local/lib/python2.7/site-packages:$PATH
	source /usr/local/share/python/virtualenvwrapper_lazy.sh

To exit `less`, press "Q".

## Chapter 3: Set up Node and NPM
Finally, we'll install a tool called LESS that we use to write CSS, the language that styles websites. LESS is a built with Node, so we'll need to install that and NPM, Node's version of `pip` or Homebrew.

Install Node using Homebrew.

	brew install node

Install NPM, the Node Package Manager.

	curl https://npmjs.org/install.sh | sh

Finally, add Node to your `~/.bash_profile` like you did for Homebrew and virtualwrapper. Copy and paste the following line below the previous two.

	export NODE_PATH=/usr/local/lib/node_modules

Save and exit out of `nano` using control + O, enter, and then control + X. Then type `source ~/.bash_profile` one more time to update your session. After that, you can treat yourself to a cup of coffee because you now have the basic tools for working like the NPR news apps team. Next up we'll be getting into the nitty gritty of working with the template, including things like [GitHub](https://help.github.com/articles/set-up-git) and [Amazon Web Services](http://aws.amazon.com/).

### Appendix 1: Postgres and PostGIS
We occasionally make maps and analyze geographic information, so that requires some specialized tools. This appendix will show you how to install the Postgres database server and the PostGIS geography stack &mdash; which includes several pieces of software for reading and manipulating geographic data. We'll explain these tools a bit more as we install them.

#### NumPy
First, we need to install a Python library called NumPy. We don't use NumPy directly, but PostGIS uses it for making geographic calculations.

    sudo pip install numpy

#### Postgres
Next up: the Postgres database server. Postgres is a useful tool for dealing with all kinds of data, not just geography, so we'll get it setup first then tweak it to be able to interpret geographic data.

	brew install postgresql

Edit your `~/.bash_profile` to add a pair of commands for starting and stopping your Postgres database server. `pgup` will start the server; `pgdown` will stop it. FYI You'll rarely ever need to `pgdown`, but we've include the command just in case.

	nano -w ~/.bash_profile

Add these two lines:

	alias pgdown='pg_ctl -D /usr/local/var/postgres stop -s -m fast'
	alias pgup='pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start'

Update your session one more time, 

	source ~/.bash_profile

and let's initialize your Postgres server. We only need to do this once after installing it.

	initdb /usr/local/var/postgres/ -E utf-8

Finally, let's start up the Postgres server.

	pgup

#### PostGIS
These deceptively simple commands will install an awful lot of software. It's going to take some time, and your laptop fans will probably sound like a fighter jet taking off. Don't worry; it can take the heat.

	brew install gdal --with-postgres

Still hanging in there? 

	brew install postgis

Now you can create your first geographically-enabled database. For more information on how to do that postgis [tells you how to do this](http://postgis.net/docs/manual-2.0/postgis_installation.html#create_new_db_extensions).

### Appendix 2: The Terminal
Since you're going to be working from the command line a lot, it's worth investing time to make your terminal something that's a little more easy on the eyes.

#### iTerm2
Download [iTerm2](http://www.iterm2.com/#/section/home). The built-in terminal application which comes with your Mac is fine, but iTerm2 is slicker and more configurable. One of the better features is splitting your terminal into different horizontal and vertical panes: one for an active pane, another for any files you might want to have open, and a third for a local server.

#### Solarized
[Solarized](http://ethanschoonover.com/solarized/files/solarized.zip) is a set of nice, readable colors. Unzip the `solarized.zip` file.

Now, inside iTerm2 go to iTerm > Preferences > Profiles and select "Default." Choose "Colors" and find the "Load Presets…" button at the bottom of the window. Select "Import" and navigate to `solarized/iterm2-colors-solarized/` and double-click on `Solarized Dark.itermcolors`. After it's been imported, you can find "Solarized Dark" on the "Load Presets" list. Click and select "Solarized Dark" to change the colors appropriately.

![You can edit your theme from the Preferences menu](/img/posts/a2_solarized.png)

See? Much nicer.

### Appendix 3: The Text Editor
Since your code is stored entirely as text files on your computer, you'll want a nice text editor. Our instructions showed you how to use `nano`, a text editor that you'll find on almost every computer. However, there are at least two others that the team uses. Text editors are like the Microsoft Word of the programming world, except they come packed with all kinds of handy dandy features to make writing code a synch.

#### SublimeText 2
If you're more comfortable with an editor that you can open up like Word, [SublimeText2](http://www.sublimetext.com/2) has a sweet graphical user interface and some [nice customizations](http://net.tutsplus.com/tutorials/tools-and-tips/sublime-text-2-tips-and-tricks/) available. You'll likely want to learn some [keyboard shortcuts](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html) to make yourself more efficient. You can also prettify it with the [Flatland theme](https://github.com/thinkpixellab/flatland)

#### Vim
Personally, I prefer vim &mdash; a terminal based editor that requires you to type rather than point-and-click to work on files. It comes pre-installed on your computer, but there's a lot of little keyboard shortcuts you'll need to get comfy with before you can just dive-in. Here's a nice [cheat sheet](http://www.tuxfiles.org/linuxhelp/vimcheat.html), though, you might want to keep open in a tab if this is your not familiar with it. You can add all kinds of features, but our teammate Chris recommends [nerdtree](https://github.com/tpope/vim-surround) and [surround](https://github.com/scrooloose/nerdtree). Here are [some videos](http://net.tutsplus.com/sessions/vim-essential-plugins/) to help make vim and those particular add-ons.

## Conclusion
And with that you now have a sweet hackintosh. Happy hacking, and if you haven't setup a [Github](https://github.com/) account, you can try out your new tools and [play with some of our code](https://github.com/nprapps). Github provides [a thorough walkthrough](https://help.github.com/) to get you setup and working on some open sourced projects.

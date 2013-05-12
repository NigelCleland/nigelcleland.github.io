---
layout: post
title: "Setting up Octopress with Ubuntu"
date: 2013-05-08 19:41
comments: true
categories: 
- octopress
- rbenv
- ruby	
---

Right, so this post will serve as a general overview as to a couple of the difficulties I had when setting up Octopress and Ruby.
I mainly use Python for my day to day analysis work and as such have little experience with Ruby.
I used rbenv for my installation and a couple issues arose which were a bit of a pain to sort out as googling appeared to let me down here.

<!-- more -->

So a couple things. If you are using Ubuntu then you need to use .profile instead of .bash_profile. 
If done right the bottom two lines of the following should be.

``` 
cat ~/.profile

export PATH="$HOME/.rbenv/bin:$PATH
eval "$(rbenv init -)"
``` 

Another issue which may arise is the difference between the system, and rbenv ruby installation.
The commands you are looking for here are as follows.

``` 
rbenv versions # Will list the installed versions of rbenv
rbenv local <build> # Will set a local version of ruby for the specific folder
rbenv global <build> # Specify the global build of ruby to use.
ruby --version # List the current version of ruby
```

I recommend playing around with each of these commands until you're comfortable switching between different versions of ruby.
In my initial installations I was having a lot of difficulty with different versions of ruby in different locations.
Generally, just a major annoyance to say the least.

Once these two issues were sorted out installation was pretty simple overall.
Hopefully if anyone else is having similar difficulties this post may point them in the right direction.

I'll update this post if needed or if I remember something new.




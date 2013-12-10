---
layout: post
title: "Running uberwriter from the command line"
date: 2013-12-10 16:14
comments: true
categories:
---

Uberwriter is a beautiful markdown textwriter which has two greater features I find essential for writing.

1. Full screen mode
2. Focus mode

Full screen mode does exactly what you'd expect it to. But Focus mode is something quite cool. It will automatically keep the sentence you are writing, and the line you are on highlighted and centered. It then dulls the outside text.

In terms of actually writing it is exceptional. Terrible for editing though but that's expected. Unfortunately uberwriter doesn't come with a command line expression when it is installed. Furthermore it is installed in a slightly weird location.

The command you need to run uberwriter is:

```
/opt/extras.ubuntu.com/uberwriter/bin/uberwriter
```

I prefer to keep such commands as an alias as it makes life on my own computer much easier at the expense of working on others much more difficult.

```
subl ~/.bash_aliases
```

Then add a line:

```
alias "uber=/opt/extras.ubuntu.com/uberwriter/bin/uberwriter"
```

Now you can use uberwriter from the command line just by typing uber and a filename, or by itself.

```
uber /path/to/super/awesome/file.md
```

Hope this is helpful to anyone


---
layout: post
title: "Why Researchers should strive to develop repeatable code"
date: 2013-05-20 11:06
comments: true
categories: [programming, research]
---

As a researcher you'll often have to conduct a lot of smaller experiments, testing out a hypothesis here, running some data analysis there. Yet a lot of people I know, and I am personally guilty of this as well, do not make these scripts repeatable. Writing smart, testable, repeatable code takes longer and for many researchers who code out of necessity, not out of love it is not as intuitive. In many undergraduate degrees, particularly in engineering, the goal throughout the course of study is more to finish the assignment, pass the paper and repeatability be damned.

However, this behaviour becomes counterproductive once you start researching. I cannot imagine the number of hours I have wasting having to repeat useless boiler plate, hard coded examples when some intelligent preparation work, although taking a bit longer initially would save me far more time in the long run.

<!-- more -->

The temptation is to justify taking the quick route through the rational realisation that a lot of the code you write will often be thrown away. Therefore, what is the point in making sure it is repeatable if we may throw out the hypothesis and start again anyway? Here, focus upon the methods you are applying, know that data you are applying it to and take a couple lessons from functional programming.

For example, you may create a great hypothesis which works for your current set of data. However, you've screwed up and hard coded a bunch of exceptions, magic numbers and other such parameters into the code. Elements you didn't need to. Wouldn't it be great if you could simply point your magical, world breaking, code at a new source of data and let the computer sort it all out?

In my opinion this is what researchers should be striving to do. To separate content and form so to speak from one another. If you are developing a model, generalise the parts of it and then set it up so they are easy to connect. The model itself should update itself depending upon what data you feed it and shouldn't be limited as such. By arbitrarily limiting it you're really just kicking yourself later on down the road.

So, how can we avoid screwing up, and start developing elegant repeatable code? Let's go through a couple options.

* Folder structure and file naming
* Version Control
* Smarter development of objects and functions
* Modules and packages
* Tests

## Folder Structure and File Naming

Folder structure, or directory layout, seems like a strange thing to be placing upon a list of developing elegant code. Same with file naming. So what does this have to do with research and developing code? Quite simply a lot. Let's talk about the initial folder layout I use a simple three tier approach as follows. 

Note, that here the data is for trivial examples. If you're using anything more complex then I highly recommend you start researching into Databases and whether your data is suitable for them. However, one caveat is that introducing a Database layer may add significant overhead and if you are unskilled this may be beyond you. Nonetheless, give it a try, see if you like it. As a general rule of thumb think about how much information you are needing to store. 10 MB, you may be okay with a file. 100 MB, pushing it, possibly okay if you're using a library like pandas and vectorised calculations. 1000 MB, Databases start becoming your friend very very quickly. Additionally, you should ask yourself whether other people will also need access to the data sources.

```
data
	- set_one
	- set_two
scripts
	- experiment_one
	- experiment_two
modules
	- module_one
	- module_two
```

Now, what I've done is I've completed separated the three key aspects of my work. All of my data is contained within its own folder. I don't touch it and I certainly don't change any of the files within once they've been modified. What this ensures is that if I chose to repeat something I can be sure that my dataset hasn't changed, and that assuming that the code has changed my program and code should just *work*.

Secondly, I've separated out my scripts and modules. Here we should define what each is. A module is something you've developed which is testable and method independent. It may be a package you've got off [github](github.com) or something you've developed yourself, such as a useful package of [tools](github.com/NigelCleland/pdtools). Now, your scripts on the other hand are the nitty gritty of what you do. You can still write these in a terrible fashion however at least the tools you use, and the data you run it on should be easily separable.

## Version Control

What is Version Control and why, as a researcher, do I care? Ever worked with other people and what ends up getting passed around is a ton of files with names such as ``` file_name_person_number_44_date_37_final_copy_3.ext ```. If yes, and if this made you absolutely hate life and become seriously concerned about the future well being of the human race then version control is for you!
Now, I recommend using what people are comfortable with, however if you're new to it then my recommendation is to pick up git.

Git is a decentralised version control system which makes merging together branches etc easy. Don't know what any of that means? Then pick up a copy of the progit book from github and get reading. I'm not going to go through all of the nitty gritty here, just to say that you should be using it and if you're not then file name hell may be for you! Importantly, version control lets you do as the name suggests and manage different versions of something in a sane easy to implement manner. What is great about it is if you use a plain text based document system, such as LaTeX then you can also manage your documents with it. This makes storing different versions simple (e.g. define a branch as 0.1.0) and you'll always be able to return to that branch for prior iterations.

## Smarter development of objects and functions

Whenever you need to hard code something in to your scripts you should ask yourself. Do I really need to be doing this? Is there a better way? Can I write this in such a way that I will understand it a week from now, a month from now? Code should be written so that it is easy for you to understand, not the machine. If at a later point you need something to be faster then by all means start optimising things (e.g. move towards C). However, initially write it so that you can understand it. Only rewrite the truly performance critical parts.

Here things which help include, keyword arguments instead of hard coded parameters. Comments, docstrings, shorter functions. Resist the urge to develop so called "God" functions or classes that do everything. Instead, separate these (where sensible) into smaller reusable functions or methods. Obviously this is context specific and I'm not going to give too many specifics.

Just imagine future you is standing behind you with a shotgun and will pull the trigger if he doesn't understand what, or why, you are doing something in particular which is undocumented. The other key element here is to avoid, where possible, scope creep.

Don't just randomly and haphazardly add new methods, or functions to a script and make it do something completely unexpected because it was easier at the time. Separate out the components and then join them together in an intelligent fashion.

## Modules and packages

Modules and packages are simply reusable code. They're broadly use case independent and are therefore fantastic for saving time. There are hundreds of packages out there which will simplify your life and make you more productive. My advice is to use them.

My second piece of advice is to take those functions which you use again and again and create your own custom packages. You don't have to release this to the general public but a toolbox which makes your life far far easier is not something to be dismissed. The code you write which goes into your module should be the best code you've written. You're going to be using it hundreds of times and you want to know exactly what it will do in all situations.

## Tests

Finally we come to the worst aspect of coding from my personal point of view. I know some people love writing tests, and enjoy test driven methodology however I see a key difference here. A lot of the people who enjoy test driven coding are writing software. In research we're often developing a number of small scripts which we use to generate results. Quite simply we don't have the same sense of scale, or requirements, as other programmers.

Therefore, where are tests useful? Tests are most useful in your modules and packages, and less useful in your custom scripts. My general rule of thumb is as follows, if I'm going to use this in multiple places and multiple situations then I am going to want a test for each of those places and situations. If I don't do this then I may make a modification for one particular use case forgetting that you're depending upon the previous functionality for a separate usecase. A test, if implemented and properly, would catch this behaviour and you can hopefully develop smarter.

However, the key element here is that you're reusing code. If you are using it once, in a very specific situation then the code itself almost becomes a test. If it works, then the function is correct. If it doesn't work, the function is wrong. As there is only one use case is there much value in developing a test which repeats our use case?

## Conclusions

In this post I've argued, and laid out a few examples, of why and how you should be writing more repeatable code as a researcher. The list is by no means complete and is still a work in progress. I am after all still learning how to write better code, something which will continue as long as I am writing code.

Feel free to ask any questions in the comments.
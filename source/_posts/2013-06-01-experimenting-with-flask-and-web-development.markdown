---
layout: post
title: "Experimenting with Flask and Web Development"
date: 2013-06-01 18:09
comments: true
categories: [python, flask, web development]
---

I'm currently experimenting with using Flask for a bit of Web Development.
I've never done web dev before and it has been an interesting learning experience so far. I'm going to create a fairly simple tracking app for the gym etc, basically get a Database set up and some logging going on.

<!-- more -->

Once this is done, I'll try and extend it and get some fancier features going on, visualisations etc. It will be interesting working with these to see how python can be used in such a fashion. May end up dictating some of the other work I do as well which I may want to expose via websites.

This post is currently a work in progress, where I'm up to so far

## Flask

Flask is a python micro framework which is used for web applications.
It's small, contains very few lines of code, and relies heavily upon two other libraries Werkzeug and Jinja. I'm currently implementing the project using a module type approach as follows

```
trackme/
	runserver.py
	trackme/
		static/
			style.css
		templates/
			# templates
		__init__.py
		database.py
		views.py
		models.py
		forms.py
```

Currently:

* models contains the SQLAlchemy tables
* database contains the SQLAlchemy setup infor
* views contains the flask page views
* forms contains the WTForms Forms
* __init__.py contains the app itself

## SQLAlchemy

SQLAlchemy is a library which is used to create a bit of magic when it comes to SQL databases. Currently I'm just running a local SQLite database and I'm not too too worried about anything surrounding this as this is just a personal fun project at the moment.

## WTForms

WTForms, this is designed to make Forms far far easier to use. I'm still getting my head around a lot of this but I've made good progress so far.

## Templates: Jinja 2

I'm currently not really sure how to create my own templates, nor what these are really doing but it's something I plan on learning more about as I go.

## CSS

I don't really know a hell of a lot about CSS but I guess I'm going to have to find out!

## Plotting

I'm currently not to sure what I'm going to use for plotting things out, I'm currently leaning towards either d3.js or vincent. I've been looking for an excuse to try something apart from matplotlib for awhile now so will just have to see how this goes.

## To do:

Wow, the list of things to actually get done to make this up and running are intense. I guess I can separate these into broad functionality steps.

1. Ability to add workouts quickly and easily
1a. Begin writing tests to ensure that everything works
2. Ability to view workouts easily
3. Add support for profiles
4. Figure out templates, more html and templates to beautify the app a touch.
5. Implement graphical plots for progress
6. Add measurements, e.g. weight/size etc to help track progress
7. Begin extending the app to track other aspects (e.g. food/stretching/w.e.)


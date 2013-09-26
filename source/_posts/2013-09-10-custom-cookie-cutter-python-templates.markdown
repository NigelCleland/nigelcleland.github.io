---
layout: post
title: "Custom cookie cutter python templates"
date: 2013-09-10 17:42
comments: true
categories:
---

I've been experimenting lately with custom cookie cutter templates for developing python projects.
Now, what is a cookie cutter template?
Well, it's a simply template which capitalises upon the excellent cookiecutter tool developed by Audrey Roy.
What the tool does is use jinja2 templates in order and a .json file in order to create a basic outline of a project.

<!-- more -->

Sounds simple right?
And indeed it is, especially since Audrey has already developed a master pypackage template which is quite useful for ordinary projects.
However the issue I've found with most of these is that a lot of the work I do has C dependencies in the scientifc stack, e.g. numpy, scipy, matplotlib, pandas etc.
These dependencies tend to absolutely crap themselves, especially when using Sphinx on an online documentation site such as Read the docs.
This occurs as RTD doesn't build C extensions for you, so you need to make a few special requirements in order to get the imports working properly.
Really frustrating to say the least.

Furthermore, the default Sphinx docstrings method is somewhat archaic and has a numpy of kind of awkward formatting requirements.
What I wanted was a template which would automatically create what I wanted for a data analysis python project.

Ideally I'd have the following requirements:

1. Automatic installation of core dependendencies in a virtualenv.
    + Numpy
    + Pandas
    + Matplotlib
2. Updating the Sphinx documentation to be able to handle mathematical equations as well as numpydoc style formatting. This requires the following:
    + numpydoc
    + mathjax
3. An updated MakeFile to handle some of the new functionality

So, what I've developed is an updated fork of Audreys pypackage template which handles these updates.
I've named it, entirely creatively of me, (cookiecutter-pypackage-scistack)[https://github.com/NigelCleland/cookiecutter-pypackage-scistack].

# LaTeX Templates

Now, given that we can create these excellent Python templates, wouldn't it be great to have the same for LaTeX templates?
A lot of the things required in a LaTeX document are cruft to get set up.
It would be fantastic to not have to deal with this.
Furthermore, one of the biggest issues I've had is getting multiple formatted versions of the same document up and running.

Now, I also want to use git to manage my documents and version control.
I recommend using bitbucket for this as they provide free private repos.
Thus, it makes sense to create a template which handles the basics.
Now these are still a complete work in progress but I figured I'd write about them.

## Enter the cookiecutter-latex-* family.

### Journal

The (cookiecutter-latex-journal)[https://github.com/NigelCleland/cookiecutter-latex-journal] package contains a brief template to setting up Journal Articles.
Currently supports a plain and a IEEE style, upcoming styles to support include the Elsevier as well as others which I may find useful.
The goal of this subject is to have a single, primary source of content, e.g. the main.tex file (Note that this may import chapters etc for larger files).
This file is then called by a number of containers which will create the necessary PDFs from this via a MakeFile.

Now the advantages of this template is that formatting and content are separate entities.
Multiple versions of the same document can be produced automatically.
Designed to be used with git for version control for more flexibility and power.
Single unified source of images and tables. (Note that I included a separate tab file as I generate most of my tables from csv files using a custom tool I'm going to write about soon once I tidy it up a bit.)

### Still to come:

Now, I'm also looking at extending this family to include more general templates.
My currently anticipated use cases are broadly the Memoir, Report and Beamer classes for books, reports and presentations respectively.
Most critical to me is the Beamer template which I'll most likely implement next.
Would be very cool to have a number of templates automatically implemented for
different styles although the author will likely narrow this down to one style eventually.

# Where to next:

What I'm most excited about with cookiecutter is the upcoming hooks feature.
This feature allows custom scripts to be run both before and after initialising the repo.
Would be great to have this automatically create a repo via the github api, run the initial commit and push the data up automatically to save an additional step.
Rinse and repeat this for read the docs integration and other services as well.
In short lots of interesting stuff.

A lot of my code is still rough as I've been hit by a lot of stuff lately so I haven't had the opportunity to polish as I would like

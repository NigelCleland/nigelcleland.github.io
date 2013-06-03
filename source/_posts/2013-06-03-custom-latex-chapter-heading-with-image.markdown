---
layout: post
title: "Custom LaTeX chapter heading with image"
date: 2013-06-03 19:32
comments: true
categories: [latex]
---

A brief command to incorporate an image as part of a chapter heading, and position it so that it makes sense. This took about 30 minutes of googling to pull together the appropriate pieces, not a great use of time but I learnt a couple new LaTeX tricks while I was doing it so that's all good.

<!-- more -->

What I wanted was the ability to include an image directly above a chapter title in a LaTeX report. This was so that I could define both a chapter title and image and have everything just "work" and be scaled appropriately. I should note, that this technique works best with wide screen images. In particular, for consistent sizing of the images each one should have the same resolution (otherwise it will scale oddly).

Furthermore, the command also supports an appropriate inclusion in the table of contents which is nice and means we can just use it as a stand alone command.

This is best demonstrated via example as follows

``` latex
\documentclass[12pt, a4paper]{report}

\usepackage{titlesec}
\usepackage{geometry}
\usepackage{graphicx}

\newcommand*{\imgchapter}[2]{
  \refstepcounter{chapter}
  \newgeometry{top=0cm}
  \addcontentsline{toc}{chapter}{\protect\numberline{\thechapter}#1}
  \chaptermark{#1}
  \begin{center}  
  \makebox[\textwidth]{\includegraphics[width=\paperwidth]{#2}}
  \end{center}
  {\normalfont\huge\bfseries
  \chaptertitlename\ \thechapter:  #1}
  \restoregeometry
}

\begin{document}
\tableofcontents
\imgchapter{This is a test chapter}{test.png}
\end{document}
```

And we can also check out the finished product as well

{% img /images/june2013/chapter_test.png %}

As you can see we've introduced an image above our chapter heading, the table of contents is still all set up and generally life is good.
Now, if the above code isn't working properly please make sure the appropriate packages are installed. Additionally, I did have to remove a few percentage signs as they appeared to be making the ruby parser bork or something along those lines.

One thing you may want to change is that I prefer the Chapter X: chapter title to all be on a single line. Whereas the default LaTeX style has them on separate lines.

Sources, aka where the real credit lies

Image Manipulation: 
[Will Robertson](http://tex.stackexchange.com/a/3548),
[Werner](http://tex.stackexchange.com/a/39148), 
[Schweinebackge](http://tex.stackexchange.com/a/35865)

Chapter Changes: [Alan Munn](http://tex.stackexchange.com/a/25031)

Margins: [Kevin Chen](http://stackoverflow.com/a/16259351)

Hope this is helpful
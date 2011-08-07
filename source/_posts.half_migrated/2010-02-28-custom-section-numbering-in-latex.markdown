--- 
layout: post
title: Custom Section Numbering in LaTeX
tags: 
- university
- latex
---
For our last coursework (which was for a really interesting [compilers](http://www.cs.nott.ac.uk/~nhn/G52CMP/ "G52CMP") module), I chose to present my answers in [LaTeX](http://www.latex-project.org/ "LaTeX"). It's been a very steep learning curve, but I'm extremely happy with the results. I don't even mind the slight drop in productivity that's caused by my uncontrollable urge to stop and admire my document every so often! One thing that had me stuck for a while, however, was the automatic section numbering system.

To answer the questions for the compilers coursework, I wanted my _section_s to be numbered (1, 2, 3), my _subsection_s to be numbered (a, b, c), and my _subsubsection_s to be numbered (i, ii, iii). This numbering pattern matches that which is given in the question paper, whereby a number of nested _enumerate_s were used. The default LaTeX section numbering however is (1, 1.1, 1.1.1). I finally found a way to alter this default behaviour by using the following code in the preamble:
<pre lang="latex"> \renewcommand{\thesubsection}{(\alph{subsection})}

\renewcommand{\thesubsubsection}{\roman{subsection}.}</pre>

This code modifies the way that the counters for subsections and subsubsections are printed (section counters are correct when left at the default).

I hope this saves someone even the few minutes it took me to work this out, once I'd applied some logic! It's very useful to know that LaTeX has a counter for **everything**. [This particular post](http://texblog.wordpress.com/2007/07/25/counters-in-latex/ "Counters in LaTeX") was very helpful in working out which ones to alter, and how.

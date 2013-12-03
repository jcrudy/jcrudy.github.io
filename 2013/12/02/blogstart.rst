Blogstart
=========


.. author:: default
.. categories:: none
.. tags:: none
.. comments::


Once in a great while a new blog is born, and today these rusty tubes are graced with a sparkling new presence.  It's a little rough right now, but greatness can grow from meek beginnings.  Let us take a moment to remember some of the great weblogs of the past, to whose unfathomable heights this humble site may hope one day to spy on the horizon.  `Pythonic Perambulations`_, `While My MCMC Gently Samples`_, and all the rest, may I follow in your wise footsteps.  This blog will be much in the spirit of these gentle giants.  It will concern my work and particularly my Python code.  It will contain short pieces of opinion, medium length demonstrations of technology, and longer analyses of data.  It will not be limited to Python or even to programming and statistics, but will tend to concern those topics more than any other.  At least that is my intention.

Blog Rules [#f1]_
-----------------

Every blog needs rules.  Here are some.

1. Trust no one.

2. Posts occur at least once per month.

3. Nothing is final.  If you want to see revision history, the `blog is hosted on github`_.


Blog Implementation
-------------------

This blog, at least at the moment, is constructed using the Tinkerer_ static blogging framework.  Why Tinkerer you ask?  When choosing a static blogging framework I considered the following options: Octopress, Jekyll, Hyde, Pelican, and Tinkerer.  I ruled out Octopress and Jekyll easily enough.  Although they're both popular, they're not Python and it would therefore be hard for me to read and understand the source code should the unthinkable occur.  Of the three remaining, I chose Tinkerer because it is based on Sphinx.  Sphinx is a mature and well supported Python tool for generating documentation.  Its main selling point for me is the easy support for math, syntax-highlighted code, bibtex, footnotes, and basically every other imaginable feature of a document via an impressive collection of plugins.  The downside of Tinkerer is that it's relatively small userbase has produced very few nice looking themes.  However, I've decided theming is a secondary consideration for now.  Perhaps I can help to remedy the situation in the near future.



.. [#f1] As with any ordered set of rules, the zeroeth rule is that there are no rules.

.. _Pythonic Perambulations: http://jakevdp.github.io/
.. _While My MCMC Gently Samples: http://twiecki.github.io/
.. _blog is hosted on github: https://github.com/jcrudy/jcrudy.github.io
.. _Tinkerer: http://tinkerer.me

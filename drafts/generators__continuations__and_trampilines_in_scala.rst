Generators, Continuations, and Trampolines in Scala
===================================================



.. author:: default
.. categories:: none
.. tags:: none
.. comments::


For the past two weeks or so, I've been experimenting with Scala.  Coming from Python, I have certain expectations about code being intuitive and readable.  Python has a great set of language features and standard libraries that contribute to its usability.  Today I want to talk about one language feature in particular that makes Python particularly pleasant to work with.  I'm speaking, of course, of generators.  It turns out there is no Scala equivalents, and because I'm trying to learn Scala it seemed like a worthwhile endeavor to try to build one.  This post is about my journey.

As with all great adventures, this one began with a simple google search.  


.. code-block:: python

	
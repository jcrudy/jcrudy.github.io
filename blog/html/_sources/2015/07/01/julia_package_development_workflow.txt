Julia package development workflow
==================================



.. author:: default
.. categories:: none
.. tags:: julia
.. comments::

In an effort to make a better MARS package and learn Julia, I've been making a MARS package in Julia.  I had a hard time figuring out how to set up my development environment and workflow.  What software do I need?  How do I configure it?  Where do I put stuff?  How do I run my tests?  This post is about my answers to those questions.  I'm using Mac OS 10.9.5.

Software installation
---------------------

I'm using Juno to edit my Julia files.  Installation is pretty easy.  

1. Install the latest Julia and Juno from the `download site`_.  
2. You then have to tell Juno where to find Julia.  I guess it comes bundled with its own Julia, which will only lead to problems.  See `the docs`_ for more details on this:
	a. Start Juno.
	b. Press Ctrl-Space.
	c. Type "Settings: User behaviors" and press Enter.
	d. Add [:app :lt.objs.langs.julia/julia-path "/path/to/julia"] to the settings file that appears, where "path/to/julia" is replaced with the path to the Julia you just installed.  In my case, the path is "/Applications/Julia-0.3.10.app/Contents/Resources/julia/bin/julia".


Project setup
-------------

Julia projects are easy to set up if you just want to write scripts or interact with IJulia.  Writing packages is a little trickier.  Here are steps to get you started.

1. From Juno, 
	a. Type Ctrl-Space.
	b. Type "spawn" and press Enter.
2. From the spawned Julia terminal,
	a. Type Pkg.generate("MyPackage", "MIT").  Here "MyPackage" is the name of your package and "MIT" is the desired license.  If you don't like the license, you can change it later.
3. Julia generated a skeleton for your new package and put it somewhere in your .julia directory.  It kind of sucks having to work with your code in some hidden configuration directory, so move the created package to wherever you like and then create a symbolic link from .julia.  Julia won't know the difference.  Also, rename it with a ".jl" extension.  In the terminal, do:

.. code:: bash

	mv ~/.julia/v0.3/MyPackage workspace/MyPackage.jl
	ln -s workspace/MyPackage.jl ~/.julia/v0.3/MyPackage

Your Julia version might be different, so check that.

4. Open your package in Juno.  Go to File->Open folder and select MyPackage from your workspace.


Development cycle
-----------------

Julia automatically created src and test directories, as well as src/MyPackage.jl and test/runtests.jl.  To work on your package, do the following:

1. Edit your code.
2. Edit your tests.
3. Make sure runtests.jl runs all your tests.  People usually use include() or just write all the tests directly in the file.
4. From your spawned julia terminal, do Pkg.test("MyPackage").
5. GOTO 1.


Version control and publishing
------------------------------

The package directory created by Julia is already a git repository.  You can add it to github if you want.  I haven't published any packages yet, but see the `Julia manual`_ for information on how to do it.





.. _download site: http://julialang.org/downloads/
.. _the docs: http://junolab.org/docs/install-manual.html
.. _Julia manual: http://julia.readthedocs.org/en/latest/manual/packages/


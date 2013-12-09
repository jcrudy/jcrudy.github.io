Introduction to IScala
======================



.. author:: default
.. categories:: none
.. tags:: none
.. comments::


Python is an amazingly productive glue language.  It's become popular among data scientists over the past several years, partially because of great libraries like numpy, scipy, pandas, scikit-learn, statsmodels, etc., and partially because so much of data science consists simply of gluing things together.  There is a part of my life, though, that consists of actually implementing machine learning algorithms.  Python does not make that part easy.  In order to write efficient numerical code for Python, you need to move the majority of the computation out of Python and into statically typed, compiled code.  The best solution I've found is Cython, which compiles a Python-like language to C while taking care of the Python C api for you.  I wrote py-earth in Cython and am fairly happy with the result.  However, Cython is still a (really fantastic) hack.  Getting good performance out of Cython code requires knowledge of both C and the internals of Python, and most of the good stuff about Python - passing around functions, object-oriented design, readability - is lost when you start to really optimize.  If you want to combine concurrency with objects, you actually have to start passing pointers around.

I guess what I'm saying is I want to branch out a little.  I'd like to find a language where I can be productive both as a scientific user and as a scientific developer, something modern, new but not too new, mainstream but with just a bit of an edge.  Tall.  Anyway, it seems like there are two new languages out there vying for my affection.  I chose Scala over Julia for two reasons.  Firstly, Scala is a general purpose programming language.  While statisticians and academic scientists are very happy to use single purpose tools like R to do their analyses, data scientists need the ability to actually write software and integrate their analyses into larger projects.  Secondly, Scala is implemented on the JVM and compatible with Java.  That means I can use any Java library from within Scala and use Scala to write map-reduce queries for an Hadoop cluster without using the streaming interface.

So I'm exploring the Scala ecosystem and learning the language (which so far I think is fantastic).  My goal is to do some data analyses and implement one or two non-trivial algorithms in Scala.  But one step at a time.  Today I want to share IScala_, which is basically just iPython with a Scala backend.  Although I actually rarely use iPython, I know a lot of people do and I suspect that Scala adoption will require the existence of a useful iPython replacement.


Installing
----------

The IScala readme file lists several installation options.  The one that worked for me was as follows.  First, download the `latest tarball`_ and unpack it somewhere.  I put it directly in my home directory.  Then create a scala profile for ipython:

.. code-block:: none


	$ipython profile create scala
	[ProfileCreate] Generating default config file: u'/Users/jason/.ipython/profile_scala/ipython_config.py'
	[ProfileCreate] Generating default config file: u'/Users/jason/.ipython/profile_scala/ipython_qtconsole_config.py'
	[ProfileCreate] Generating default config file: u'/Users/jason/.ipython/profile_scala/ipython_notebook_config.py'
	[ProfileCreate] Generating default config file: u'/Users/jason/.ipython/profile_scala/ipython_nbconvert_config.py'


The output above tells you the location of the ipython_config.py file.  The next step is to edit ipython_config.py to tell iPython about the IScala kernel, as below:

.. code-block:: python
	:emphasize-lines: 5,6,7,8,9


	# Configuration file for ipython.
	c = get_config()

	# Use the IScala kernel
	c.KernelManager.kernel_cmd = ["java", "-jar", 
	                              "$ISCALA_PATH/lib/IScala.jar", 
	                              "--profile", 
	                              "{connection_file}", 
	                              "--parent"]


After you get this working, with $ISCALA_PATH replaced by the path to wherever you put the unpacked IScala download, you should be able to run IScala by saying:

.. code-block:: none


	$ipython notebook --profile scala

or similarly for console or qtconsole.  


Taking it for a spin
--------------------

I'm going to try generating a random matrix with a standard normal distribution.  The first thing I'll need to do is import breeze.  My first attempt failed.


.. container:: iscala

	.. code:: scala

	    import breeze.linalg._
	    import breeze.stats.distributions._
	    

	.. parsed-literal::

	    
	    <console>:7: error: not found: value breeze
	           import breeze.linalg._
	                  ^
	    <console>:8: error: not found: value breeze
	           import breeze.stats.distributions._
	                  ^

.. .. include:: Random Matrix.rst

.. .. figure:: RandomMatrix.png
.. 	:width: 100%


I needed to tell sbt where to find breeze.  It turns out you can use iPython magic to talk to sbt like this.


.. container:: iscala

	.. code:: scala

	    %resolvers += "ScalaNLP Maven2" at "http://repo.scalanlp.org/repo"

	    %resolvers += "Scala Tools Snapshots" at "http://scala-tools.org/repo-snapshots/"

	    %resolvers += "Typesafe Repository" at "http://repo.typesafe.com/typesafe/releases/"

	    %resolvers += "Sonatype Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"

	    %libraryDependencies += "org.scalanlp" %% "breeze" % "0.6-SNAPSHOT"

	    %update

	.. parsed-literal::

	    [info] Resolving org.scalanlp#breeze_2.10;0.6-SNAPSHOT ...
	    [info] Resolving org.scala-lang#scala-library;2.10.3 ...
	    [info] Resolving org.scalanlp#breeze-macros_2.10;0.1 ...
	    [info] Resolving org.scala-lang#scala-reflect;2.10.3 ...
	    [info] Resolving com.thoughtworks.paranamer#paranamer;2.2 ...
	    [info] Resolving com.github.fommil.netlib#all;1.1.2 ...
	    [info] Resolving net.sourceforge.f2j#arpack_combined_all;0.1 ...
	    [info] Resolving com.github.fommil.netlib#core;1.1.2 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-osx-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#native_ref-java;1.1 ...
	    [info] Resolving com.github.fommil#jniloader;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-linux-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-linux-i686;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-win-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-win-i686;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_ref-linux-armhf;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-osx-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#native_system-java;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-linux-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-linux-i686;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-linux-armhf;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-win-x86_64;1.1 ...
	    [info] Resolving com.github.fommil.netlib#netlib-native_system-win-i686;1.1 ...
	    [info] Resolving org.scalanlp#lpsolve;5.5.2-SNAPSHOT ...
	    [info] Resolving net.sf.opencsv#opencsv;2.3 ...
	    [info] Resolving com.github.rwl#jtransforms;2.4.0 ...
	    [info] Resolving junit#junit;4.8.2 ...
	    [info] Resolving org.apache.commons#commons-math3;3.2 ...
	    [info] Resolving com.typesafe#scalalogging-slf4j_2.10;1.0.1 ...
	    [info] Resolving org.slf4j#slf4j-api;1.7.2 ...

	.. code:: scala

	    import breeze.linalg._
	    import breeze.stats.distributions._


That worked!  I can now generate my random matrix like this:

.. container:: iscala

	.. code:: scala

		val x = DenseMatrix.fill(10,10)(Gaussian(0,1).draw())

	.. parsed-literal:: 

		0.4695170376110142   0.5086639312405534    ... (10 total)
		0.2604624080687952   -0.03678938632256435  ...
		-1.0528337756159565  0.10082649287241126   ...
		-0.550492849679171   -0.5761878622563654   ...
		-0.9817603551889219  0.7958446618784706    ...
		-1.0001995322763473  -1.2424889465651479   ...
		-0.5879313146878662  1.206569217055404     ...
		1.890300548243616    -0.30273380341887257  ...
		0.24792587873136573  -0.04329745764599858  ...
		1.5057194826425122   0.9516921598743895    ...



Final thoughts
--------------

So IScala is up and running.  There doesn't yet appear to be any support for displaying plots in the notebook, and it is a little annoying that I have to %update to add new dependencies (which causes the Scala process to restart and all existing objects to be lost from memory).  However, IScala is a new project and these are minor issues.  During the process I discovered something disconcerting about the Scala ecosystem, however.  There is not currently a Scala equivalent of numpy.  That is, there is no basic structure that everyone agrees is the standard backend array type.  Instead, there are several competing packages (of which breeze_ is just one) that accomplish this extremely basic function.  I need to decide which one I want to develop on.


.. _IScala: https://github.com/mattpap/IScala
.. _latest tarball: https://github.com/mattpap/IScala/releases
.. _breeze: http://www.scalanlp.org/



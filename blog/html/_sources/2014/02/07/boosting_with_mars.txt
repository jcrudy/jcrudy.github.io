Boosting with MARS
==================



.. author:: default
.. categories:: python, statistics
.. tags:: none
.. comments::


Someone wrote me a few months ago asking for advice.  He was an R user who wanted to combine the adaboost algorithm with MARS and was starting to look outside the R community.  As a recovering (and occasionally relapsing) R user myself, I was only too happy to look into the matter.  As it turns out, it's actually not that hard to do with a combination of scikit-learn and py-earth.  I now recount the adventure in detail.

Let it be known that this fine community member was interested in MARS not for regression, as is its usual role, but for classification.  This first challenge is easily accomplished using the Pipeline class, as follows.

.. code:: python

	from sklearn.pipeline import Pipeline
	from sklearn.linear_model import LogisticRegression
	from pyearth import Earth

	model = Pipeline([('earth',Earth()),('log',LogisticRegression())])

The above construction is equivalent to using the earth package from R with `glm=list(family=binomial)`.

Adaboost requires a classifier that can handle weighted samples.  The `Earth` class satisfies this requirement.  `LogisticRegression` does not, but we can replace it with `SGDClassifier` using `loss='log'` to get an equivalent Pipeline that can handle sample weights.  Or, at least, it seems like it could.  As it turns out, though, the `Pipeline` class doesn't actually know how to act as a base estimator in `AdaBoostClassifier`.  When you try to fit the resulting model, you get an error.

.. code:: python

	from sklearn.pipeline import Pipeline
	from pyearth import Earth
	from sklearn.linear_model.stochastic_gradient import SGDClassifier
	from sklearn.datasets import make_classification
	from sklearn.ensemble.weight_boosting import AdaBoostClassifier


	X, y = make_classification()
	classifier = Pipeline([('earth',Earth()),('log',SGDClassifier(loss='log'))])
	model = AdaBoostClassifier(base_estimator=classifier)
	model.fit(X, y)


This code produces `TypeError: base_estimator must be a subclass of ClassifierMixin`.  You can get to the next error by making a special subclass of `Pipeline` like so.

.. code:: python

	from sklearn.pipeline import Pipeline
	from pyearth import Earth
	from sklearn.linear_model.stochastic_gradient import SGDClassifier
	from sklearn.datasets import make_classification
	from sklearn.ensemble.weight_boosting import AdaBoostClassifier
	from sklearn.base import ClassifierMixin

	class ClassifierPipeline(Pipeline, ClassifierMixin):
	    @property
	    def classes_(self):
	        return self.steps[-1][1].classes_

	X, y = make_classification()
	classifier = ClassifierPipeline([('earth',Earth()),('log',SGDClassifier(loss='log'))])
	model = AdaBoostClassifier(base_estimator=classifier)
	model.fit(X, y)

Now this code gives `ValueError: need more than 1 value to unpack`.  That's somewhat more inscrutible.  To get around this one you actually have to rewrite part of the `Pipeline` class itself.  I posted a modified `Pipeline` as a gist_.  You can patch it in and use it like this.

.. code:: python

	import pipeline as alt_pipeline
	from sklearn.base import ClassifierMixin
	from pyearth import Earth
	from sklearn.linear_model.stochastic_gradient import SGDClassifier
	from sklearn.datasets import make_classification
	from sklearn.ensemble.weight_boosting import AdaBoostClassifier

	class ClassifierPipeline(alt_pipeline.Pipeline, ClassifierMixin):
	    @property
	    def classes_(self):
	        return self.steps[-1][1].classes_
	X, y = make_classification()
	classifier = ClassifierPipeline([('earth',Earth()),('log',SGDClassifier(loss='log'))])
	model = AdaBoostClassifier(base_estimator=classifier)
	model.fit(X, y)

So that's how you use AdaBoost with MARS in Python.


.. _gist: https://gist.github.com/jcrudy/7493865#file-pipeline-py

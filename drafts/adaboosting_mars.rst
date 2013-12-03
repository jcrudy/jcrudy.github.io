AdaBoost with MARS
==================



.. author:: default
.. categories:: Python
.. tags:: Python, scikit-learn
.. comments::


AdaBoost_ is a sort of classification (or regression) meta-method.  AdaBoost takes as input a set of training data and a classification method.  It produces an improved classifier by repeatedly applying the original classification method and then reweighting the training set to emphasize those examples that were misclassified.  A prospective py-earth user recently wrote to me about using a MARS model as the base classifier for an AdaBoost classifier.  


.. _AdaBoost: http://en.wikipedia.org/wiki/AdaBoost
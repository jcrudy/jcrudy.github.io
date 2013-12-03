Software
========


I've written some software packages that people can download.   Here they are in list form.


py-earth_
	A Python implementation of the MARS_ algorithm written mostly in Cython, compatible with many prominent members of the Python data stack including pandas, scikit-learn, and patsy.


choldate_
	A Python package for performing rank 1 updates and downdates on Cholesky factors.  Because why :math:`O\left( n^3 \right)` when you can :math:`O\left( n^2 \right)`?  It works with numpy arrays from the C side, so it's pretty fast.


glm-sklearn_
	A wrapper to make statsmodels' GLM module compatible with scikit-learn.  Useful if you want to put a GLM in a Pipeline, which, I admit, I have sometimes wanted to do.


CLSOCP_
	An SOCP solver written in R, using a `smoothing Newton method technique`_.


CONOR_
	An R package for cross-platform normalization of microarray data, from back in my thesis_ days, you know.


.. _MARS: http://en.wikipedia.org/wiki/Multivariate_adaptive_regression_splines
.. _py-earth: https://github.com/jcrudy/py-earth
.. _choldate: https://github.com/jcrudy/choldate
.. _glm-sklearn: https://github.com/jcrudy/glm-sklearn
.. _CLSOCP: https://github.com/jcrudy/CLSOCP
.. _smoothing Newton method technique: http://www.sciencedirect.com/science/article/pii/S0377042707006723
.. _CONOR: https://github.com/jcrudy/CONOR
.. _thesis: http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3314675/
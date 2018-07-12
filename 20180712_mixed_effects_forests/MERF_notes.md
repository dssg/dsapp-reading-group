## Mixed Effects Regression Trees and Forests

#### Hajjem et al. 2011, 2014

Motivation

* Hierarchical/ nested data structure
* Cluster variable not useful as feature
    + often many categories/ cluster
    + want predictions also for new clusters
* Improve prediction performance

MERT

* Previous work: some papers on longitudinal trends and trees, Sela and Simonoff (2012)
* MERT
    + y_i = f(X_i) + Z_i b_i + e_i
    + b_i ~ N(0,D), e_i ~ N(0,R_i)
* Fixed part approximated by tree structure
* Linear random part, allows for random intercepts and random slopes
* Fitted via modified EM algorithm where y* (without random part) is handed over to CART to estimate f(X_i)
    1. random effects known, estimate CART tree
    2. CART tree known, estimate random effects
* Prediction
    + known cluster: get fixed part from tree node and add random part based on cluster membership
    + new cluster: use only fixed part

MERF

* Previous work: cluster based bootstrapping
* Same as MERT, but now f(X_i) is estimated by RF and out-of-bag predictions f*(X_i) are used to estimate random effects in next step
* Standard Bootstrapping used based on assumption that random effects totally explain intracluster correlation

Extensions

* Generalized Mixed Effects Regression Trees (GMERT)
* GMERF?

Application

* Simulations: MERT, MERF outperform CART, RF when random effects present
* Real data examples

Implementation

* Python: https://pypi.org/project/merf/
* R: only packages for related methods
    + https://cran.r-project.org/web/packages/glmertree/index.html
    + https://cran.r-project.org/web/packages/REEMtree/REEMtree.pdf

Comments

* Problems of multilevel models may apply here as well
    + issues of small level-2 sample sizes (Stegmueller 2013, Elff et al. 2016)
    + estimation with many random effects can become cumbersome
* Tuning MERFs?

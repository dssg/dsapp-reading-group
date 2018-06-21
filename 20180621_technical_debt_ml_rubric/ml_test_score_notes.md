# Reading group 2018-06-21

## The ML Test Score: A Rubric for ML Production Readiness and Techincal Debt Reduction  (Proceedings of IEEE Big Data, 2017)

Link: https://ai.google/research/pubs/pub46555

* Introduction
  * Goal: 28 'actionable' tests for ML systems
  * "prior work has identified problems but been largely silent on how to address them"
  * "this paper details actionable advice drawn from practice and verified with interviews"
* Tests for features and data
  * Data 1: feature expectations are captured in a schema
    * What are sensible 'feature expectations' - think in terms of statistical priors?
      * Prior support - hard boundary for feasible data values
      * Prior density - "soft boundary" for likelihood of data values
  * Data 2: all features are beneficial
    * Very vague, reduces to "test feature importance / significance"
  * Data 3: No feature's cost is too much
    * How do you quantify the cost of adding a feature? 
      * Highly model dependent
      * Generally nonlinear?
    * How do you measure instability with adding noisy features?
    * Sparsity constraints not typically enforced in overtrained ML models - how to control?
  * Data 4: features adhere to meta-level requirements
    * i.e. "data constraints are uniformly applied through the pipeline"
  * Data 5: data pipeline has privacy controls 
  * Data 6: new features can be added quickly 
  * Data 7: test ETL for feature generation 
* Tests for model development
  * Model 1: Every model specification undergoes a code review and is checked into a repository
    * Best way to version control config files? Private repos? 
  * Model 2: Offline proxy metrics correlate with actual online impact metrics
  * Model 3: all hyperparameters have been tuned 
    * This is an entire field of research, and hardly a trivial question
  * Model 4: the impact of model staleness is known 
    * Evaluating "stale" models - what determines staleness?
      * How much do we expect target distributions to evolve over time?
      * Does a fast decay of "stale" models imply recency bias / overfitting?
  * Model 5: complex models outperform simple models
    * How can we design model groups to best test this?
  * Model 6: model quality is sufficient on all important data slices
    * Slicing by bias categories, at risk categories, other pubpol-specific topics
  * Model 7: model has been tested for consideration of inclusion - see #6
* Tests for ML infrastructure
  * Infra 1: Training is reproducible 
    * What goes into complete reproducibility?
      * Controlling pseudo-random number generation?
      * Controlling live data sources?
  * Infra 2: Model specification code is unit tested 
    * Testing implementations - do we currently have integration tests for `catwalk` models?
  * Infra 3: the full ML pipeline is integration tested 
  * Infra 4: inspect data quality before serving it in model
  * Infra 5: the model allows debugging by observing the step-by-step computation of training or inference on a single example 
    * On the up side: could be useful for showing how `individual_importances` translate into decision
    * On the down side: could yield bias in how to interpret feature importances
  * Infra 6: models are tested via a canary process before they enter production serving environments
    * What distinguishes a "canary process" from a critical integration test?
  * Infra 7: models can be quickly and safely rolled back to a previous serving version 
* Monitoring tests for ML
  * Monitor 1: dependency changes result in notification
  * Monitor 2: data invariants hold in training and serving inputs - duplicate?
  * Monitor 3: Training and serving features compute the same values - duplicate?
  * Monitor 4: models are not too stale - duplicate?
  * Monitor 5: the model is numerically stable 
    * Numerical stability is not limited to ETL issues- see model monitor deep dive
  * Monitor 6: the model has not experienced a dramatic or slow-leak regression in training speed, serving latency, throughput, or RAM usage 
  * Monitor 7: the model has not experienced a regression in prediction quality on served data - duplicate?
* Survey insights
  * Goal is to grade ML systems based on their adherence to the traits above
  * Performed interviews at Google
    * Aside: authors literally include a citation for the statement "checklists are helpful even for expert teams [18]"
  * Common themes:
    * Teams have trouble communicating data ownership, which reduces incentives to do integration tests
    * Model "canarying" occurs long after engineering decisions were made
  * Survey self-assessment
    * "Teams using purely image or audio data did not feel many of the Data tests were applicable" ???
    * How to handle cost (time, data, money, etc.) of acquiring lables? Not addressed at all!

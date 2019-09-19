## Practical Automated Machine Learning for the AutoML Challenge 2018

Authors: Matthias Feurer, Katharina Eggensperger, Stefan Falkner, Marius Lindauer, Frank Hutter

Link: https://ml.informatik.uni-freiburg.de/papers/18-AUTOML-AutoChallenge.pdf

### Abstract:

The article demonstrates the winning entry to AutoML challenge 2018 wiht an algorithm dubbed PoSH Auto-sklearn, which combines an automatically pre-selected portfolio, ensemble building and Bayesian optimisation with successive halving. 

### Introduction
The first AutoML challenge demonstrated that automated machine learning is efficient and can perform better than human experts  (Guyon et al., 2015, 2016). This is an winning entry to the second AutoML challenge (Guyon et al., 2018.)

The first challenge sparked the development of Auto-sklearn (Feurer et al., 2015a) using sklearn  (Pedregosa et al., 2011) as the underlying framework and Auto-WEKA  (Thornton et al., 2013) as the basis. Our approach automatically constructed machine learning pipelines suggested by Bayesian optimisation method SMAC (Hutter et al., 2011), warm-started with meta-learning  (Feurer et al., 2015b) and combined with post-hoc ensemble building to achieve robust performance.

The second challenge is different in that:
- there was only a single auto-phase
- the datasets provided were comparably homogeneous: dense, binary classification and normalised area under the ROC curve as the target metric
- datasets could be marked as being recorded sequentially

### Our apporach: PoSH Auto-sklearn
Particularly in the light of the tighter time constraints, the new framework is developed. ![Image]()

**Successive Halving**
A key issue we identified during the last AutoML challenge was that training expensive configurations on the complete training set, combined with a low time budget, does not scale well to large datasets. At the same time, we noticed that our (then manual) strategy already yielded good predictions for ensemble building. For our new submission, we therefore made use of the recent bandit strategy Successive Halving (SH, Jamieson and Talwalkar (2016)) and also adapted our configuration space to consider classifiers which can be leveraged by SH’s budget allocation. 

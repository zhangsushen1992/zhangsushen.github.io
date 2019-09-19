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
Particularly in the light of the tighter time constraints, the new framework is developed. ![Image](Research/Architectural_search/figures/auto_frame.png)

**Successive Halving**
A key issue we identified during the last AutoML challenge was that training expensive configurations on the complete training set, combined with a low time budget, does not scale well to large datasets. At the same time, we noticed that our (then manual) strategy already yielded good predictions for ensemble building. For our new submission, we therefore made use of the recent bandit strategy Successive Halving (SH, Jamieson and Talwalkar (2016)) and also adapted our configuration space to consider classifiers which can be leveraged by SH’s budget allocation. SH allows to specify a minimum and maximum budget for each configuration (eg in terms of number ob training iterations or dataset size); it starts with the minimum budget and iteratively increases the budget for the pipelines that perform best with the current budget. It is simple to implement.

**Bayesian Optimisation with Successive Halving**
Recently, SH and its extension _Hyperband_ have been successfully applied to hyperparameter optimisation problems (Jamieson
and Talwalkar, 2016; Li et al., 2017), demonstrating strong performance with small budges. Combining Hyperband with Bayesian optimisation yields the state-of-the-art hyperparameter optimisation method BOHB (Bayesian Optimization HyperBand)(Falkner et al., 2018). Since BOHB was shown to outperform both traditional Bayesian optimization without budget considerations, as well as Hyberband and SH by itself, we decided to use it as the optimization workhorse for our submission.

**Portfolio Building**
While our submission to the last AutoML challenge used meta-features to train on seen data and validate on new dataset using the trained configuration, we simplified this process. We started with the same static portfolio for all dataset. The static portfolio is constructed as follows: 421 binary-classification datasets from _OpenML_ (Vanschoren et al., 2014). For each dataset we optimise with pipeline _SMAC_ (Hutter et al., 2011) evaluated on all 421 datasets to obtain 421x421 test score matrix. We then used greedy _submodular function minimisation_ (Krause and Golovin, 2014) w.r.t. the normalized regret across all datasets in this matrix to obtain a portfolio of machine learning pipelines.

**Configuration space**
We adopted dataset preprocessing (feature scaling, imputation of missing values, treatment of categorical values) but not feature prepsocessing), and one of four classifiers: SVM, RF, linear classifcation (via SGD) or XGBoost (Chen and Guestrin, 2016) leading to 37 hyperparameters. We used the number of iterations as the budget, except for SVM where dataset size is the budget.

**Ensemble building**
Ensemble selectionn (Caruana et al., 2004) added too many bad models to the final ensemble. A new regularisation technique similar to library pruning  (Caruana et al., 2006) was added. Specifically, we compare loss of candidate model to single best model and if the relative difference between losses was larger than 3%, we did not consider the model for the ensemble.

**PoSH Auto-sklearn**
The model runs one iteration of SH on the porfolio and then uses Bayesian optiimsation to obtain new configurations for SH. We started ensemble slection in a separate process and continuously updated our ensemble as new models were evaluated.

### Results and Analysis
We managed to evaluate our dataset-agnostic portfolio and to evaluate at least one pipeline on the full budget (ie finish one iteration of successive halving). For three of the datasets we even finished a second iteration of successive halving and for two we also reached the necessary number of function evaluations to build a model for guiding the search. Overall, the results indicate that the portfolio provided a robust and diverse enough set of pipelines, so it was hard to find much better configurations in the remaining time. 

Interestingly, even though we filtered ensemble members much more aggressively compared to our previous approaches (Feurer et al., 2015a), the number of candidate models were still large. This indicates many pipelines performed well on these datasets. The new regularisation technique was very important to prevent overfitting.

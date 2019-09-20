## Evolutionary Neural AutoML for Deep Learning

Authors: Jason Liang, Elliot Meyerson, Babak Hodjat, Dan Fink, Karl Mutch, Risto Miikkulainen

Link: https://arxiv.org/abs/1902.06827

### Abstract:
AutoML systems have been developed focusing mostly on hyperparameter optimisation. This paper introduces an evolutionary AutoML framework, LEAF, that optimises hyperparameter, architectures and size of network. It makes use of state-of-the-art evolutionary algorithms (EA) and distributed computing frameworks. Experimental results on image classification and NLP show state-of-the-art performance. LEAF demonstrates architecutre optimisation provides a significant boost over hyperparameter optimisation. Moreover, network are minimised with little drop in performance. LEAF demoncratises and improves AI, making AI practical in future applications.

### Introduction
There are no guiding principles for selecting DNN architectures for a specific task or task domain. Finding the right architecture and hyperparameters is essentially reduced to a black-box optimisation process. However, manual testing and evaluation is a tedious and time consuming process that requires experience and expertise. The architecture and hyperparameters are often chosen based on history and convenience rather than theoretical or empirical principles. It is unknown whether the network can perform as well as it could. Automated configuration of DNNs is compelling because:
- to find innovative configurations of DNNs that also perform well
- to find configurations that are small enough to be practical
- to enable finding of configurations without domain expertise

Currently, the most common approach to satisfy the first goal is through partial optimisation. The authors might tune a few hyperparameters or switch between several architectures unsimultaneously [24, 49]. This is understandable since the search space is massive and existing methods do not scale as the number of hyperparameters and architecture complexity increases. 

#### Grid Search
The most widely used method is grid search, where hyperparameters are discretized into a fixed number of intervals and all combinations are searched exhaustively. Each combination is tested by training a DNN with those hyperparameters and evaluating its performance with respect to a metric on a benchmark dataset. While this method is simple and can be parallelised easily, its computational complexity grows combinatorially with the number of hyperparameters, and becomes intractable once the number of hyperparameters exceeds four or five [27]. Grid search also does not search for optimal architecture, which is another important aspect to determine performance. A method that can optimise structure together with parameters is needed.

#### Second Goal: Limit Complexity
Application of DNNs in smartphones is often limited due to the few gigabytes of RAM thus the second goal of DNN optimisation is to minimise complexity or size of a network, while simultaneously maximising its performance [25]. Thus, a method for optimising multiple objectives is needed.

#### Third Goal: Democratize AI
Commercial systems such as Google AutoML [1] and Yelp's Metric Optimization Engine (MOE [5], commericalised as SigOpt [8]) have been developed. However, they are limited in scope of problems and feedeback provided to the user. For example, Google AutoML is a black-box that hides architecture and training from user; only an API is provided to query new inputs. MOE only tunes hyperparameters. Neither systems minimise size or complexity.

#### This Paper: based on CoDeepNEAT
This paper presents a novel AutoML system called LEAF (Learning Evolutionary AI Framework) that addresses these three goals. It leverages and extends an existing state-of-the-art evolutionary algorithm for architecture search called CoDeepNEAT [35], which evolves both hyperparameters and network strucutre. The speciation and complexification heuristics inside CoDeepNEAT also allows it to be easilyt adapted to multi-objective optimisation to find minimal architectures. 

#### Application
The effectiveness of LEAF will be demonstrated on two domains: 
- in language: Wikipedia comment toxicity classification (aka Wikidetox)
- in vision: Chest X-rays multitaks image classification



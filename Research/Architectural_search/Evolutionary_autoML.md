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

### Background and related work
#### Hyperparameter Tuning
- Random/Grid search and BO

Random search and grid search can optimise simple DNNs but are ineffective when all hyperparameters are crucial to performance. For such cases, Bayesian optimization using Gaussin processes [44] is a feasible alternative. It requires few function evaluations and works well on multimodal, non-separable and noisy functions where there are several local optima. However, it is computationally expensive and scales cubically with number of evaluated points. DNGO [45] addresses this problem by replacing Gaussian processes with linearly scaling Bayesian neural networks. Another downside of BO is the poor performance when number of hyperparameters is moderately high, 10-15 [32].

- EA

One particular model is CMA-ES [32]. A Gaussian distribtuion for the best individuals in the population is estimated and used to generate/sample the population for the next genearation. Furthermore, it has mechanisms for controlling the step-size and the direction that the population will move. It performs well in real-world high-dimensional optimisation problems and outperforms BO in tuning CNN. However, it is limited to continuous optimisation and does not extend naturally to architecture search.

#### Architecture search for DNNs
- RL

A recurrent neural network (LSTM) controller generates a sequence of layers that begin from the input and end at the output of a DNN [55]. The LSTM is trained through a gradient-based policy search algorithm called REINFORCE [51]. The architecture search space explored by this approach is stufficiently large to improve upon hand-design. On CIFAR-10 and ImageNet, it achieved 1-2 percentage points of the state-of-the-art, and on language modeling benchmark, it achieved state-of-the-art performance [55].

However, 
1) the architecture of the optimised network still must have either a linear or tree-like core structure; arbitrary graph topologies are outside the search space. Thus, it is still up to the user to define an appropriate search space beforehand as starting piont.
2) the number of hyperparameters that can be optimised for each layer are limited
3) computations extremely heavy; thousands of candidate architectures have to be evaluated and trained, requiring hundreds of thousands of GPU hours

- EA 

Black-box optimisation algorithms that can optimise arbitrary structure. Approaches use a modified version of NEAT [43], an EA for neuron-level neuroevolution [46], for searching network topologies. Others rely on genetic programming [47] or hierarchical evolution [31]. There is some very recent work on multiobjective evolutionary architecture search [18, 33], where the goal is to optimise both the performance and training time/complexity of the network.

- EA vs RL

The main advantage of EA over RL is that they can optimise over much larger search spaces. For instance, approaches based on NEAT can evolve arbitrary graph topologies. Hierarchical evolutionary methods can search over very large spaces efficiently and evolve complex architecutres quickly from a minimal starting point. Thus, performnace of EA match or exceeds RL. Current state-of-the-art results on CIFAR-10 and ImageNet were achieved by EA [42]. In this paper, LEAF uses CoDeepNEAT, a powerful EA based on NEAT that is capable of hierarchically evolving networks with arbitrary topology.

### LEAF overview
LEAF composes of three components: algorithm layer, system layer, problem-domain layer.The algorithm layer allows the LEAF to evolve DNN hyperparameters and architectures. The system layer parallelizes training of DNNs on cloud compute infrastructure such as Amazon AWS [2], Microsoft Azure [6], or Google Cloud [3], which is required to evaluate the fitnesses of the networks evolved in the algorithm layer. The algorithm layer sends the network architectures in Keras JSON format [14] to the system layer and receives fitness information back. These two layers work in tandem to support the problem-domain layer, where LEAF solves problems such as hyperparameter tuning, architecture search, and complexity minimization.

#### Algorithm Layer

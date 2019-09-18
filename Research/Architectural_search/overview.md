## Neural Architecture Search (NAS)

Author: Thomas Elsken, Jan Hendrik Metzen, and Frank Hutter

Excerpt from: Chapter 3, Automated Machine Learning pp 63-77

Link: https://link.springer.com/chapter/10.1007/978-3-030-05318-5_3

Current design of architectures are mainly constructed mannually by human experts, making it error prone and time-consuming. Automation of architecture search is, therefore, an important research area. It is a subfield of AutoML and overlaps with hyperparameter optimisation and meta-learning.

Three categories of architecture search include:
- **search space**: Prior knowledge of the task simplifies search and reduces search space, but is susceptible to human bias and is limited by current human knowledge.
- **search strategy**: The search encompasses classical exploration-exploitation trade-off: the need to find well-performing architectures speedily conflicts with the possibility of a premature convergence to suboptimal architectures.
- **performance estimation strategy**: Traditional training and validation is computationally expensive to be performed on a large number of architectures. Current research focuses on reducing cost of performance estimations.

### Search Space

1) **Chain-structured neural networks**:

A sequence of _n_ layers, where the _i_'th layer _L<sub>i</sub>_ receives input from _L<sub>i-1</sub>_ and output to _L<sub>i+1</sub>_. The architecture _A_ = _L_<sub>n</sub>◦...◦ _L_<sub>1</sub>◦ _L_<sub>0</sub>. The search space is parameterised by:

(1) number of layers _n_, 

(2) type of operation (pooling, convolution, etc) 

(3) hyperparamters (number of filters, kernel size, strides, number of fully-connected units). 

(3) depends on (2) hence the search space is conditional.

2) **Multi-branch networks**:

Input to layer _i_ is a function _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>), combining previous layer outputs. Special cases include: 

(1) Chain-structured: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_L_<sub>i-1</sub><sup>out</sup>, 

(2) Residual Networks: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_L_<sub>i-1</sub><sup>out</sup> + _L_<sub>j</sub><sup>out</sup>, _j_<_i_ 

(3) DenseNets: concatenate previous layer outputs: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_concat_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)

3) **Repeated Mortifs**:

Architecture can be viewed as repeated motifs, sometimes dubbed cells or blocks. For example, normal cell preserves dimensionality while reduction cell reduces spatial dimension. Final architecture is constructed by stacking cells in a predefined manner. The advantage of this includes:

(1) Reduce search space.
(2) Cells more adaptable to different datasets.

Problem is how to choose meta-architecture, namely how to connect cells. Ideally the meta-architecture should be optimised automatically as part of NAS. Even search space based on a single cell with fixed meta-architecture is difficult: the optimisation is non-continuous and high-dimensional. Unbounded search space can be constrained by defining maximal depth, generating fixed-size search spaces with conditional dimensions.

### Search Strategy

Search strategies include random search, Bayesian optimisation, evolutionary methods, reinforcement learning and gradient based methods. Early success used Bayesian optimisation for vision architectures.

#### Reinforcement Learning
To frame NAS as RL problem, the generation of neural architecture can be seen as agent's action, with the action space identical to search space. The agent's reward is based on an estimate of the performance of the trained architecutre on unseen data. Different RL approaches differ in agents's policy and optimisation. Zoph and Le [75] use RNN policy to sequentially sample a string that in turn encodes the neural architecture. Initial optimisation uses REINFORCE policy gradient algorithm, then uses Proximal Policy Optimisation (PPO). Baker et al [4] use Q-learning to train a policy which sequentially chooses a layer's type and corresponding hyperparameters. An alternative view is: as sequential decision processes in which the policy samples actions to generate the architecture sequentially, the environment's state contains a summary of the actions sampled so far, and the (undiscounted) reward is obtained after final action. Since no interaction with an environment occurs, we treat it as sequential generation of single action, i.e. multi-armed bandit problem.

A related approach was by Cai et al [10], who frame NAS as a sequential decision process: the state is the current architecture, the reward is an estimate of the architecture's performance, and the action corresponds to an application of function-preserving mutations, dubbed netowrk morphisms [12, 63], followed by a phase of training the network. In order to deal with variable-length network architectures, they use a bi-directional LSTM to encode architectures into a fixed-length representation. Based on this encoded representation, actor networks decide no the sampled action. The combination of these two components constitute the policy, which is trained end-to-end with the REINFORCE policy gradient algorithm. We note that this approach will not visit the same state (architecture) twice so that strong genearalisaton over the architecutre space is required from the policy.

#### Neuro-evolutionary approaches
Miller et al [44] use genetice algorithms to propose architectures and use back-propagation to optimise weights. Many neuro-evolutionary approaches since then [2,55,56] use genetic algorithms to optimise both the neural architecutre and its weights; however, SGD-based weight optimisation outperforms evolutionary ones. (for RL tasks competitive results can be obtained [15,51,57]). More recent works use gradient-based methods to find weights and EA for optimising architecture [22, 38, 43, 49, 50, 59, 66]. The mutations are local opeartions, such as adding or removing a layer, altering the hyperparameters of a layer, adding skip connections, as well as altering training hyperparameters. After training, the fitness (eg performance on a validation set) is evaluated and they are added to the population.

Neuro-evolutionary methods differ in how they sample parents, update populations, and generate offsprings. Real et al [50 ,49] and Liu et al [38] use tournament selection [27] to sample parents. Elsken et al [22] sample parents from a multi-objective Pareto front using an inverse density. Real et al [50] remove worst individual from a population, [49] remove the oldest individual, and [38] do not remove individuals. To generate offspring, most approaches initialise child networks randomly, while [22] employ Lamarckian inheritance, ie knowledge passed on with network morphisms. [50] let offsprint inherit all non-mutant parameters to speed up learning. 

#### Comparison
Real et al [49] conduct a case studying to compare RL, evolution, and random search (RS), concluding RL and evolution perform equally well in terms of final test accurracy, iwth evolution having better anytime performance and finding smaller models. Both are better than RS with a small margin. 

#### Bayesian Optimisation
Popular for hyperparameter optimisation, BO is limited for NAS since typical BO toolboxes are based on Gaussian processes and focus on low-dimensional continuous optimisation problems. Several works use tree-based models for high-dimensional conditional spaces and achiev state-of-the-art performance [7, 19, 41,69]. There is preliminary evidence that these may outperform evolutionary algorithms [33].

#### Hierarchical Search
Negrinho and Gordon [45] and Wistuba [65] exploit tree-structure of their search space and use Monte Carlo Tree Search. Elsken et al [21] propose a simple yet well performing hill climbing algorithm that discovers high-quality architectures by greedily moving in the direction of better performing architectures without requiring more sophisticated exploration mechanisms.

#### Gradient-based Optimisation
Liu et al [39] propse a continouse relaxation of the search space to enable gradient-based optimisation: instead of fixing a single operation oi, the author selects from {o1,...,om}, with a coefficient of probability λ attached to each selection. He then optimise both the network weights and the architecture by alternating gradient descent steps for λ. Operation i is optimised with i=argmax<sub>i</sub>λ<sub>i</sub>. Shin et al [54] and Ahmed and Torresani [1] also employ gradient-based optimisation of neural architectures, however only considering optimising layer hyperparaemters or connectivity patterns.

### Performance Estimation Strategy
Performance estimation is necessary to evaluate a certain architecture _A_. Simplist is to train _A_ on training data and evaluate on validation data but this often requires thousands of GPU days. To reduce computational burden, performance is estimated based on lower fideliites of the actual performance after full training (aka proxy metrics). Lower fidelities include shorter training times, training on a subset of data, on lower-resolution images, or with less filters per layer. This introduces bias, but if the relative ranking of architectures remain stable, this is not a problem. Some research also argue the difference can be dramatic, proposing the use of higher fidelities.

Another way to evaluate performance builds upon learning curve extrapolation. Initial learning curves are extrapolated and architectures predicted to perform poorly are terminated. Some train a surrogate model to extrapolate performance based on architectural/cell porperties. The main challenge is, in order to speed up, good predictions in a large search space need to be made based on few evaluations.

Another approach to speed up is to initialize weights based on weights of other trained architectures. One way is network morphisms, modifying architecture while not changing the function represented by the network. This increases capacity, retains performance without training from scratch. Advantage is unlimited upper bound on architecture's size. Disadvantage is that it may lead to overly complex architectures. Approximate network morphism allows shrinking architectures.

One-shot Architecutre Search speeds up performance estimateion, treating architectures as different subgraphs of a supergraph and shares weights between architectures that have edges of this supergraph in common. Only the weights of a single one-shot model need to be trained, and architectures can then be evaluated by inheriting trained weights. Disadvantage is that it incurs large bias and underestimates the actual performance of architectures severely. But ranking architectures is effective. Related is meta-learning of hypernetworks that generate weights for novel architectures and thus requires only training the hypernetwork but not the architectures themselves. The main difference is that weights are note strictly shared by generated by the shared hypernetwork.

The limitation of one-shot NAS is that the supergraph defined a-priori restricts the search space to subgraphs. Moreover, approaches which require that the entire supergraph resides in GPU memory during architecutre search will be restricted to relatively small supergraphs and search spaces hence are often used in cell-based search spaces. It is not clear what biases are introduced with weight-sharing scheme.

### Future Directions
Many focus on image classification since 1) NAS is challenging in this area and manual engineering predominates, and 2) there is well-defined search space with knowledge from manual engineering. Other areas to apply NAS include language modelling, music modelling, image restoration, network compression; applications to reinforcement learning, generative adversarial networks, semantic segmentation, or sensor fusion.

Another direction is multi-task problems and multi-objective problems, in which measures of resource efficiency are used as objectives along with the predictive performance on unseen data. Likewise, extension of RL/bandit approaches is applied to learn policeis that are conditioned on a state that encodes task properties/resource requirements. One-shot NAS has been used to generate different architectures depending on the task or instance on-th-fly. 

Defining more general and flexible search spaces is a related direction. Transcending standard definition of cells and architecture can increase the power of NAS. A search space allowing more general structure would make NAS more broadly applicalbe. Common search space does not allow novel building blocks hence is limited.

Comparison of performance is difficult since architecture is not the single factor. For example, results on CIFAR-10 dataset differ in search space, computational budget, data augmentation, training procedures, regularization and other factors. The definition of common benchmarks is thus important. It would also be interesting to evaluation NAS as part of a full open-source AutoML system where hyperparameters and data augmentaiton pipeline are optimised along with NAS.

While NAS has achieved impressive performance, it fails to provide insights to why specific architectures work well and why architectures derived in independent runs are similar. Identifying common motifs, understanding performance, and invesigating generalisation property are desirable.

### Bibliography
#### Reinforcement Learning
75. Zoph, B., Vasudevan, V., Shlens, J., Le, Q.V.: Learning transferable architectures for scalable
image recognition. In: Conference on Computer Vision and Pattern Recognition (2018)

4. Baker, B., Gupta, O., Naik, N., Raskar, R.: Designing neural network architectures using
reinforcement learning. In: International Conference on Learning Representations (2017a)

10. Cai, H., Chen, T., Zhang, W., Yu, Y., Wang, J.: Efficient architecture search by network
transformation. In: Association for the Advancement of Artificial Intelligence (2018a)

12. Chen, T., Goodfellow, I.J., Shlens, J.: Net2net: Accelerating learning via knowledge transfer.
In: International Conference on Learning Representations (2016)

63. Wei, T., Wang, C., Chen, C.W.: Modularized morphing of neural networks. arXiv:1701.03281
(2017)

## Neural Architecture Search with Reinforcement Learning

Authors: Barret Zoph, Quoc V Le

Link: https://arxiv.org/pdf/1611.01578.pdf

Abstract: RNN to generate model descriptions and train RNN with RL to maximise the expected accuracy of the generated architectures on a validation set. Test set outperforms state-of-art on CIFAR-10 and Penn Treebank dataset.

### Introduction
Structure and connectivity of a NN can be specified by a variable-length string. It is possible to use a RNN - the controller - to generate such a string. Training the network specified by the string - "child network" - on the real data will result in an accuracy on a validation set. Using this accuracy as the reward signal, we can compute the policy gradietn to update the controller. As a result, in the next iteration, the controller will give higher probabilities to architectures that receive high accuracies. 

### Related Work
Hyperparameter optimisation is widely researched (Bergstra et al., 2011; Bergstra & Bengio, 2012; Snoek et al., 2012; 2015; Saxena & Verbeek, 2016). These are limited because they search from fixed-length space. The practice work better only with good inital model (Bergstra & Bengio, 2012; Snoek et al., 2012; 2015). There are Bayesian optimisation methods (Bergstra et al., 2013; Mendoza et al., 2016), that allow non-fixed length architectures but are less general and less flexible than methods in this paper.

Neuro-evolution algorithms are more flexible for composing novel models but are less practical at large scale. They are search-based thus are slow and require many heuristics to work well.



### Bibliography
#### Hyperparameter optimisation
James Bergstra, Remi Bardenet, Yoshua Bengio, and Bal ´ azs K ´ egl. Algorithms for hyper-parameter ´
optimization. In NIPS, 2011.

James Bergstra and Yoshua Bengio. Random search for hyper-parameter optimization. JMLR, 2012.

Jasper Snoek, Hugo Larochelle, and Ryan P. Adams. Practical Bayesian optimization of machine
learning algorithms. In NIPS, 2012.

Shreyas Saxena and Jakob Verbeek. Convolutional neural fabrics. In NIPS, 2016.

Jasper Snoek, Oren Rippel, Kevin Swersky, Ryan Kiros, Nadathur Satish, Narayanan Sundaram,
Mostofa Patwary, Mostofa Ali, Ryan P. Adams, et al. Scalable bayesian optimization using deep
neural networks. In ICML, 2015.

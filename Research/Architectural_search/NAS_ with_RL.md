## Neural Architecture Search with Reinforcement Learning

Authors: Barret Zoph, Quoc V Le

Link: https://arxiv.org/pdf/1611.01578.pdf

Abstract: RNN to generate model descriptions and train RNN with RL to maximise the expected accuracy of the generated architectures on a validation set. Test set outperforms state-of-art on CIFAR-10 and Penn Treebank dataset.

### Introduction
Structure and connectivity of a NN can be specified by a variable-length string. It is possible to use a RNN - the controller - to generate such a string. Training the network specified by the string - "child network" - on the real data will result in an accuracy on a validation set. Using this accuracy as the reward signal, we can compute the policy gradietn to update the controller. As a result, in the next iteration, the controller will give higher probabilities to architectures that receive high accuracies. 

### Related Work
Hyperparameter optimisation is widely researched (Bergstra et al., 2011; Bergstra & Bengio, 2012; Snoek et al., 2012; 2015; Saxena & Verbeek, 2016). These are limited because they search from fixed-length space. The practice work better only with good inital model (Bergstra & Bengio, 2012; Snoek et al., 2012; 2015). There are Bayesian optimisation methods (Bergstra et al., 2013; Mendoza et al., 2016), that allow non-fixed length architectures but are less general and less flexible than methods in this paper.

Neuro-evolution algorithms are more flexible for composing novel models (Wierstra et al. (2005); Floreano et al. (2008); Stanley et al. (2009)) but are less practical at large scale. They are search-based thus are slow and require many heuristics to work well.

NAS has parallels to program synthesis and inductive programming, the idea of searching a program from examples (Summers, 1977; Biermann, 1978). Probabilistic program induction has been successfully used in machine learning such as solving simple Q&A (Liang et al., 2010; Neelakantan et al., 2015; Andreas et al., 2016), sorting numbers (Reed & de Freitas, 2015) and learning with very few examples (Lake et al., 2015).

The controller in NAS is auto-regressive, predicting hyperparameters one a time, conditioned on previous predictions. The idea is borrowed from the decoder in end-to-end sequence to sequence learning. Unlike sequence to sequence learning, our method optimises a non-differentiable metric, which is the accuracy of the child network. It is similar to BLEU optimisation in Neural Machine Translation (Ran- zato et al., 2015; Shen et al., 2016). Unlike these approaches, our method learns directly from reward signal without supervised bootstrapping.

Another related idea of learning to learn is meta-learning (Thrun & Pratt, 2012), a general framework of using information learned in one task to improve a future task. More closely related is the idea of using a neural network to learn the gradient descent updates for another network (Andrychowicz et al., 2016) and the idea of using RL to find updat policies for another network (Li & Malik, 2016).

### Methonds
![Image]('https://github.com/zhangsushen1992/zhangsushen.github.io/blob/master/Research/Architectural_search/RNN.png')
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

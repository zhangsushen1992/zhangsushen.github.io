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

#### General Model Descriptions with a Controller RNN
![Image]('https://github.com/zhangsushen1992/zhangsushen.github.io/blob/master/Research/Architectural_search/RNN.png')
An RNN is run to generate tokens such as number of filters, filter height, filter width, stride height, etc, for each layer in an RNN. The process of generating an architecture stops if the number of layers exceeds a certain value. This value follows a schedule where it is increased as training progresses. The structure built by RNN is tested on held-out validation set and the accuracy is used to optimise RNN parameters θ<sub>c</sub>. A policy gradient method is used to update θ<sub>c</sub> such that the controller RNN generates better architectures over time.

#### Training with REINFORCE
The list of tokens that the controller predicts can be viewed as a list of actions a<sub>1:T</sub> to design an architecture for child network. The test accuracy R is the reward signal. The expected reward is represented as:

J(θ<sub>c</sub>) = E<sub>P(a<sub>1:T</sub>;θ<sub>c</sub>)</sub>[R]

Since the reward signal R is non-differentiable, we need to use a policy gradient method to iteratiely update θ<sub>c</sub>. In this work, we use the REINFORCE rule from Williams (1992):

∇<sub>θ<sub>c</sub></sub>J(θ<sub>c</sub>) = 

#### Increase Architecture Complexity with Skip Connections and Other Layer Types
Modern architectures contain skip connections or branching layers such as GoogleNet (Szegedy et al., 2015), Residual Net (He et al., 2016a). We introduce method to allow skip connetions or branching layers, to widen the search space. We use a set-selection type attention (Neelakan- tan et al., 2015) which was built upon the attention mechanism (Bahdanau et al., 2015; Vinyals et al., 2015). At layer N, we add an anchor point which has N-1 content-based sigmoids to indicate the previous layers that need to be connected. Each sigmoid is a function of the current hiddenstate of the controller and the previous hiddenstates of the previous N-1 anchor points:

P(Layer j is an input to layer i) = sigmoid(v<sup>T</sup>tanh(W<sub>prev</sub> * h <sub>j</sub> + W<sub>curr</sub> * h<sub>i</sub>))

where h<sub>j</sub> represents the hiddenstate of the controller at anchor point for the j-th layer, where j ranges from 0 to N-1. We then sample from these sigmoids to decide what previous layers to be used as inputs to the current layer. The matrices W and v are trainable parameters. As these connections are also defined by probability distributions, the REINFORCE method still applies without any significant modifications.

In the framework, if one layer has many input layers, then all input layers are concatenated in the depth dimension. Skip connections can cause "compilation failures" where one layer is not compatible with another layer, or one layer may not have any input or output. To circumvent these issues, we employ three techniques:
- If a layer is not connected to any input layer, then the image is used as the input layer
- At the final layer, we take all layer outputs that have not been connected and concatenate them before sending this final hiddenstate to classifier
- If input layers to be concatenated have different sizes, we pad the small layers with zeros so that the concatenated layers have the same sizes.

It is also possible to add the learning rate, pooling, local contrast normalisation, batchnorm. 

#### Generate Recurrent Cell Architecutres
We will modify the above method to generate recurrent cells. At every time step t, the controller needs to find a functional form for h<sub>t</sub> that takes x<sub>t</sub> and h<sub>t-1</sub> as inputs. The simplest way is to have h<sub>t</sub> = tanh(W<sub>1</sub> * x<sub>t</sub> + W<sub>2</sub> * h<sub>t-1</sub>), which is the basic recurrent cell. A more complicated formulation is LSTM (Hochreiter & Schmidhuber, 1997).

The 

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

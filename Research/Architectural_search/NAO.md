## Neural Architecture Optimization

Authors: Renqian Luo, Fei Tian, Tao Qin, Enhong Chen, Tie Yan Liu

Link: http://papers.nips.cc/paper/8007-neural-architecture-optimization.pdf

### Abstract
Automatic neural architecture design has shown its potential in discovering powerful neural network architectures. Existing methods, no matter based on RL or EA, conduct search in a discrete space, which is highly inefficient. In this paper, we propose a simple and efficient method to automatic neural architecture design based on continuous optimisation. We call this new approach neural architecture optimisation (NAO). There are three key components in our proposed approach: (1) An encoder embeds/maps nueral network architectures into a continuous space. (2) A predictor takes the continuous representation of a network as input and predicts its accuracy. (3) A decoder maps a continuous representation of a netowrk back to its architecture. The performance predictor and the encoder enable us to perform graidnet based optimisation in the continuous space to find the embedding of a new architecture with potentially better accuracy. Such a better embedding is then decoded to a network by the decoder. 

Experiments show that the architecture discoverd by our method is very competitive for image classification task on CIFAR-10 and language modeling task on PTB, outperforming or on par with the best results of previous architecture search methods with a significantly reduction of comptational resources. Specifically we obtain 2.11% test set error rate for CIFAR-10 image classification task and 56.0 test perplexity of PTB langugage modelling task. The best discovered architectures on both tasks are sucessfully transferred to other tasks such as CIFAR-100 and WikiText-2. Furthermore, combined with the recent proposed weight sharing mechanism, we discover powerful architecutre on CIFAR-10 (with error rate 3.53%) and on PTB (with test set perplexity 56.6), with very limited computational resources (less than 10 GPU hours) for both tasks.

### Introduction
We propose to optimise network architecture by mapping architectures into a continuous vector space (i.e. network embeddings) and conducting optimisation in this continuous space via gradient based method. On one hand, similar to the distributed representationa of natural language, the continuous representation of an architecture is more compact and efficient in representing its topological information; On the other hand, optimizing in a continuous space is much easier than directly searching within discrete space due to better smoothness.

We call this optimisation based approach NAO. The core of NAO is an encoder model responsible to map a neural network architecture into a continuous representation. On top of the continuous representation we build a regression model to apporximate the final performnace (eg classification accuracy on the dev set) of an architecture. It is noteworthy that the regression model is similar to the perofrmance predictor in previous works. What distinguishes our method is how to leverage the performance predictor: different with previous work that uses the performance predictor as the heuristic to select the already generated architectures to speed up the searching process, we directly optimise the module to obtain the continuous representation of a better network by gradient descent. The optimised representation is then leveraged to produce a new neural network architecture that is predicted to perform better. To achieve that, another key module for NAO is designed to act as the decoder recovering the discrete architecture from the continuous representation. The decoder is an LSTM model equipped with attention mechanism that makes the exact recovery easy. The three components are jointly trained in a multi-task setting which is beneficial to continuous representation: the decoder 
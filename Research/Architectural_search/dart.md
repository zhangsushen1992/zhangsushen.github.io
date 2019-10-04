## DARTS: Differentiable architecture search

Authors: Hanxiao Liu, Karen Simonyan, Yiming Yang

Link: https://arxiv.org/pdf/1806.09055.pdf

### Abstract
This paper addresses the scalability challenge of achitecture search by formulating the task in a differentiable manner. Unlike conventional approaches of applying evolution or RL over a discrete and non-differentiable search space, our method is based on the continuous relaxation of the architecture representation, allowing efficient search of the architecture using gradient descent. Extensive experiments on CIFAR-10, ImageNet, Penn Treebank and WikiText-2 show that our algorithm excels in discovering high-performance convolutional architectures for image classification and recurrent architectures for language modelling, while being orders of magnitude faster than state-of-the-art non-differentiable techniques. Our implementation has been made publicly available to facilitate further research on efficient architecture search algorithms.

### Introduction
Discovering state-of-the-art neural network architectures requires substantial effort of human

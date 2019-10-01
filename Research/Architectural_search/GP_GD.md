## Genetic programming and Gradient Descent: A Memetic Approach to Binary Image Classification

Authors: Benjamin Patrick Evans, et al

Link: https://arxiv.org/pdf/1909.13030.pdf

Rating: ***

### Abstract
This paper presents a hybrid (memetic) approach combining Genetic programming (GP) and Gradient-based optimisation for image classification to overcome the limitations of 1) reliance on human design and 2) low interpretability. The performance using local search mechanism is compared to baseline on four binary classification image datasets.

### Introduction
CNN models suffer from the reliance on manual craft and low interpretability. Various methods have been proposed to overcome these limitations [7,8] but with huge computational cost. Attempts suh as DeconvNets [9] and saliency maps [10] increased interpretability of convolutional layers but not fully connected layers.

Genetic programming (GP) is an evolutionary computation (EC) technique also used in image classification. The architecture is determined automatically, without human intervention. 

In this work, we incorporate a local search mechanism into the evolutionary process, similar to the updating scheme used in CNNs as a method of fine-tuning.

### Background
Memetic algorithm is the area combining EC with local search [12]. While EC methods such as GP are gradient-gree optimisation methods, gradient information can be incorporated. In [13,14], gradient descent is used as local search mechanism in GP for classification. GP+ local search is shown to outperform GP.

[15] assigns a weighted coefficient θ to each node in a tree. The output, rather than being n, is now θn with θ periodically optimised using Trust Region algorithm. Local search is shown to outperform GP without losing interpretability, and generating smaller trees.

[16] termed "lifetime learning" as GP+local search. It searches for replacement nodes from the function set for each node in the tree. This is similar to "exhaustive mutation" for a node with all allowable variations trialled. It outperforms standard GP.

### New Method
#### Program structure
The proposed structure has three tiers. Tier 1 contains raw image as input and optional convolution and pooling operations. Tier 2 is the aggregation tier. This works over a region of the image and applies statistical measures such as minimum, maximum, mean and std. Tier 3 is classification, with +,-,x and protected (0 if denominator 0). The only compulsory tier is the aggregation tier, converting image to numeric output. 

#### Fine tuning with gradient-based optimisation
Tree can be written as nested functions. For example:
```
F(x) = AggMin(conv(x,filter),Rectangle, 0.1,0.1,0.5,0.5)
```
where x is the image. The loss function is:
```
f(x,θ) = L(targets, σ(F(x,θ))
```
σ is the activation function. Since functions in tree and loss function are differentiable, this is a differentiable optimisation problem, looking for θ. The loss function is Cross-Entropy loss since classification accuracy is notoriously poor as loss function. Loss is averaged over batch number. 

#### Local search integration
Efficiency is important in GA because several loops are run. However, even with mini-batch, the overall process is slow. Three processes are proposed: 1) standard ConvGP, 2) local search on 25 fittest individuals every 10th generation starting form the first generation 3) 100 epochs of SGD run on final generation as fine-tuning. GP searhc for models, local search to improve on top-perofrmers. 

### Experiment Design
Datasets:
- Hand postures [18]: closed fists vs open hands
- JAFFE [19]: Japanese female facial expression dataset: Happy vs Sad
- Office [20]: Mobile phone vs calculator
- CMU faces [21]: Happy vs sad

### Results and Discussions
No statistically significant improvement was observed for all dataset. However, the average tree size on two datasets were smaller with local search. Also the learnt filter have increased interpretability.

### Future work
- Making the current fixed learning rate adaptive
- More exhaustive search over the number of epochs, hopefully generating improved classification accuracy
- A validation set introduced to track loss
- Parallel funning to reduce training time



## A Framework for Designing the Architectures of Deep Convolutional Neural Networks

Authors: Saleh Albelwi, Ausif Mahmood

Link: https://www.mdpi.com/1099-4300/19/6/242

### Abstract
We propose a framework that automatically designs a CNN. A new optimisation objective function that combines error rate and the information learnt by a set of feature maps using deconvolutional networks (deconvnet) is proposed. The new objective function allows the hyperparameters of the CNN architecture to be optimised in a way that enhances the performance by guiding the CNN through better visualisation of learnt features via deconvnet. The acutal optimisation is implemented via _Nelder-Mead Method (NMM)_. Further, our new objective function results in much _faster convergence_ towards a better architecture. The proposed framework has the ability to explore a CNN architecture's numerous design efficiently and allows effective, distributed execution and synchronisation via web services. The proposed design outperforms in terms of error rate. The results are comparative with state-of-the-art on MNIST dataset and performs reasonably against state-of-the-art on CIFAR-10 and CIFAR-100. Our approach has a significant role in increasing the depth, reducing the size of strides, and constraining convolutional layers not followed by pooling layers in order to find a CNN architecture that produces a high recognition performance.

### Introduction
In this article, we present an efficient optimisation framework that aims to design a high-performing CNN architecture for a given dataset automatically. in this framework, we use deconvolutional networks (deconvnet) to visualise the information learnt by the feature maps. The deconvenet produces a reconstructed image that includes the activated parts of the input image. A good visualisation shows that the CNN model has learnt properly, a poor visualisation shows ineffective learning. We use a correlation coefificent based on Fast Fourier Transport (FFT) to measure the similarity between the original images and their reconstructions. The quality of the reconstruction, using the correlation coefficient and the error rate, is combined into a new objective function. We use the Nelder-Mead Method (NMM) to automate the search for a high-performing CNN architecture through a large search space by minimizing the proposed objective function. We exploit web services to run three vertices of NMM simultaneously on distributed computers to accelerate the computation time. 

### Related Work
#### Grid and Random Search
The drawback of grid search is its expensive computation, which increases exponentially with the number of hyperparameters and the depth of exploration desired [28]. Recently, random search [29] has reported better results, requiring less computational time. However, neither improves architecture based on previous evaluations.

#### Bayesian optimisation
Recently, Bayesian Optimisation methods have been used for hyperparameter optimisation [24, 30, 31]. BO constructs a probabilisitc model based on previous evolutions of the objective function. Popular techniques include Spearmint [24] and Sequential Model-based Algorithm Configuration (SMAC) [30]. According to [32], BO are limited because they work poorly whne high-dimensional hyperparameters are invovled and are very computationally expensive.[24] uses BO with Gaussian process. [21,33,34] optimised continuous hyperparameters of DNN. [34-39] proposed adaptive techniques for automatically updating continuous hyperparameters to improve the convergence speed of backpropagation. Early stopping [40,41] can be used when error rate not improved. In [42], an effective technique is proposed to initialise weights.[47] used PSO for multiple ensemble pruning. The drawback of GA is computational cost. In [48] l1-regularisation is used to automate the selection of hte number of units.[50] proposed CoDeepNEAT-based Neuron Evolution of Augmenting Topologies (NEAT) to determine the type of each layer and its hyperparameters. [51] used GA to find filter size through zeroth order interpolation.

#### Reinforcement learning
[49] applied RL to RNNs.  RL based on Q-learning [52] was used to search for design one layer at a time, where the depth is decided by user. However, there is long execution time with significant computational resources.

#### Visualisation approach
[14,53] used feature maps. [14] visualised second layer of AlexNet. However, problem with CNN are diagnosed with expert knowledge. Selection of architecture is mannual.

#### Evolutionary algorithms
[22] uses GA to optimise filter sizes and number of filters only, with a high error rate. [43] uses particle swarm optimisation (PSO).

### Framework Model
#### Reducing training set
Instance selection, i.e. to use a subset or sample of training dataset to achieve acceptable accuracy, is proposed and reviewed in [59]. [60] evaluates different instance selection algorithms on CNNs. In this framework, we use Random Mutual Hill Climbing (RMHC) as preprocessing step of instance selection. The reason to use RMHC is that the user can predefine the size of the training sample, which is not possible with other algorithms. 

Statistics is used to determine the sample size. We use the margin error and confidence interval. Margin error is the maximum range between error of whole training set and the sample error. The confidence interval measures reliability of results of training sample. We use 95% confidence interval in determining the optimal size of a training sample.

#### CNN Feature Visualisation
Layer activation shows the activations of the feature maps [62] as a bitmap. Activation maximization [63] retrieves the images that maximally activate the neuron. However, the ReLU activation does not always have a semantic meaning. Deconvolutional network [14] shows parts of input learned by a feature map, making it easier to analyse potential problems with higher interpretability.

#### Correlation coefficient
The correlation coefficient [64] measures the similarity between two iamges or independent variables, maximal when images are high similar. The correlation between input image and the reconstructed image is recorded as Corr. Corr for different images are accumulated to be CorrRes. This is defined by:

Corr(A,B) = 1/n Σ<sub>i=1</sub><sup>n</sup>((a<sub>i</sub>-b)/σ<sub>a</sub>)((a<sub>i</sub>-b)/σ<sub>b</sub>)

With Fast Fourier Transform (FFT), it is defined as:

Corr(A,B) = F<sup>−1</sup>[F(A) ◦ F∗(B)] 


#### Objective Function
Since error rate on different validation set varies, the model design cannot be generalised. We present a new objective function that exploits information from the error rate (Err) combined with correlation results, written as:

f(λ) = η(1 − Corr<sub>Res</sub>) + (1 − η) Err

where η is a coefficient measuring the importance of Err and CorrRes. NMM is used to optimise the objective.

#### Nelder Mead Method
The Nelder Mead algorithm (NMM) or simplex method [67], is a direct search technique used when the derivative information is unknown. NMM uses a concept called a simplex, which is a geometric shape consisting of n+1 vertices for optimising n hyperparameters. It finds the optimal vertex through reflection, expansion, contraction and shrinkage. This is specially designed for derivative-free algorithm, similar to GA, PSO, BO, where derivatives are not required. NMM is used here due to speed and ease of parallelisation.

#### Accelerating processing time with parallelism
Since serial NMM executes the vertices seqentially one vertex at a time, the optimisation is expensive. Since there are no dependences between the vertices, it can be paralleled. We implement a synchronous master-slave NMM model, which distributes the running of several vertices on workers while the master machine controls the whole optimisation procedure. The master cannot move to the next step until all of the workers finish their task.
Recently, web services [68] provide a powerful technology for interacting between distributed applications. Two popular methods are Simple Object Access Protocol (SOAP) and Represetational State Transfer (RESTful). We use RESTful [71] to create web services because it is simple to implement as well as lightweight, fast and readable by humans.

### Results and Discussion
#### Dataset
We use CIFAR-10, CIFAR-100 and MNIST.

#### Experimental setup
- Training settings: Weights are initialised based on Xavier initialisation technique [42], biases set to zero. We apply ReLU and employ early stopping to prevent overfitting. Dropout [11] is implemented with rate of 0.5. 
- Nelder Mead settings: the total number of hyperparameters n is required to construct the initial simplex with n+1 vertices.  

#### Discussion
- The proposed objective function generates CNNs with lower error rate than conventional error-rate only objective function
- The proposed framework generates lower error rates on CIFAR-10 and CIFAR-100 compared to expert design, random search, GA, and SMAC
- It does not get stuck on a local minimum early. More flexibility to sample a new architecture
- The proposed framework generates less pooling layers giving a better visualisation
- Running in parallel is 3x faster than in series
- Compare to the state-of-the-art methods (including Maxout, DropConnect, DSN, ResNet(110), CoDeepNEAT, MetaQNN, RL, GA) our method on MNIST, CIFAR-10 and CIFAR-100 produced comparable results, with less processing time. 
- RMHC outperforms random set by 2%. 

### Future directions
- Application to image datasets
- Explore cancellation criteria to discover and avoid evaluating poor architectures, improving execution time

## MONAS: Multi-Objective Neural Architecture Search

Authors: Chi-Hung Hsu et al

Link: https://arxiv.org/pdf/1806.10332.pdf

### Abstract
MONAS, a framework for multi-objective neural architectural search, employs reward functions considering both prediction accuracy and other important objectives (eg power consumption) when searching for architectures. It achieved comparable or better classificaiton accuracy on computer vision applications, while satisfying the additional objectives such as peak power.

### Introduction
MONAS considers both prediction accuracy and additional objectives (eg energy consumption). MONAS is adaptable to customised constraints and objectives and is generalisable to different neural networks. 

### Background
Pareto-NASH and Nemo used Evolutionary algorithms to search under multi-objectives. Steven and McCourt used BO to optimise the trade-off between model accuracy and model inference time. The major difference with previous work is that we consider model computation cost, such as power consumption or Multiply-Accumulate(MAC) operations. 

### Framework
#### Overview
Two-stage frameowkr similar to NAS. In generation stage we use a RNN as a robot network (RN), hich geneartes a hyperparameter sequence for a CNN. In the evaluation stage, we train an existing CNN model as a target network(TN) with the hyperparameters output by the RNN. The accuracy and energy consumption of N are rewards to RN. The RN updates itself based on this reward with RL.

#### Implementation Details
RN. The input sequence of the LSTM is a vector whose length is the number of hyperparameter candidates and is initialised to zeros. The ouput of LSTM is fed into a softmax layer, and a prediction is then completed by sampling through the probabilities representing for each hyperparameter selection. We use one-hot encoding.

TN & Search Space. We experiment on AlexNet and CondenseNet. We predict stage and growth.

### Reinforcement Learning Process
#### Policy Gradient
Peter and Schaal 2006 treat hyperparameter decisions as a series of actions in the policy-based RL. We take RNN as an actor with paramether θ and θ is updated by policy gradient to maximise the expected reward R<sub>θ</sub>:

θ<sub>i+1</sub> = θ<sub>i</sub> + η∇R<sub>θi</sub>

where η is the learning rate. We approximate ∇R<sub>θ</sub> with method of (Zoph and Le, 2017a):

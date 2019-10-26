## Designing Neural Network Architectures using Reinforcement Learning

Authors: Bowen Baker, Otkrist Gupta, Nikhil Naik, Ramesh Raskar

Link: https://arxiv.org/abs/1611.02167

### Abstract
We introduce MetaQNN, a meta-modelling algorithm based on RL to automatically generate high-performing CNN architectures for a given learning task. The learning agent is trained to sequentially choose CNN layers using Q-learning with an epsilon-greedy exploration strategy and experience replay. The agent explores a large but finite space of possible architectures and iteratively discovers designs with improved performance on the learning task. On image classification benchmarks, the agent-designed networks (consisting of only standard convolution, pooling and fully-connected layers) beat existing networks designed with the same layer types and are competitive against the state-of-the-art methods that use more complex layer types. We also outperform existing meta-modelling approaches for network design on image classification tasks.

### Introduction
We construct a novel Q-learning agent whose goal is to discover CNN architectures that perform well on a given machine learning task with no human intervention. The learning agent is given the task of sequentially picking layers of a CNN model. By discretizing and limiting the layer parameters to choose from, the agent is left with a finite but large space of model architecutres to search from. The agent learns through random exploartion and slowly begins to exploit its findings to select higher performing models using the epsilon-greedy strategy.

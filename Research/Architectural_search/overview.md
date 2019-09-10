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

1) Chain-structured neural networks:

A sequence of _n_ layers, where the _i_'th layer _L<sub>i</sub>_ receives input from _L<sub>i-1</sub>_ and output to _L<sub>i+1</sub>_. The architecture _A_ = _L_<sub>n</sub>◦...◦ _L_<sub>1</sub>◦ _L_<sub>0</sub>. The search space is parameterised by:

(1) number of layers _n_, 

(2) type of operation (pooling, convolution, etc) 

(3) hyperparamters (number of filters, kernel size, strides, number of fully-connected units). 

(3) depends on (2) hence the search space is conditional.

2) Multi-branch networks:

Input to layer _i_ is a function _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>), combining previous layer outputs. Special cases include: 

(1) Chain-structured: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_L_<sub>i-1</sub><sup>out</sup>, 

(2) Residual Networks: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_L_<sub>i-1</sub><sup>out</sup> + _L_<sub>j</sub><sup>out</sup>, _j_<_i_ 

(3) DenseNets: concatenate previous layer outputs: _g<sub>i</sub>_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)=_concat_(_L_<sub>i-1</sub><sup>out</sup>,...,_L_<sub>0</sub><sup>out</sup>)

3) Repeated Mortifs

Architecture can be viewed as repeated motifs, sometimes dubbed cells or blocks. For example, normal cell preserves dimensionality while reduction cell reduces spatial dimension. Final architecture is constructed by stacking cells in a predefined manner. The advantage of this includes:

(1) Reduce search space.
(2) Cells more adaptable to different datasets.

Problem is how to choose meta-architecture, namely how to connect cells. Ideally the meta-architecture should be optimised automatically as part of NAS.

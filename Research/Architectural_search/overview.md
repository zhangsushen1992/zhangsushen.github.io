## Neural Architecture Search

Author: Thomas Elsken, Jan Hendrik Metzen, and Frank Hutter

Excerpt from: Chapter 3, Automated Machine Learning pp 63-77

Link: https://link.springer.com/chapter/10.1007/978-3-030-05318-5_3

Current design of architectures are mainly constructed mannually by human experts, making it error prone and time-consuming. Automation of architecture search is, therefore, an important research area. It is a subfield of AutoML and overlaps with hyperparameter optimisation and meta-learning.

Three categories of architecture search include:
- **search space**: Prior knowledge of the task simplifies search and reduces search space, but is susceptible to human bias and is limited by current human knowledge.
- **search strategy**: The search encompasses classical exploration-exploitation trade-off: the need to find well-performing architectures speedily conflicts with the possibility of a premature convergence to suboptimal architectures.
- **performance estimation strategy**: Traditional training and validation is computationally expensive to be performed on a large number of architectures. Current research focuses on reducing cost of performance estimations.

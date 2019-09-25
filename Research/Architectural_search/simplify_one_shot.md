## Understanding and Simplifying One-Shot Architecture Search

Author: Gabriel Bender, Pieter-Jan Kindermans, Barret Zoph, Vijay Vasudevan, Quoc Le

Link: http://proceedings.mlr.press/v80/bender18a/bender18a.pdf

Abstract: Weight sharing across models amortises the cost of training in architectural search. Research in the past remains complex and requires hypernetworks or RL controllers. This research identifies promising architectures from a complex search space without hypernetworks or RL.

### Introduction:
Training from scratch is computationally expensive. With weight sharing, one can train a single large network capable of emulating any architecture in the search space.


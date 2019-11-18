## Integrating Learning and Planning
### Introduction
- Instead of learning value function from experience, learn model from experience
- Then use planning to construct a value function or policy
- Integrate learning and planning into a single architecture

### Model-based Reinforcement Learning
- experience -> model learning -> model -> planning -> value/policy -> acting -> experience -> ...

Advantages of model-based RL:
- can efficiently learn model by supervised learning methods
- can reason about model uncertainty

Disadvantages of model-based RL:
- first learn a model, then construct a value function
- - two sources of approximation error

What is a model?
- A model M is a representation of an MDP <S, A, P, R> parameterised by η
- Assume state space S and action space A are known
- M = <P<sub>η</sub>,R<sub>η</sub>> transition probabilities and rewards
- - S<sub>t+1</sub> ~ P<sub>η</sub>(S<sub>t+1</sub>|S<sub>t</sub>,A<sub>t</sub>)
- - R<sub>t+1</sub> = R<sub>η</sub>(R<sub>t+1</sub>|S<sub>t</sub>,A<sub>t</sub>)
- Typically assume conditional independence between state transitions and rewards
- - P[S<sub>t+1</sub>,R<sub>t+1</sub>|S<sub>t</sub>,A<sub>t</sub>] = P[S<sub>t+1</sub>|S<sub>t</sub>,A<sub>t</sub>]P[R<sub>t+1</sub>|S<sub>t</sub>,A<sub>t</sub>]

Model learning
- Goal: estimate model M<sub>η</sub> from experiences {S1,A1,R2,...,ST}
- This is a supervised learning problem: S1,A1 -> R2,S2 ; S2,A2 -> R3,S3 ... ST-1,AT-1 -> RT,ST
- Learning s,a->r is a regression problem
- Learning s,a->s' is a density estimation problem
- Pick loss function, eg mean-squared error, KL divergence
- Find parameters η that minimise empirical loss

Examples of Models:
- Table lookup model
- Linear expectation model
- Linear Gaussian model
- Gaussian process model
- Deep belief network model

Table lookup model:
- Model is an explicit MDP, P,R
- Count visits N(s,a) to each state action pair
- - P = (1/N) ∑**1**(St,At,St+1=s,a,s')
- - R = (1/N) ∑**1**(St,At=s,a)Rt
- Alternative, record experience and randomly pick matching (s,a,...)

Plan with a model:
- Given a model M<sub>η</sub> =<P<sub>η</sub>, R<sub>η</sub>>
- Solve the MDP <S, A, P<sub>η</sub>, R<sub>η</sub>>
- Use planning algorithm: value iteration, policy iteration, tree search

Sample-based planning:
- Use the model only to generate samples
- Sample experience from model
- - S<sub>t+1</sub> ~ P<sub>η</sub>(S<sub>t+1</sub> | S<sub>t</sub>,A<sub>t</sub>)
- - R<sub>t+1</sub> = R<sub>η</sub>(R<sub>t+1</sub> | S<sub>t</sub>,A<sub>t</sub>)
- Apply model-free RL to sample: Monte-Carlo control, Sarsa, Q-learning
- Sample-based planning methods usually more efficient
### Integrated Architectures
4408
### Simulation-based Search

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


### Integrated Architectures

### Simulation-based Search

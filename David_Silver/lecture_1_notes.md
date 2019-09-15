## Lecture 1
### Characteristics of Reinforcement Learning (RL)
RL is a different learning paradigm: 
- There is no supervisor, only a reward signal. 
- Feedback is delayed, not instantaneous.
- Time really matters (sequential, non i.i.d data)
- Agent's action affect the subsequent data is receives

### The RL Problem
Definition (Reward Hypothesis):

All goals can be described by the maximisation of expected cumulative reward. 

Sequential Decision Making:

- Goal: select actions to maximise total future reward
- Actions may have long term consequences
- Reward may be delayed
- May be better to sacrifice immediate reward to gain more long term reward

At each step t, the agent:

Executes action A<sub>t</sub>
Receives observation O<sub>t</sub>
Receives scalar reward R<sub>t</sub>

The environment:

Receives action A<sub>t</sub>
Emits observation O<sub>t</sub>
Emits scalar reward R<sub>t</sub>

The history is :
- H<sub>t</sub> = A<sub>1</sub>, O<sub>1</sub>, R<sub>1</sub>, ..., A<sub>t</sub>, O<sub>t</sub>, R<sub>t</sub>
- All observable variables up to time t
- What happens next depends on history.

State is the information used to determine what happens next
State is a function of the history: S<sub>t</sub>=f(H<sub>t</sub>)

The environment state S<sub>t</sub><sup>e</sup> is the environment's private representation.
This is what the environment uses to pick the next observation/reward
Usually not visible to the agent, even if visible, may contain irrelevant information

The agent state S<sub>t</sub><sup>a</sup> is the agent's internal representation.
used to pick the next action
Can be any function of history: S<sub>t</sub><sup>a</sup> = f(H<sub>t</sub>)

An information state (aka Markov state) containes all useful information from the history:
Definition
A state St is Markov if and only if:
P[S<sub>t+1</sub> | S<sub>t</sub>] = P [S<sub>t+1</sub> | S<sub>1</sub>,...,S<sub>t</sub>]
The future is independent of the past given present

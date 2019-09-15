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
Goal: select actions to maximise total future reward
Actions may have long term consequences
Reward may be delayed
May be better to sacrifice immediate reward to gain more long term reward

At each step t, the agent:
Executes action A<sub>t</sub>
Receives observation O<sub>t</sub>
Receives scalar reward R<sub>t</sub>

The environment:
Receives action A<sub>t</sub>
Emits observation O<sub>t</sub>
Emits scalar reward R<sub>t</sub>

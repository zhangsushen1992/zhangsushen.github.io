## Lecture 2: Markov Decision Process
### Introdcution to MDPs
**Markov** decision processes formally describe an envrionemnt for RL
- where the environment is fully observable
- the current state completely characterises the process
- almost all RL problems can be formalised as MDPs (e.g. optimal control is continous MDPs, bandits are MDPs with one state, partially observable problems can be converted to MDPs)

Markov Property
- Definition:
A state St is Markov if and only if
 P[S<sub>t+1</sub> | S<sub>t</sub>] = P[S<sub>t+1</sub> | S<sub>1</sub>,..., S<sub>t</sub>]
 
 

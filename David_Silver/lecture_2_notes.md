## Lecture 2: Markov Decision Process
### Introdcution to MDPs
**Markov** decision processes formally describe an envrionemnt for RL
- where the environment is fully observable
- the current state completely characterises the process
- almost all RL problems can be formalised as MDPs (e.g. optimal control is continous MDPs, bandits are MDPs with one state, partially observable problems can be converted to MDPs)

Markov Property
- Definition:
A state S<sub>t</sub> is Markov if and only if
P[S<sub>t+1</sub> | S<sub>t</sub>] = P[S<sub>t+1</sub> | S<sub>1</sub>,..., S<sub>t</sub>]

State Transition Matrix
For a Markov state s and successor state s', the state transition probability is defined by:
P<sub>ss'</sub> = P[S<sub>t+1</sub>=s' | S<sub>t</sub>=s]

The transition matrix P defines transition probabilities from all state s to all successor states s': \
P = [[P<sub>11</sub> ... P<sub>1n</sub>], 
  >> [...........], \
  >> [P<sub>n1</sub> ... P<sub>nn</sub>]] 

**Markov Process**
- A Markov process is a memoryless random process, i.e. a sequence of random states S<sub>1</sub>, S<sub>2</sub>, ... with the Markov property.
- Definition:
Markov Process (or Markov Chain) is a tuple <S,P>
- - S is a (finite) set of states
- - P is a state transition probability matrix
- - P<sub>ss'</sub> = P[S<sub>t+1</sub>=s' | S<sub>t</sub>=s]


 

 

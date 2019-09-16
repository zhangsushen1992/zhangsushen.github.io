## Lecture 2: Markov Decision Process
### Introdcution to MDPs
**Markov** decision processes formally describe an envrionemnt for RL
- where the environment is fully observable
- the current state completely characterises the process
- almost all RL problems can be formalised as MDPs (e.g. optimal control is continous MDPs, bandits are MDPs with one state, partially observable problems can be converted to MDPs)

**Markov Property**
- Definition:
A state S<sub>t</sub> is Markov if and only if
P[S<sub>t+1</sub> | S<sub>t</sub>] = P[S<sub>t+1</sub> | S<sub>1</sub>,..., S<sub>t</sub>]

**State Transition Matrix**
- For a Markov state s and successor state s', the state transition probability is defined by:
P<sub>ss'</sub> = P[S<sub>t+1</sub>=s' | S<sub>t</sub>=s]

- The transition matrix P defines transition probabilities from all state s to all successor states s': \
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

### Markov Reward Process
A Markov reward process is a Markov chain with values.
- Definition: A Markov Reward Process is a tuple <S,P,R,γ>
- - S is a finite set of state
- - P is a state transition probability matrix: P<sub>ss'</sub> = P[S<sub>t+1</sub>=s' | S<sub>t</sub>=s]
- - R is a reward function, R<sub>s</sub> = E[R<sub>t+1</sub> | S<sub>t</sub> = s]
- - γ is a discount factor, γ∈[0,1]

The return Gt is the total discounted reward from time step t.
- G<sub>t</sub> = R<sub>t+1</sub> + γR<sub>t+2</sub> + ... = ∑<sub>k=0</sub><sup>∞</sup>γ<sup>k</sup>R<sub>t+k+1</sub>
- The discount factor is the present value of future rewards
- γ=1: extremely far-sighted, γ=0: short-sighted, only current reward

Why discount?
- Mathematically conveneinet to discount rewards
- Avoids infinite returns in cyclic Markov processes
- Uncertainty about the future may not be fully represented
- Financially, time value of money
- Animal/human behaviour shows preference for immediate reward
- Possible to use undiscounted Markov reward processes (γ=1) if all sequneces terminate

**Value function**
The value function v(s) gives the long term value of state s
- Definition: The state value function v(s) of an MRP is the expected return starting from state s: v(s) = E[G<sub>t</sub> | S<sub>t</sub>=s]

**Bellman Equation for MRPs**
- The value function can be decomposed into two parts: immediate reward R<sub>t+1</sub> and discounted value of successor state γv(S<sub>t+1</sub>) 
- v(s) = E[G<sub>t</sub> | S<sub>t</sub>=s]
> = E[R<sub>t+1</sub> + γR<sub>t+2</sub> + γ<sup>2</sup>R<sub>t+3</sub> + ... | S<sub>t</sub>=s] \
> = E[R<sub>t+1</sub> + γ(R<sub>t+2</sub> + γR<sub>t+3</sub> + ...) | S<sub>t</sub>=s] \
> = E[R<sub>t+1</sub> + γG<sub>t+1</sub> | S<sub>t</sub>=s] \
> = E[R<sub>t+1</sub> + γv(S<sub>t+1</sub>) | S<sub>t</sub>=s]

v(s) = R<sub>s</sub>+ γ∑<sub>s'∈S</sub>P<sub>ss'</sub>v(s')

The Bellman equation can be expressed concisely using matrices: v=R+γRv

> [v(1)      [R<sub>1</sub>       [P<sub>11</sub> ... P<sub>1n</sub>  [v(1)  \
> ...... =...... + γ     ......       ......     ......   \
> v(n)]       R<sub>n</sub>]       P<sub>n1</sub> ... P<sub>nn</sub>]  v(n)] 

The Bellman equation is a linear equation, can be solved directly:
- v = R + γPv 
- (1-γP)v = R 
- v = (1-γP)<sup>-1</sup>R

- Computational complexity is O(n<sup>3</sup>) for n states
- Direct solution only possible for small MRPs
- Iterative methods for large MRPs: eg
- - Dynamic programming
- - Monte-Carlo evaluation
- - Temporal Difference Learning

### Markov Decision Processes
A Markov decision process (MDP) is a Markov reward process with decisions. It is an environment in which all states are Markov.
- Definition: A Markov Decision Process is a tuple <S,A,P,R,γ>
- - S is a finite set of states
- - A is a finite set of actions
- - P is a state transition probability matrix: P<sub>ss'</sub> = P[S<sub>t+1</sub>=s' | S<sub>t</sub>=s, A<sub>t</sub>=a]
- - R is a reward function: R<sub>s</sub> = E[R<sub>t+1</sub> | S<sub>t</sub> = s, A<sub>t</sub>=a]
- - γ is a discount factor 

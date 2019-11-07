## Planning by Dynamic Programming

### Introduction
- Dynamic: sequential or temporal component
- Programming: Optimising a program i.e. policy
- A method for solving complex problems by breaking down into subproblems
- Properties of dynamic programming:
- - Optimal substructure (principle of optimality applies) (optimal solution can be decomposed into subproblems)
- - Overlapping subproblems (subproblems recur many times) (solutions can be cached and reused)
- - Markov decision processes satisfy both properties (Bellman equation gives recursive decomposition) (value function stores and resuses solutions)
- Dynamic programming assumes full knowledge of MDP
- It is used for planning in an MDP
- for prediction: input to MDP <S,A,P,R,γ> and policy π or input to MRP <S, P<sup>π</sup>, R<sup>π</sup>, γ>
- - output: value function v<sup>π</sup>
- for control: input to MDP <S,A,P,R,γ>
- - output: optimal value function v<sub>*</sub> and optimal policy π<sub>*</sub>
- prediction predicts the value function given a policy. control chooses a policy that gives the optimal value function

- Dynamic programming is used to solve:
- - scheduling algorithms
- - string algorithms(sequence alignment)
- - graph algorithms(shortest path algorithms)
- - graphical models(viterbi algorithm)
- - bioinformatics (lattice models)

### Policy Evaluation
- Problem: evaluate a give policy π
- Solution: iterative application of Bellman expectation backup
- v<sub>1</sub> -> v<sub>2</sub> -> ... ->v<sub>π</sub>
- Using synchronous backups
- - At each iteration k+1
- - For all states s∈S
- - Update v<sub>k+1</sub>(s) from v<sub>k</sub>(s')
- - where s' is a successor state of s

v<sub>k+1</sub>(s) = Σ<sub>a∈A</sub>π(a|s)(R<sup>a</sup><sub>s</sub> + γΣ<sub>s'∈S</sub>P<sub>ss'</sub><sup>a</sup> v<sub>k</sub>(s'))

v<sup>k+1</sup> =R<sup>π</sup> + γP<sup>π</sup>v<sup>k</sup>

### Policy Iteration
- Given a policy π, evaluate the policy with: v<sub>π</sub>(s) = E[R<sub>t+1</sub>+γR<sub>t+2</sub>+...|S<sub>t</sub>=s]
- Improve the policy by acting greedily with respect to v<sub>π</sub>: π' = greedy(v<sub>π</sub>)
- Need many iterations of improvement/evaluation to converge to π*

### Value Iteration
v*(s) <- max<sub>a∈A</sub> R<sub>s</sub><sup>a</sup>+γΣ<sub>s'∈S</sub>P<sub>ss'</sub><sup>a</sup> v<sub>*</sub>(s')
- Apply these updates iteratively. Intuition: start with final rewards and work backwards
- Unlike policy iteration, there is no explicit policy. Intermediate value functions may not correspond to any policy.

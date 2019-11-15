## Model-Free Prediction
### Introduction
- Known MDP using dynamic programming vs unknown MDP using model-free methods

### Monte-Carlo Learning
- learn directly from episodes of experience
- model-free: no knowledge of MDP transitions/ rewards
- no bootstraping
- idea: value = mean return instead of expected return
- only applicable to episodic MDPs

First-visit Monte-Carlo Policy Evaluation
- To evaluate state s
- The **first** time-step t that state s is visited in an episode (averaged over different episodes)
- Incremental counter N(s) <- N(s) + 1
- Increment total return S(s) <- S(s) + Gt
- Value is estimated by mean return V(s) = S(s) / N(s)
- Law of large numbers V(s) -> v<sub>π</sub>(s) as N(s) -> ∞

Every-visit Monte-Carlo Policy Evaluation
- To evaluate state s
- **Every** time-step t that state s is visited in an episode
- Increment counter N(s) <- N(s) + 1
- Increment total return S(s) <- S(s) + Gt
- Value is estimated by mean return V(s) = S(s) / N(s)
- Again, V(s) -> v<sub>π</sub>(s) as N(s) -> ∞

Incremental mean: μ<sub>k</sub> = μ<sub>k-1</sub> + (1/k)(x<sub>k</sub>-μ<sub>k-1</sub>)

Incremental Monte-Carlo Updates:
- Update V(s) incrementally after episode S1, A1, R2,...,ST
- For each state St with return Gt: N(St) <- N(St) + 1
V(St) <- V(St) + (1/N(St))(Gt-V(St))
- In non-stationary problems, it can be useful to track a running mean, ie forget old episode
V(St)<- V(St) + α(Gt - V(St))

### Temporal-Difference Learning
- TD methods learn directly from episodes of experience
- TD is model-free: no knowledge of MDP transitions / rewards
- TD learns from incomplete episodes, by bootstrapping
- TD updates a guess towards a guess

MC and TD:
- Goal: learn v<sub>π</sub> online from experience under policy π
- Incremental every-visit Monte-Carlo
- - Update value V(S<sub>t</sub>) toward actual return Gt: V(S<sub>t</sub>) <- V(S<sub>t</sub>) +  α(G<sub>t</sub> - V(S<sub>t</sub>))
- Simplest TD learning algorithm TD(0)
- - Update value V(S<sub>t</sub>) toward estimated return: V(S<sub>t</sub>) <- V(S<sub>t</sub>) +  α(R<sub>t+1</sub> + γV(S<sub>t+1</sub>) - V(S<sub>t</sub>))
- - R<sub>t+1</sub> + γV(S<sub>t+1</sub>)  is called the TD target
- - ẟ<sub>t</sub> = R<sub>t+1</sub> + γV(S<sub>t+1</sub>) - V(S<sub>t</sub>) is called the TD error

Advantages and Disadvantages of MC vs TD
- TD can learn before knowing the final outcome
- - TD can learn online after every step
- - MC must wait until end of episode before return is known
- TD can learn without the final outcome
- - MC can only learn from complete sequences
- - TD works in continuing (non-terminating) environments
- - MC only works for episodic (terminating) environments

Bias/Variance Trade-off
- Return G<sub>t</sub> = R<sub>t+1</sub> + γR<sub>t+2</sub> + ... + γ<sub>T-1</sub>R<sub>T</sub> is unbiased estimate of v<sub>π</sub>(S<sub>t</sub>)
- True TD target R<sub>t+1</sub> + γv<sub>π</sub>(S<sub>t+1</sub>) is unbiased estimate of v<sub>π</sub>(S<sub>t</sub>)
- TD target R<sub>t+1</sub> + γV(S<sub>t+1</sub>) is biased estimate of v<sub>π</sub>(S<sub>t</sub>)
- TD target is much lower vairance than the return:
- - Return depends on many random actions, transitions, rewards (noise in each variable)
- - TD target depends on one random action, transition, reward (noise from one step)

- MC has high veriance, zero bias
- - Good convergence properties
- - (even with function approximation)
- - Not very sensitive to initial value
- - Very simple to understand and use
- TD has low variance, some bias
- - Usually more efficient than MC
- - TD(0) converges to v<sub>π</sub>(s)
- - (but not always with function approximation)
- - More sensitive to initial value (self-feeding)

- TD exploits Markov property, more efficient in Markov environments
- MC does note exploit Markov property, more effective in non-Markov environments

Batch MC and TD:
- MC and TD converge: V(s)->v<sub>π</sub>(s) as experience -> ∞
- But what about batch solution for finite experience? e.g repeatedly sample episode k∈[1,K]
- Apply MC or TD(0) to episode k

Bootstrapping and Sampling:
- Bootstrapping: update involves an estimate
- - MC does not bootstrap, DP bootstraps, TD bootstraps
- Sampling: update samples an expectation
- - MC samples, DP does not sample, TD samples

full backups      dynamic programming                 exhaustive search
sample backups    temporal-difference learning        monte-carlo
                   <---------------bootstrapping, λ--------------------->
                shallow backups                                     deep backups
       
### TD(λ)
n-step prediction
- let TD target look n steps into the future
- infinite n is monte-carlo

- Define the n-step return:
- - G<sub>t</sub><sup>(n)</sup> = R<sub>t+1</sub> + γR<sub>t+2</sub> +...+γ<sup>n-1</sup>R<sub>t+n</sub> + γ<sup>n</sup>V(S<sub>t+n</sub>)
- n-step TD learning:
- - V(S<sub>t</sub>) <- V(S<sub>t</sub>) + α(G<sub>t</sub><sup>(n)</sup> - V(S<sub>t</sub>))

- The λ-return G<sub>t</sub><sup>λ</sup> combines all n-step returns G<sub>t</sub><sup>(n)</sup>
- Using weight (1-λ)λ<sup>n-1</sup>: G<sub>t</sub><sup>λ</sup>=(1-λ)Σ<sub>n=1</sub><sup>∞</sup>λ<sup>n-1</sup>G<sub>t</sub><sup>(n)</sup>
- Forward-view, TD(λ): V(S<sub>t</sub>) <- V(S<sub>t</sub>) + α(G<sub>t</sub><sup>(n)</sup> - V(S<sub>t</sub>))

Eligibility Traces:
- Frequency heuristic: assign credit to most frequent states
- Recency heuristic: assign credit to most recent states
- Eligibility traces combine both heuristics: E0(s)=0, Et(s) = γλE<sub>t-1</sub>(s) + **1**(S<sub>t</sub>=s)

Backward view TD(λ)
- Keep eligibility trace for every state s
- Update value V(s) for every state s
- In proportion to TD-error ẟ<sub>t</sub> and eligibility trace E<sub>t</sub>(s):
- ẟ<sub>t</sub> = R<sub>t+1</sub> + γV(S<sub>t+1</sub>)-V(S<sub>t</sub>)
- V(s) <- V(s) + αẟ<sub>t</sub>E<sub>t</sub>(s) 

When λ=0, only current state is updated: E<sub>t</sub>(s) = **1**(S<sub>t</sub>=s), V(S<sub>t</sub>) <- V(S<sub>t</sub>) + αẟ<sub>t</sub>E<sub>t</sub>(s) 

For TD(0) update: V(S<sub>t</sub>) <- V(S<sub>t</sub>) + αẟ<sub>t</sub>

TD(1) is same as MC. Credit deferred until end of episode.
```
Theorem: The sum of offline updates is identical for forward-view and backward-view of TD(λ).
```
Σ<sub>t=1</sub><sup>T</sup>αẟ<sub>t</sub>E<sub>t</sub>(s) = Σ<sub>t=1</sub><sup>T</sup>α(G<sub>t</sub><sup>λ</sup> - V(S<sub>t</sub>))**1**(S<sub>t</sub>=s)

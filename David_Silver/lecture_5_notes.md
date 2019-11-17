## Model-free Control
### Introduction
Model-free because model is unkonwn but experience is sampled, or model is known but is too big to use except by samples

On-policy: learn about policy π from experience sampled from π
Off-policy: learn about policy π from experience sampled from μ

The reason to use Q instead of V: Greedy policy improvement over V(s) requires model of MDP (requires transition probabilities to evaluate optimal π):

π'(s)=argmax<sub>a∈A</sub>R<sub>s</sub><sup>a</sup>+P<sub>ss'</sub><sup>a</sup>V(s')

But using Q(s,a) it can be model-free:

π'(s)=argmax<sub>a∈A</sub>Q(s,a)


### On-policy Monte-Carlo Control

GLIE Definition:

Greedy in the Limit with Infinite Exploration (GLIE)
- All state-action pairs are explored infinitely many times:
- lim<sub>k→∞</sub>N<sub>k</sub>(s,a)=∞
- The policy converges on a greedy policy
- lim<sub>k→∞</sub>π<sub>k</sub>(a|s) = **1**(a=argmax<sub>a'∈A</sub>Q<sub>k</sub>(s,a'))

GLIE Monte-Carlo Control:
- sample kth episode using π: {S1,A1,R2,...,ST} ~ π
- for each state St and action At in the episode:
- N(St, At) <- N(St,At) + 1
- Q(St,At) <- Q(St,At) + 1/(N(St,At)) * (Gt-Q(St,At))
- Improvement based on new action-value function:
- ϵ <- 1/k
- π <- ϵ-greedy(Q)
### On-policy Temporal-difference Learning

### Off-policy Learning

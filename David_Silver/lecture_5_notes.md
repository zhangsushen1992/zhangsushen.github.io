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
- Improve policy based on new action-value function:
- ϵ <- 1/k
- π <- ϵ-greedy(Q)

Theorem: GLIE Monte-Carlo control converges to the optimal action-value function Q(s,a)->q*(s,a)

TD has several advantages over MC:
- Lower variance
- Online
- Incomplete sequences

Insight: apply TD to Q(S,A), use ϵ-greedy policy improvement, update every time-step

### On-policy Temporal-difference Learning
- Sarsa: S,A -> R -> S' -> A'
- Q(S,A) <- Q(S,A) + α(R + γQ(S',A') - Q(S,A))

Theorem:
- Sarsa converges to the optimal action-value function, Q(s,a) -> q*(s,a), under the following conditions:
- GLIE sequence of policies π<sub>t</sub>(a|s)
- Robbins-Monro sequence of step-sizes α<sub>t</sub>
- - Σ<sub>t=1</sub><sup>∞</sup>α<sub>t</sub>=∞
- - Σ<sub>t=1</sub><sup>∞</sup>α<sub>t</sub><sup>2</sup><∞

Define the n-step Q-return:
- q<sub>t</sub><sup>(n)</sup> = R<sub>t+1</sub> + γR<sub>t+2</sub> + ... + γ<sup>n-1</sup>R<sub>t+n</sub> + γ<sup>n</sup>Q(S<sub>t+n</sub>)
- n-step Sarsa updates Q(s,a) towards the n-step Q-return
- Q(St,At) <- Q(St,At) + α(q<sub>t</sub><sup>(n)</sup> - Q(St,At))

Forward view Sarsa(λ):
- q<sub>t</sub><sup>λ</sup> = (1-λ) Σ<sub>n=1</sub><sup>∞</sup>λ<sup>n-1</sup>q<sub>t</sub><sup>(n)</sup>
- Q(St,At) <- Q(St,At) + α(q<sub>t</sub><sup>λ</sup> - Q(St,At))

Backward view Sarsa(λ):
- Just like TD(λ), we use eligibility traces in an online algorithm
- But Sarsa(λ) has one eligibility trace for each state-action pair
- - E<sub>0</sub>(s,a) = 0
- - E<sub>t</sub>(s,a) = γλE<sub>t-1</sub>(s,a) + **1**(St=s,At=a)
- Q(s,a) is updated for every state s and action a
- In proportion to TD-error ẟt and eligibility trace E<sub>t</sub>(s,a)
- - ẟ<sub>t</sub> = R<sub>t+1</sub> + γQ(S<sub>t+1</sub>,A<sub>t+1</sub>) - Q(S<sub>t</sub>,A<sub>t</sub>)
- - Q(s,a) <- Q(s,a) + αẟ<sub>t</sub>E<sub>t</sub>(s,a)

### Off-policy Learning
- Evaluate target policy π(a|s) to compute v<sub>π</sub>(s) or q<sub>π</sub>(s,a) while following behaviour policy μ(a|s)
- Reuse experience from old policies
- Learn about optimal policy while following exploratory policy; learn about multiple policies while following one policy

Estimate the expectation of a different distribution:
- E<sub>X~P</sub>[f(X)] = ΣP(X)f(X) = ΣQ(X) [P(X)/Q(X)] f(X) = E<sub>X~Q</siub> [P(X)/Q(X)] f(X)

Off-policy not working well on MC; on TD:
- V(St) <- V(St) + α(π(At|St)/μ(At|St) * (R<sub>t+1</sub>+γV(St+1)) - V(St))

Q-learning
- Next action is chosen using behaviour policy At+1 ~ μ(.|St)
- We consider alternative successor action A'~π(.|St)
- Update Q(St,At) towards value of alternative action
- Q(St,At) <- Q(St,At) + α((R<sub>t+1</sub>+γQ(St+1,A') - Q(St,At))

- We now allow both behaviour and target policies to improve
- The target policy π is greedy wrt Q(s,a)
- - π(St+1) = argmax<sub>a'</sub> Q(St+1,a')
- The behaviour policy μ is ϵ-greedy wrt Q(s,a)
- The Q-learning target then simplifies:
- - R<sub>t+1</sub>+γQ(S<sub>t+1</sub>,A') 
- - = R<sub>t+1</sub>+γQ(S<sub>t+1</sub>,argmax<sub>a'</sub> Q(S<sub>t+1</sub>,a')) 
- - = R<sub>t+1</sub>+ max<sub>a'</sub>γQ(S<sub>t+1</sub>,A')
- Theorem: Q-learning control converges to the optimal action-value function, q*(s,a)

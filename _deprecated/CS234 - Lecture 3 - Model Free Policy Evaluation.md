---
category: cs234
---

Def: **Policy Evaluation** is computing the value of all states given a particular policy $\pi$. Policy evaluation can be wither **model-based** (requires the transition and reward probabilities) or **model-free** (does not require information about transitions and rewards) 

Recall from Lecture 2: **Model-Based Dynamic Programming Iterative Method for Policy Evaluation**
For $k$ = 1... until convergence; For all states s:
  - $V_k^\pi(s) = R^\pi(s,\pi(s)) + \gamma \sum_{s' \in S} P^\pi(s'|s, \pi(s))V^\pi_{k-1}(s')$

In Dynamic Programming, we use $V^\pi_{k-1}(s')$ as an estimate for the actual value of the reward at state $s'$. This estimation is called **bootstrapping** (Appendix 1).

Cons:
  - Requires assumption that state is Markov.
  - This is model-based: requires an explicit model of the reward function.

# Model Free Policy Evaluation
Def: **Episode**: A complete sample of a policy on the MDP until termination.
Def: Below, let $G_{i,t} = \sum_{t' = t}^\infty \gamma^{t'-t}r_{t'} = r_{i,t} + \gamma r_{i,t+1} + \cdots $ be the expected reward for episode $i$ starting at time $t$.

## Monte Carlo Policy Evaluation
Simulate the policy many times and average rewards recieved. Several variants exist (Appendix 2).
This is performed once per episode. The value of every state is updated once the episode is completely sampled through.
The most common variant for the update used is **incremental every-visit MC**, which performs the incremental update $V^\pi(s) = V^\pi(s) + \alpha (G_{i,t} - V^\pi(s))$ for each sampled episode. $t$ in $G_{i,t}$ refers to the time when a particular state was seen according to some update rule in the episode, but the updates occur after the episode has already been run through.
$\alpha$ is chosen $\ge 1/N(s)$ where $N(s)$ is the number of unique states visited so far. Other variants are **first-visit MC** and **every-visit MC** (Appendix 2).

(Note: We can apply the same Monte Carlo update, but to learn the state-action value function (aka Q function) instaed of the V function. In this case the update is
$$Q^\pi(s_t,a_t) = Q^\pi(s_t,a_t) + \alpha (G_{i,t} - Q^\pi(s_t,a_t)$$ This is the policy evaluation step used in Monte Carlo Control.)

Pros: Does not assume state is Markov.

Cons: 
  - Requires episodic settings (processes must terminate). In particular cannot handle settings with self-loops. 
  - Generally a very high variance estimator, and reducing variance can require a lot of data.
  - Slow to update: Only one update every episode.

## Temporal Difference (TD) learning
This is the most popular method of the three. It is a combination of Monte Carlo and Dynamic Programming Methods. We sample episodes, but instead of updating at the termination of each episode, for every time step $(s_t,a_t,r_t,s_{t+1})$ we observe in every episode we perform the bootstrapped update
$$V^\pi(s_t) = V^\pi(s_t) + \alpha ([r_t + \gamma V^\pi(s_{t+1})] - V^\pi(s_t))$$

Pros: Updates are applied much faster, after every action instaed of every episode, so generally much lower variance than Monte-Carlo Methods. Since we also bootstrap, TD can be used in episodic or infinite-horizon cases.

However, TD updates can take a bit longer to propagate than Monte Carlo, because you only update your reward for the state that you’re currently in. You don’t backpropagate the reward information to states that you previously visited, which happens in Monte Carlo.

(Note: We can apply the same TD update, but to learn the state-action value function (aka Q function) instaed of the V function. In this case we need to observe  $(s_t,a_t,r_t,s_{t_1}, a_{t+1})$, and the update is
$$Q^\pi(s_t,a_t) = Q^\pi(s_t,a_t) + \alpha ([r_t + \gamma Q^\pi(s_{t+1},a_{t+1})] - Q^\pi(s_t,a_t))$$ This is the policy evaluation step used in SARSA.)

![Screenshot 2020-03-29 at 5.19.49 PM.png](/assets/blog_resources/80F9C4ACDE25FE29A3002EB7052B2F77.png)

![D0219E351F2C53A159E24444FEDDCED6.png](/assets/blog_resources/ECC99F9E89FB51787A6F20D109EFD8D9.png)![Screenshot 2020-03-29 at 5.28.14 PM.png](/assets/blog_resources/CAD6F4593B3733569A98E5D818A1A751.png)

TD[0] is bootstrapping after the first reward, TD[1] is boostrapping after the second reward, etc.

![Screenshot 2020-03-29 at 5.33.59 PM.png](/assets/blog_resources/F960B6B2CDD767388AC20D2C268A6DC8.png)

**Summary of the Three Methods:**

(See Appendix 3 for a case where MC/TD give different results) 

![Screenshot 2020-03-29 at 5.38.13 PM.png](/assets/blog_resources/2C4647864B515C9D6FCC24EC4F98EAD6.png)

Rationale: DP and TD use bootstrapping. This handles onn-episodic domains, but is a biased estimate of value and requires the Markov Assumption. 

DP is model-based learning (needs transition model), whereas MC/TD is model-free and requires no models. All three have convergence guarentees.

**Tradeoffs:**

If the world is not Markovian, you don’t want to make the Markov assumption and get wrong results, so Monte Carlo is better. 

If the world is Markov, TD is a lot faster. 

![Screenshot 2020-03-29 at 5.39.50 PM.png](/assets/blog_resources/2B479980A4E3B8658397BC5CFFA39CF5.png)

![Screenshot 2020-03-29 at 5.43.31 PM.png](/assets/blog_resources/93A40A2A3E390F2E4BF994C249966D2F.png)

(TD[0] converges to DP policy V^\\pi for the MDP with the maximum likelihood estimates for teh dynamics and the reward model)

![Screenshot 2020-03-29 at 6.11.05 PM.png](/assets/blog_resources/2A407A2AA543263442C8782C0350D0C7.png)

**Appendix 1: Policy Evaluation - Dynamic Programming**

Note that in the Dynamic Programming method we are bootstrapping the future expected rewards by using $V\_{k-1}^\\pi$ as a stand-in for the sum of future rewards. In policy evaluation, this is precise (not an estimate) because we are in the model-based case, but in the model-free case generally using this substitution would be a biased estimate.![Screenshot 2020-03-29 at 4.38.07 PM.png](/assets/blog_resources/E7598067BAFF7E84BEC5102B948827CD.png)

**
**

**Appendix 2: Variants of Monte Carlo Policy Evaluation**

![Screenshot 2020-03-29 at 4.51.11 PM.png](/assets/blog_resources/22E6BAEF8FDBA9AB3C9FAF1288696870.png)

No bias becuase the state information you use is iid.

![Screenshot 2020-03-29 at 4.52.12 PM.png](/assets/blog_resources/12F03A01B6DBB3B019DB050DAE09142D.png)

![Screenshot 2020-03-29 at 4.53.45 PM.png](/assets/blog_resources/377F8360ED47B712B2FF11D37D3154B0.png)

(Equivalent Formulation to Every-visit Monte Carlo)

If you go to a state multiple times in every episode, your returns are correlated so it’s no longer iid. But its still consistent and has a lot lower variance.

![Screenshot 2020-03-29 at 4.54.56 PM.png](/assets/blog_resources/D937C8F9B57D21A2736AA670B8A3DCA4.png)

![Screenshot 2020-03-29 at 5.09.17 PM.png](/assets/blog_resources/230953C2490FB6C9098039108C7B0244.png)

**Appendix 3: Different between the three methods for model-free policy evaluation:**

![Screenshot 2020-03-29 at 5.43.24 PM.png](/assets/blog_resources/58955319D8391FAE1B1A0F0202D3B69C.png)

The above supposes the online case- being able to sample the above observations many many times, and perfomring updates each time.

V(A) = 0 in ML estimation, but V(B) = 3/4 in TD estimation.

**Appendix 4: Stats Recap**

![Screenshot 2020-03-29 at 4.48.42 PM.png](/assets/blog_resources/CEDDC70FE93EB63C43ADB260A7CD1038.png)

Mean Squred Error is Bias^2 + Variance
# Explore-Exploit
  - Applications
    - Web advertising (Estimating CTR)
    - Cold start for Recommendation Engines
    - Clinical Trials
    - Adaptive Routing (Package Routing)
    - Asset Pricing
    - Product Pricing
    - Promo Optimization

# Multi-Armed Bandits
## Basic k-armed bandit
  - Each arm $a$
    - Wins (reward=1) with fixed (unknown) prob $\mu_a $
    - Loses (reward=0) with fixed (unknown) prob $1-\mu_a$
  - All draws are independent given $\mu_1,...,\mu_k$
  - Problem: How to pull arms to maximize total rewards?

## Stochastic k-armed bandit
### Setting
- Set of k choices (arms)
- Each choice $a$ is associated with unknown probability distribution $P_a$ with support in $[0,1]$. Given $P_a$, rewards are independent between draws.
- In each round $t$:
  - We pick some arm $a$ and obtain as reward random sample $X_t$ from $P_a$.
- Our goal is to maximize $\sum_{t=1}^{T} X_t$

### Solution
This is a RL algorithm. We find a **policy**: a strategy or rule that in each iteration tells me which arm to pull.
We quantify performance of the algo using **regret**.
Def: Regret:
  - Let $\mu_a$ be the mean of $P_a$
  - The payoff/reward of the best arm is $\mu* = max_a \mu_a$
  - Let $i_1, i_2, ..., i_r$ be the sequence of arms pulled
  - Instantaneous regret at time $t$: $r_t = \mu^{*} - \mu_{a_t}$
  - Total regret is $R_T = \sum_{t=1}^T r_t$
  - A typical goal is: We wnat a policy (arm allocation strategy) that guarentees $\frac{R_T}{T} \rightarrow 0$ as $T \rightarrow \infty$


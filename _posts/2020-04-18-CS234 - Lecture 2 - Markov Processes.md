# 2.1 Types of Markov Processes
Def: A state has the **Markov property** if and only if: $p(s_{t+1}|s_t,a_t) = p(s_{t+1}|h_t,a_t)$
## Markov Processes
Def: A **Markov Process** is a double $(S,P)$, with a finite set $S$ of states, and $P$, a transition model specifying $p(s_{t+1} = s' | s_t = s)$
If the state space is finite, P can be expressed as a matrix of transition probabilities.

## Markov Reward Process:
Def: A **Markov Reward Process** is a quadruple $(S,P,R, \gamma)$, with a finite set $S$ of states, and $P$, a transition model specifying $p(s_{t+1} = s' | s_t = s)$, and $R$, a reward function $R(s_t = s) = \mathbb{E}[r_t|s_t = s]$. Rewards are discounted by a discount factor $\gamma$.
Def: The **return** is the discounted sum or rewards.
Def: The **value function** of a state is the expected of the return of a state. You can estimate the value of a state by simulation.
Def: The **Bellman equation** states that $V(s) = R(s) + \gamma \sum_{s' \in S}P(s'|s)V(s')$. This is used for policy evaluation.

## Markov Decision Process (MDP):
Def: A **Markov Decision Process** a tuple $(S,A,R,P,\gamma)$ with finite set $S$ of states, a finite set $A$ of actions, and $P$, a transition model from each action and each state specifying $p(s_{t+1} = s' | s_t = s, a_t = a)$, and $R$, a reward function $R(s_t = s, a_t = a) = \mathbb{E}[r_t|s_t = s]$. Rewards are discounted by a discount factor $\gamma$.

Def: A MDP **policy** is a conditional distribution over actions $\pi(a|s) = P(a_t=a|s_t=s)$ specifying what action to take in each state. Note that an MDP + a policy $\pi(a|s)$ is simply a Markov Reward Process.
  - Def: **stationary** policies only depend on the current state, that is, we can parameterize it $\pi(a|s)$. 
  - Def: **nonstationary** policies depend on the timestep as well as the current state. We just parameterize it $\pi(a_t|s_t, t)$
  
Def: The **Q-value** of a policy $\pi$ given state $s$ and action $a$ is the value of the policy applied after folloing action $a$ from state $s$: $Q^\pi(s,a) = R(s,a) + \gamma \sum_{s' \in S} P(s'|s)V^\pi(s')$.

### Operators:
Def: The **Bellman backup operator** $B$ is an operator using the Bellman equation applied to the value function to improve its value over all states $s$ (note the max, and note that $BV(s)$ is not a function but the operator $B$ applied to the function $V(s)$).
$$B \; V(s) = \max_a \left[ R(s,a) + \gamma \sum_{s' \in S} p(s' | s,a) V(s') \right]$$


Given a particular policy $\pi$, the Bellman backup operator $B^\pi$ is defined as:
$$B^\pi \; V(s) = R^\pi(s) + \gamma \sum_{s' \in S}P^\pi (s' | s) V(s)$$

To evaluate the value of a policy $\pi$, we can essentially just run $B^\pi B^\pi B^\pi \cdots B^\pi V_0$ - that is, finding a fixed point of $B^\pi$.

Def: **Contracton operator**: Let $O$ be an operator and let $|x|$ be any norm of $x$.
If for some functions $V,V'$, we have $|OV - OV'| \le |V - V'|$, then $O$ is a contraction operator.

# 2.2 Model-Based MDP Problems
## Policy Evaluation (Computing the Value of a Given Policy):
Def: Policy Evaluation is computing the value of a given policy for every state. There are two model-based approaches to policy  evaluation.
 - Analytic Solution (Model Based): Recall the Bellman Equation states $V(s) = R(s) + \gamma \sum_{s' \in S}P(s'|s)V(s')$. For finite MDP with state space size $N$, this is expressed in matrix form as $V = R + \gamma PV \implies V = (I-\gamma P)^{-1}R$. Solving analytically takes $O(N^3)$ time.
 - Iterative Solution using DP (Model Based):
    - Run $B^\pi B^\pi B^\pi \cdots B^\pi V_0$, that is, finding a fixed point of $B^\pi$:
      For k=1... until convergence:
      ...For s in S:
      ......$V_k(s) = R^\pi(s) + \gamma \sum_{s' \in S} P^\pi(s'|s)V_{k-1}(s')$
      (Computational Complexity $O(N^2)$.)

(See Lecture 3 for Exposition of Model Free Policy Evaluation)
  - Simulation Solution (_Model Free_)- One can also solve for the value function by simulating different outcomes from a given state taking a particular policy.

## The MDP Control Problem (Computing the optimal policy)
Def: The **MDP control problem** is the computation of the optimal policy: $\pi^*(s) = \arg \max V^\pi(s)$.
There always exists a unique optimal value function and one or more optimal policies that achieve that function. The optimal policies for a MDP in an infinite horizon problem is deterministic (not stochastic) and independent of time step (stationary).
  - We often find the optimal policy through **Policy iteration**, which updates a policy by changing the next action to be the maximizer of the value given we follow the old policy for all future actions. Its not immediately intuitive why this should lead to the optimal policy, but it does. (Proof in Appendix 1). Policy iteration is equivalent to iterating **policy evaluation** to upate the value of the current policy, then  **policy imporvemnt** to generate the next policy given the current policy.
    - Loop Until Converenge:
      - **Policy Evaluation**:
        - Compute the value of the current policy $V_{\pi_i}(s) = R^{\pi_i}(s) + \gamma \sum_{s' \in S} P^{\pi_i}(s'|s)V_{\pi_{i-1}}(s')$
      - **Policy Improvement:**
        - For s in S and a in A, compute:
      $$Q^{\pi_i}(s,a) = R(s,a) + \gamma \sum_{s' \in S} P(s'|s,a) V^{\pi_i}(s')$$
        - Compute new policy $\pi_{i+1}$ for all $s \in S$:
      $$\pi_{i+1} = \arg \max_a Q^{\pi_i}(s,a) \;\; \forall s \in S$$
  - Another approach is **value iteration**. Idea: Maintain optimal value of starting in a state s if there is a finite number of steps $k$ left in the episode, iterate to consider longer and longer episodes.
    - Essentially it iteratively applies the Bellman Backup Operator $V_{k+1} = BV_k$ until converence, so value iteration is equivalently $V^* = BBB \cdots BV_0$.
    - Loop until convergence:
      - For each state s, apply the Bellman backup operator:
        \begin{align}
          V_{k+1}(s) & = \max_a \left[ R(s,a) + \gamma \sum_{s' \in S} P(s' | s,a)V_k(s') \right] \\
          \pi_{k+1}(s) & = \arg \max_a \text{of the above}
        \end{align}
    - This converges because the Bellman backup operator is a contraction if $\gamma < 1 $ (Proof in Appendix 2).
 
Note: Value Iteration and Policy Iteration are exactly the same, except the former only updates the state-value function (V), and the latter updates the state-action value function (Q). They result in the same value function at convergence. $V(s)$ is the max of the Qs over all possible actions $a$ from state $s$.

**Appendix 1: Proof of Policy Iteration**

A Proof that policy iteration leads to monotonic improvement and converges.

![Screenshot 2020-03-29 at 3.35.56 PM.png](/assets/blog_resources/9C8A52347FA8265F3FA07A492FB24880.png)

So a justification is required. In fact, we can telescope the substitution many times, and each time the inequality is ≤.

![Screenshot 2020-03-29 at 3.38.18 PM.png](/assets/blog_resources/08BA80EFA9964EEB6D2E6F84563D1EA8.png)

Because policy iteration monotonically increases the value function, and becuase the number of states is finite, it always converges. 

**Appendix 2: Proof that Bellman Operator is a Contraction Operator:**

![Screenshot 2020-03-29 at 4.16.11 PM.png](/assets/blog_resources/E23BAD0273BAD13C6397B110509789B8.png)
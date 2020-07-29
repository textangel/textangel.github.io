Note: Everything in this lecture is in the model-based (tabular MDP) setting. Extending it to the general model-free setting is an important on-going research topic.

## Probably Approximately Correct
Issue: Cumulative Regret does not distinguish between a lot of small errors and a few huge errors. Idea - PAC: Bound the number of non-small errors.
Formally, an algorithm is PAC if it chooses an action $a$ whose value is $\epsilon$ optimal ($Q(a) \ge Q(a^*) - \epsilon$) with probability at least $1-\delta$ on all but a polynomial nuber of steps in the domain (Domain is # actions, $\epsilon$, $\delta$, etc). Most PAC algorithms are based on optimism of Thompson Sampling.
In Bandits we tend to use regret for evaluation, but in MDPs we tend to use PAC. 

**PAC vs Regret**
Regret compares the state with respect to the most optimal discussion you could ever have made globally. It always judges the score compared to if it had made optimal decisions forever, and is a pretty harsh criteria.

PAC judges the score under the state distribution reached under the algorithm. PAC compares the algorithm to the optimal algorithm given the current state space you are in. (This is the meaning of $Q(a^*)$, which is the optimal value that you can attain from now on only looking into the future, ignoring whatever you could have attained if you had made other decisions in the past.) Empirically, this is why the PAC values tend to be must smaller than regret values.

## MDPs
### Optimistic Initialization
Initiate action-value function $Q(s,a)$ optimistically (eg $r_{max}/(1-\gamma)$). 
Empirically works pretty well, although no theoretical guarentees. Papers in Appendx 1
Approaches
  - Be very optimistic until confident that empirical estimates are close to true dynamics/reward parameters by maximum likelihood (Brafman and Tennenholtz 2002)
  - Be optimitisic given the information we have
    - Compute confidence sets on dynamics and reward models (confidence bounds), or
    - Add reward bonuses that depend on experience/data (see below)

Reward bonuses are the easiest to extend to the model-free case and work just as well as confidence sets. 

## PAC Algorithms for Tabular (Model-Based) Settings
### MBIE-EB Model-Based Interval Estimation with Exploration Bonus
We suppose in this setting the rewards are bounded between 0 and 1.

Given $\epsilon, \delta$, some constant $m$. 
- We keep track of some counts:
  - $n_{sa}(s,a)$ the number of times we saw each $(s,a)$ pair, 
  - $n_{sas}(s,a,s')$ the number of times we saw each $(s,a,s')$ pair.
  - $rc(s,a)$ the running sum of rewards we've gotten for any state-action pair for all time. 
- Define: $\beta = \frac{1}{1-\gamma}\sqrt{0.5 \ln(2|S| |A| m/\delta)}$
- Initialize all above values to 0 and also initialize some state.
- Initialize $Q_t(s,a) = \frac{1}{1-\gamma} \forall s \forall a$. This is the most optimistic value function possivle since rewards are bounded between 0 and 1.

Algo: Loop until convergence
  - Update the actions: $a_t = \arg \max_{a \in A} Q(s_t,a)$
  - Observe reward $r_t$ and state $s_{t+1}$
  - Update the counts and the reward sum: 
    - $n_{sa}(s_t,a_t) = n(s_t,a_t)+1,$
    - $n_{sas}(s_t,a_t,s_{t+1}) = n_{sas}(s_t,a_t,s_{t+1})+1,$
    - $rc(s_t,a_t) = rc(s_t,a_t)n_{sa}(s_t,a_t)+r_t$
  - Next, we estimate a running empirical reward model and transition model:
    - $\hat R(s,a) = \frac{rc(s_t,a_t)}{n_{sa}(s_t,a_t)}$
    - $\hat T(s' | s,a) = \frac{n_{sas}(s_t,a_t,s_{t+1})}{n_{sa}(s_t,a_t)} \forall s_{t+1} \in S$
  - Finally, we update $Q(s,a)$ using value iteration, using our empirical estimates of the reward model $\hat R(s,a)$ and transition model $\hat T(s' | s,a)$, and add in  a reward bonus which is different for each state action pair. Note that we could also do this with alogirhtms other than value iteration: While not converged, do:
    - $Q(s,a) = \hat R(s,a) + \gamma \sum_{s'} \hat T (s' | s,a) \max_{a'} Q(s',a') + \underbrace{\frac{\beta}{\sqrt{n_{sa}(s,a)}}}_{\text{reward bonus term}} \forall s \in S, a \in A$
      - Above, the reward bonus term depends on $\beta$ and the number of times we have tried that state-action pair.
      - Idea: Anyting we haven't sampled before will look maximally awesome, and anything else will be a combination of its empirical average reward plus a reward bonus, and the reward bonus will get smaller as we gather more data.

![Screenshot 2020-04-11 at 7.21.59 PM.png](/assets/blog_resources/4080FF704105ED82200B126805DCD9A8.png)

You can also think of the above as solving a new MDP with the same transitions as the old MDP, but with additional reward bonus for states we have explored less.

This algorithm is PAC - It's value is $\epsilon$ optimal ($Q(a) \ge Q(a^\*) - \epsilon$) with probability at least $1-\delta$ on all but a polynomial number of steps.

MBIE-EB algorithm is much better compared to other algorithms including greedy. Appendix 2. 

# Bayesian Model-Based RL - Thomspon Sampling for MDPs
Maintain posterior distribution over MDP models.
Estimate posteriors for both transitions $p(T | h_t)$ and rewards $p(R | h_t)$. $R$ is usualyl simmilar to Bandits (Bernoulli using Beta prior) or Multinomial/Dirchlet), and usually $T$ is a Multinomial using Dirchlet prior (Direchlet distribution parameterized by number of times each of the multinomial states was visited).

**Thompson Sampling for MDPs**
Initialize prior over dynamic model $p(T(s'|s,a)$ and rewards model $p(R_{as})$ for each (s,a), initialize start state.
Loop t = 1,2,3,...
  - Sample an MDP M: for each $(s,a)$ pair, sample a dynamics model $T(s'|s,a)$ and a reward model $R(s,a)$
  - Compute $Q_M^*$, the optimal value for the sampled MDP, and update $a_t = \arg \max_{a \in A} Q^*_M(s_t,a)$.
  - Observe reward $r_t$ and next state $s_{t+1}$.
  - Update posterior $p(T(s'|s,a)|s_{t+1}$, $p(R_{as}|r_t)$ using Bayes rule
  
end loop

Thompson sampling also implements probability matching and empirically also does really well. Generalizes to non-tabular MDPs, but kind of poorly. (**Ben Van Roy's** work on Thomspon Sampling for MDPs, Called Posterior Sampling). 

![Screenshot 2020-04-12 at 4.21.56 PM.png](/assets/blog_resources/E3572180A1CC03E2305B9BA5C65DF5C1.png)

**Generalization**
Recall PAC ensures epsilon-optimality almost all of the time except for timesteps polynomial in the state space. However, this is not useful if the state space is infinite or really large (ie pixels). How can we genrealize it to those cases?

Very active area of current research. A long way to go with theory still. No good asymptotic guarees for some of the basic algorithms. So even further than finite sample MDPs.

Recall MBIE-EB algo for finite state and action domains. What meeds to be modified for continupus/extremely large state or actions spaces? We need some way to generalize our state space, replacing the running counts we keep in the discrete space, that say "for this state I visited, similar states all will have their optimism bonus reduced". (Some sort of state embedding in a distance metric).

Most work on this is based on model-free RL, since model-based RL is very hard to scale, errors in your model specification make planning very hard. I.e. Do value function approximation with a reward bonus/exploration bonus term. Much research has been done as to the paramterization of the reward bonus term. (Appendix 3)
  - ie. density estimate model, hash-based /bucket-based discretization of continuous state spaces, etc
  

![Screenshot 2020-04-12 at 4.31.38 PM.png](/assets/blog_resources/7914B5C0256FCF00E7C09E5025FB1020.png)

![Screenshot 2020-04-12 at 4.36.03 PM.png](/assets/blog_resources/B66010243978FBB60CA61E1796B5490D.png)

Idea: You can do thompson sampling over state presetnation and also the parameters. However, hard to scale up

![Screenshot 2020-04-12 at 4.39.50 PM.png](/assets/blog_resources/AC767EA30B4A428DDC4B0CAD0C11C571.png)

![Screenshot 2020-04-12 at 4.39.53 PM.png](/assets/blog_resources/666B3902B06636C920860F2E4CD10130.png)

The last paper (Bayesian Linear Regression on top of DNN last layer to add back uncertainty performs very good).

**Appendix 1**

![Screenshot 2020-04-11 at 7.16.58 PM.png](/assets/blog_resources/AE2F8B117179FB9F9404127C92B76CC8.png)

The Even-Dar and Mansour intial value is huge, because the denominator is vanishignly small. Only a theoretical value, not useable.

## Appendix 2: Sufficient Conditions to make an RL Algorithm PAC
A lot of proofs to show algorithms are PAC follow similar lines to logic to below.

If the below 3 conditions are satisified, then an RL algorithm is PAC - that is, is $O(\epsilon)$ optimal $1-\delta$ of the time on all but $N = \left( \frac{\xi(\epsilon,\delta)}{\epsilon (1-\gamma)^2} \ln (\cdots) \right)$ time steps.
1. **Optimism**: Computed value of the algorithm is greater than or equal to the true optimal value of the state action pair minus epsilon for all time steps $Q_t(s_t,a_t) \ge Q^*(s_t,a_t) - \epsilon \;\;  \forall s,a,t$
2. **Accuracy**: The computed value of $V_t^{\pi_t}(s_t)$ minus $V_\mu^{\pi_t}$, which is a "_half-optimistic_" MDP in between our optimistic MDP and the real MDP, is within epsilon. $V_t^{\pi_t}(s_t) - V_\mu^{\pi_t}(s_t) \le \epsilon$
3. **Bounded Learning Complexity**: Total number of Q updates must be bounded, and the number of times we visit an known state action pair must be bounded by a function $\xi(\epsilon, \delta)$.
  - Strehl, Li, Littleman (2006) Incremental Model based learners with Formal Learnign Guarentees
  
### Using above to show that MBIE-EB is PAC:
1. Optimism: We show that $Q(s,a) = \hat R(s,a) + \gamma \sum_{s'} \hat T (s' | s,a) \max_{a'} Q(s',a') + \frac{\beta}{\sqrt{n_{sa}(s,a)}}$ is an upper bound to the true $Q^*$. Consider some state action pair $(s,a)$, such that $n_{sa}(s,a) < m$ (1).
Define $X_i := r_i + \gamma V^*(s_i)$ where $r_i$ is the reward from the $i$th time we sampled $(s,a)$, and $V^*(s_i)$ is the value of the next state we got to on the $i$th time that we sampled $(s,a)$.
Then $\mathbb{E}[X_i] = Q^*(s,a)$. Using Hoeffding (or some similar probability bound), we can prove that
\begin{align}
P\left(Q^*(s,a) - \frac{1}{n(s,a)}\sum_{i=1}^{n(s,a)} X_i \ge \frac{\beta}{\sqrt{n(s,a)}}\right) & \le e^{-2 \beta^2 (1-\gamma^2)} \tag{1} \\
& = \delta/(2|S| |A| m) << 1
\end{align}
Where the last equiality is from plugging in $\beta = \frac{1}{1-\gamma}\sqrt{0.5 \ln(2|S| |A| m/\delta)}$.
The above says, as you get more and more samples of this particular state-action pair, how far away could you be from optimal if you didn't know $V^*$, and the proof says that the probability of divergence more than $\frac{\beta}{\sqrt{n(s,a)}}$ is very small.
Note: $\frac{1}{n(s,a)}\sum_{i=1}^{n(s,a)} = \hat R(s,a) + \gamma \sum_{s'} \hat T(s',s,a) V^*(s')$. Optimism can be proven by induction.  Note: (1) can thus be rewritten  This is almost the same as the MBIE-EB update equation above, except with $V^*(s')$ in place of $\max_{a'} Q(s',a')$. This means that if you have a number of samples, you can bound the difference between the above and Q^* 

2. Accuracy: 
  Define: **known (s,a) pairs** - pairs for which we have sufficient data such that their estimated model is close to the true model. $(s,a)$ where $\hat R(s,a)$ and $\hat T(s'|s,a)$ is close to true $R(s,a)$, $T(s'|s,a)$. If all $(s,a)$ pairs are known, we can show that $|V^{\pi^*} - V_\mu^{\pi^*}| \le f(\epsilon)$. Let the known set of $(s,a)$ pairs be called $K$. The below result is known as the **simulation lemma**: If models are close, transition and reward model is close to the true transition and reward model, your empirical value function is also close to your true value. 

    i.e. you can propagate empirical predictive error to control error. If you have good predictive error, you can end up with small control error. The known state action pairs are a way to quantify what level you need to be 'good enough', depending on whatever epsilon you want.
  
    Define $M'$: An MDP where its rewards and transition model ($R', T'$ are
    - equal the true reward and transition model $R, T$ on all (s,a) in the known set $K$.
    - equal the $\tilde M$ reward and transition model $\tilde R, \tilde T$ on all (s,a) not in the known set $K$.
  
   Define $\hat M'$: An MDP where its rewards and transition model ($\hat R', \hat T'$ are
    - equal the empirical reward and transition model $\hat R, \hat T$ on all (s,a) in the known set $K$.
    - equal the $\tilde M$ reward and transition model $\hat R, \hat T$ on all (s,a) not in the known set $K$.
  
    We can use these to define a series of inequalities that relate the values we get in our empirical policy and the values that we get in our optimal policy -> we use these to prove accuracy.
    
3. Bounded Learning
Use the pigeonhole principle to divide your stream of experiences into episodes, and consider the probabilty of reaching an known (s,a) pair in T steps. If this probability is small, we are near optimal on the known (s,a) pairs. If this probability is large, this can't happen too many times. Since we assumed for each unknown (s,a) pair, we only visit it $m$ times, so we will eventulaly run thorugh all of them, and by the pigeonhole principle, this probability will be bounded. (i.e. We will make a bounded number of bad decisions).

(1) Suppose that in our algorithm, we only use the first $m$ samples of $(s,a)$ to compute $\hat R$ and $\hat T$, after that we throw away our data. We do this to make the high probability bound work. (Empirically and in other proofs you can remove this restriction but we use it to make the proof work).

![Screenshot 2020-04-12 at 4.15.10 PM.png](/assets/blog_resources/A066CFC9AE4965B89B20B871AA8A9F89.png)

**Appendix 3**: Exploration Bonus Improves Performane on Montezuma’s Revenge Significantly

![Screenshot 2020-04-12 at 4.38.45 PM.png](/assets/blog_resources/DF56806ABF8B3DDA2DEE5DDD0AD48902.png)
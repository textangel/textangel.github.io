---
category: cs234
---

Today we estimate the Value and Q function as a function with parameters $w$: $(V(s),Q(s,a)) \rightarrow (\hat V(s;w), \hat Q(s,a;w))$. This helps us reduce memory, computation, and experience needed to find V,Q, $\pi$. See slides for intro.

Linear Value Function Approximation (Appendix 1): If $x_1(s), x_2(s), \cdots$ are feature vectors computed over $s$, approximate the value function with:
$$\hat V(s;w) = \sum_{j=1}^n x_j(s)w_j = x(s)^T w$$

## Relation to Previous Material
The updates are still exactly the same as in previous lectues, only instaed of updating the value (V) function or the state-action value (Q) function, we update the weights $w$ instead. This requires us to multiply the update step by a gradient.

The update rule is now 
$$
\begin{align}
& \Delta w = \alpha (G_t - \hat V(s_t; w)) \nabla_w \hat V(s_t; w) \;\;  & \text{Monte Carlo} \\
& \Delta w = \alpha (r + \gamma \hat V(s_{t+1}; w) - \hat V(s_t; w)) \nabla_w \hat V(s_t; w) \;\;  & \text{TD Learning}
\end{align}
$$
Note: Change $\hat V(s_t; w)$ to $\hat Q(s_t, a_t; w)$ to use Q-values, in particular, using Q-values for the TD-learning step is SARSA and adding a $\max_{a}$ in front of the target $\hat Q(s_t, a; w)$ term is Q-learning.

Appendix 1:

**Linear Function Approximation for Value Functions (Intro)**

![Screenshot 2020-03-30 at 1.06.15 PM.png](/assets/blog_resources/C351CDAB97F91AF2E565526CA8219A83.png)

**Monte Carlo**

![Screenshot 2020-03-30 at 1.10.26 PM.png](/assets/blog_resources/48844EFB3E101A56D3051CF586AA559C.png)

![Screenshot 2020-03-30 at 1.10.45 PM.png](/assets/blog_resources/AB5B1FE5662A9033A29190EC069AF72B.png)

**Batch MC Value Function Approximation for Policy Evaluation**

If you have all data and it is small enough, you can compute the optimal weight matrix directly and analytically.

![Screenshot 2020-03-30 at 1.29.42 PM.png](/assets/blog_resources/BE8396447119DD9EB96119B117C7A1A4.png)**
**

**TD Learning Function Approximation:**

 ![Screenshot 2020-03-30 at 1.32.14 PM.png](/assets/blog_resources/1AB4D3657BC366AA5CF3B5D9A0347954.png)![Screenshot 2020-03-30 at 1.32.54 PM.png](/assets/blog_resources/F8C1E31A8A1666E150CA603ADBA7A34E.png)‘

![Screenshot 2020-03-30 at 1.33.31 PM.png](/assets/blog_resources/9212C71713741D16CDB0D6B1A05D9E81.png)‘'

![Screenshot 2020-03-30 at 1.34.01 PM.png](/assets/blog_resources/9777D5D5E6006A65D48D828A72D20A7F.png)

**Appendix 1.3 Linear Function Approximation for Value Functions: Convergence Guarentees**![Screenshot 2020-03-30 at 1.23.08 PM.png](/assets/blog_resources/3653B8885F154B6E3E4B4D669EF7386E.png)’'

Def: **balance equation-** We look at all the all the actions we could take in the current state distribution (d(s’)), and we look at where we can transition to on the next state (p(s’|s,a)), and get the new state distribution in the next state, and they have to be identical.

The **stationary distribution** d(s) tells us in the long run what proprotion of the time you will spend in each of the states.

**For Monte Carlo**

![Screenshot 2020-03-30 at 1.27.33 PM.png](/assets/blog_resources/470EA1C7908D32F5D9573CEE68FCF200.png)

![Screenshot 2020-03-30 at 1.39.19 PM.png](/assets/blog_resources/33BC5418DF8CD6D4B001DCABD7530899.png)

We look at the mean squared error of our value function approximator with respect to the stationary distribution. (Intuition: We care more about states that we expect to visit a lot). 

Then Monte Varlo Policy Evaluation with Linear Value Function Approximation converges to the weights with min MSE possible w.r.t the stationary distribution.

The TD learning oplicy evalutioan with linear function approimation converges to the weights with min MSE possible w.r.t the stationary distribution within a 1/(1-\\gamma) boundary. 

TD has a slightly higher error than MC becuase you are propagating back errors through bootstrapping.

**Appendix 1.2 Eample: Baird 1995 Example for MC policy evaluation. **

![Screenshot 2020-03-30 at 1.17.04 PM.png](/assets/blog_resources/F604BB8073CE72B325C7DB0EBFD8953F.png)

There are 7 states (s1,…,s7)

There are 7 features computed on the states (s1 = 2w1+w8, …, s6=2w6+w8, s7=w7+2w8) as in picture above.

Note that the feature space is larger than the state space which is totally okay.

There are two actions: a1 - deterministically go to s7, s2 - go to states s1,…,s6 with equal probability.

To make it nice there is a tiny change of termination every time you are in s7.

We start the weights as [11111111], and then we update the weights for every action in every episode.

MC updates the weights -3, while TD learning updates is -0.3\. This is expected since TD learning makes smaller updates but makes them more frequently.

![Screenshot 2020-03-30 at 1.35.56 PM.png](/assets/blog_resources/DB1E7016142242B2B684BAB108506656.png)

**Control Using Value Function Approximation**

This is where things start to get unstable. Now we are doing function approcmation, bootstrapping, sampling, and off-policy learning, but the real source of the instability is the off-policy learning (becuase we change our control policy over time).

Once you start combining funciton approximation (value and Q estimates are not precise) and bootstrapping (errors propagate back) and off-policy learning (the control policy changes over time depending on what current estimates are), they can fail to converge to something good.

Now the features is x\_j(s,a) cover both the states and the actions

![Screenshot 2020-03-30 at 1.48.42 PM.png](/assets/blog_resources/71F073963CC4CE60ED31AB5D7FECD68C.png)

Why this starts to pose convergence problems

![Screenshot 2020-03-30 at 1.50.03 PM.png](/assets/blog_resources/588BA5E8BDF0968436F9280FE365EA45.png)

Summary of convergence with V

![Screenshot 2020-03-30 at 1.51.32 PM.png](/assets/blog_resources/1CBA1FDE2C1A527F88275310A2255427.png)

However there are some alorithms with convergence guarentees

![Screenshot 2020-03-30 at 1.55.46 PM.png](/assets/blog_resources/FB10D74EB90F8C6DE4947B3D77283A27.png)

![Screenshot 2020-03-30 at 1.56.37 PM.png](/assets/blog_resources/11EBD901A4FB17AD90508875D47F09B9.png)

**Summary**

![Screenshot 2020-03-30 at 1.57.09 PM.png](/assets/blog_resources/22A629A515E31F86295CAAC0F9D5C587.png)

**Appendix 1.5 Eample: Baird 1995 Example for Control**

![Screenshot 2020-03-30 at 1.51.26 PM.png](/assets/blog_resources/C5CB6CB8EF695EA750A5F83845BF5854.png)

Suppose you were trying to evaluate the value of a policy a1.

Suppose we use the following approach: If you ever see data that is inconsistent with policy a1 (you took anaction in a1 but did not end up in a state specified by a1), ignore it. (This is known as a **behavorial policy**).

However, if you use TD learning on this, your weight can not converge (diverge). Main reason for this is that the distribution of data in your new dataset is not the same as the distribution of data under your desired policy.
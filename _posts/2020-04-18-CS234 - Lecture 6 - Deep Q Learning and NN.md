Nonlinear function approximations for RL:

* Kernel Based Approaches (Convergence Properties but don’t scale well)
* Neural Networks

History of NN approaches to RL:

* 1994 TD Backgammon.
* 1995-1990 Was proved that Function approximation + off policy control + bootstrapping (“the deadly triad”) can fail to converge and even simple cases can fail. People felt discouraged from using NN approaches.
* 2014 Atari

**Deep Q Learning and Improvements**

![IMG\_0353.PNG](/assets/blog_resources/33E15FDDDFE6750133831D8872A18C05.png)

![IMG\_0354.PNG](/assets/blog_resources/5190FC2120E936D9EF15AD9374F22FC0.png)In 2014 Atari Paper, DQN Used Q-learning with CNN as Value Function Approximator. They used two tricks to make it work:** Experience Replay** and **Fixed Q Targets**.

**Experience Replay**

Keep a store of prior experience and sample from that store with replacement as the training dataset.

Benefits - allows us to use a lot more data - experience is not just thrown away after being used.

Note that each sampled action (s,a,r,s’) will update the weights a different amount even if the same action (s,a,r,s’) was sampled before - because the Q function was different from before, so the update is now different.

Expereince replay was super super important for success (Appendix 1)!

![IMG\_0356.PNG](/assets/blog_resources/A099571ED17B400B5B231B91F393A50F.png)

**Fixed Q Targets**

Basically, instead of updating the target weights evert step, when calculating the bootstrap value use the same weights for every couple of steps (freeze its value) and then reset the frozen value to the new learned value every n steps. This is no longer precise gradient descent but improves stability in the training with little loss.

![IMG\_0361.PNG](/assets/blog_resources/63EA01AB4D93B23DC611BB203D87780C.png)**
**

**Some Improvements over Original DeepQL Paper.**

![IMG\_0365.PNG](/assets/blog_resources/805B01368095142AC4FA43F091CDC19B.png)

**Double DQN**

Improvement over Original Deep QL Paper.

Basically Double Q-learning applied to DQN.

Easy to Implement, impressive Results (Apendix 1)

![IMG\_0368.PNG](/assets/blog_resources/9AF705BB4C5233FD751755531A5A93EF.png)

**Prioritized Replay:** In TD Learning, the order of sampling from the replay experience affects the speed of learning, becuase it determines whether or not some updates will propagate to other states.

The authors proposed sampling updates with probability proportional to their DQN error. So tuples (s,a,r,s’) for which the DQN update is more accurate is sampled more often.

This seems to have a very high impact also! (Appendix 1) (See Appendix 2 for example)

![IMG\_0370.PNG](/assets/blog_resources/7E35C5C7B92DBA576CFAEF97B9DB97E7.png)

![IMG\_0371.PNG](/assets/blog_resources/88744C88680EB517DB0463722105F261.png)**
**

If we knew the optimal order in which to sample, the above paper proved that the time to convergence can be reduced by an exponential factor! Of course it is computationally infeasible to know the exact optimal order, but we can estimate it with heuristics.

![IMG\_0372.PNG](/assets/blog_resources/E96BEBC5DD7BA145067F3A821AAC5B91.png)

(note: alpha is a temperature setting which, if 0 gives requal prob for all actions and if large gives the max action every time).

**Dueling QN:**

Also very good effects! (Appendix 1)

Intuition: The features that you might need to write down the values of the state might be different from the features you need to specify the relative benefit of differnet actions (advantage functions) in that state.

Idea: Instead of estimating the Q value function, estimate the advantage function. 

![IMG\_0377.PNG](/assets/blog_resources/F340CDBEC3734A6FF08B8325DB6CD079.png)**
**

![IMG\_0378.PNG](/assets/blog_resources/312B1A5EE7BEA30695820AE5D92A818B.png)

However, the advantage function is unidedentifiable (there is not a unique V and A you can decompose Q into - you can add a constant to one and subtract the same from the other and get the same Q. So we add an additional constraint to force out a particular V, A.

![IMG\_0379.PNG](/assets/blog_resources/6AF1C48EDDC8F636037014E6A68260E5.png)

**Practical Tips for training DQNs:**

![IMG\_0381.PNG](/assets/blog_resources/188FC6FC5501B7DBCD0CD073DCA43117.png)**
**

![IMG\_0382.PNG](/assets/blog_resources/EBBC606249373B3E847B93FC9B354A3B.png)**
**

**
**

**Appendix 1: DQN for Atari, Summary and Results**

**
**

![IMG\_0363.PNG](/assets/blog_resources/E2D1FF7746FD28242A436437C57E395D.png)**
**

![IMG\_0364.PNG](/assets/blog_resources/4215D1F9423E745CB7848C1F76BBD480.png)

![IMG\_0369.PNG](/assets/blog_resources/9779CDACDF2B9DA1AF892F8CBF6E2037.png)

![IMG\_0373.PNG](/assets/blog_resources/F3232A687403CD1D4403E502AD1232DE.png)![IMG\_0380.PNG](/assets/blog_resources/A25B85C1A5958A27E118CA979108F5BA.png)

**Appendix 2: Mars Rover example for different values of estimate for differnet orders sampling.**

![Screenshot 2020-03-30 at 10.37.10 PM.png](/assets/blog_resources/412C1AD81D175D3A89444163354C8D45.png)

Tip: Build and debug your Q-learning implementation first before you test on Atari, Atari could take several hours.


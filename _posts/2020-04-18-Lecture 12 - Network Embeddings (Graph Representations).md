## Network node embeddings (Graph Representation Learning)

 - Goal: Efficient task-independent graph embeddings, for applications including clustering/community detection, node classification, and link prediction (Example (1)).

![Screenshot 2019-10-29 at 7.34.57 PM.png](/assets/blog_resources/03B98EC3049413ABB4778C860169809E.png)

![Screenshot 2019-10-29 at 7.36.15 PM.png](/assets/blog_resources/EE3DB606FD41C2A52B1624D29C565522.png)

Assume we have a Graph G with vertex set V, and adjacency matrix A.

Goal: Embed the nodes so that similairity in the embedding space (e.g. dot product) approximates similarity in the originla network, in terms of the graph neighborhood. To accomplish this we learn an encoding function $Enc(v)$.

Sub-goals
  1. Define an encoder (a mapping from nodes to embeddings)
  2. Define a node similarity function (i.e. a measure of similarity in the original network)
  3. Optimize the parameters of the encoder so that 
  $$similiarty(u,v) \approx z_v^T z_u$$

Aside. Basic Starter Example: "Shallow Encoding"
  - Simplest encoding approach: encoder is just an embedding-lookup. Each node is assigned a unique embedding vector. That is,
  $$ENC(v) = Z v$$, and v is a one-hot indicator vector and Z is the matrix of all the embedding columns.

![Screenshot 2019-10-29 at 7.43.43 PM.png](/assets/blog_resources/AA4249DE1FEE4F344884DC15C33A39D0.png)

Aside: How to define node similarity?
Key choice of methods is how they define node similarity.
  Should two nodes have similar embeddings if they
   - are connected?
   - share neighbors?
   - have similar 'structural roles'? (no)

## DeepWalk: Random Walk Approaches to Node Embeddings
We say that $z_u^Tz_v \approx$ Prob that u and v co-occuer on a random walk over the network.
We want to estimate the prob of visditing node v on a random walk starting from node u using some random walk strategy R ($P_R(v|u)$).
And then optimize embeddings to encode these random walk statistics. This is called **DeepWalk**

Algo: DeepWalk:
  1. Run short, fixed-length random walks starting from each node on the graph using some strategy R.
  2. For each node $u$ collect $N_R(u)$, the multiset of nodes visited on random walks starting from $u$.(2)
  3. Optimize embeddings according to: Given node $u$, predict its neighbots $N_R(u)$, to minimize the sum log likelihood of neighbors:
  $$max_z \sum_{u \in V} \log(P(N_R(u)|z_u)$$ (5,6 )
where we define probability using the softmax parameterization.

$$Pr(z_v | z_u) = \frac{\exp(z_v \cdot z_u)}{\sum_{n \in V} \exp(z_n \cdot z_u)}$$
Note that this assumes independence: Conditional likelihood factoizes over the set of neighbors (you can add log likelihoods).

In actuality, we have to estimate the log likelihood using random sampling, and the true equaiton used for computation is given below. (3) 
The above method is essentially work2vec, applied to random walks over the graph interpreted as 'sentences'. (6)


### Computational Process:
**Must use negative sampling**

![Screenshot 2019-10-29 at 8.25.35 PM.png](/assets/blog_resources/3B7C4BE77E90BF2AE516B040385D37A4.png)

But this is too expensive: Nested sum over nodes gives $O(|V|^2)$ complexity, the reason is the normalization term from the softmax.

Solution: **Negative Sampling**:
<i>Distribute the log, replace the exp with a  sigmoid function, and instead of normalizing over all the nodes, just normalize over a random sample of the nodes. You ahve to replace the exp with a sigmoid function because the sigmoid makes each term a "probability" between 0 and 1, putting the numerator and denominator in the same scale and making it possible to normalize over not all the nodes.</i>

$$\log \left( \frac{\exp(z_u^Tz_v)}{\sum_{n \in V} \exp(z_u^Tz_n)} \right) \approx$$
$$\log(\sigma(z_u^Tz_v)) - \sum_{i=1}^k {\log(\sigma(z_u^Tz_{n_i}))}, s.t.  \; n_i \tilde \;P_V$$

How to we take the ramdom samples? We sample $k$ negative nodes proportional to degree. In practice, k = 5 - 20.

(Ref 5)
So in actuality, in step 3. above, we optimize embeddings using Stochastic Gradient Descent according to 

$$\mathscr{L} = \sum_{u \in V} \sum_{v \in N_R(u)} {-\log( P(v|z_u))} $$
with $P(v|z_u)$ defined according to the softmax and efficiently appromated using negatie sampling. (4)

How should we randomly walk? Simplest idea in DeepWalk is to just run fixed-length, unbiased random walks starting from each node, but such a notion of siilarity has flaws. Node2Vec (Jure's discovery, Grover and Leskovec, 2016) uses a biased random walk instead.

## Node2Vec:
  - Goal is Same as above: Embed nodes with similar network neighborhoods close in the fature space. We frame this goal as prediction-task independent maximum likelihood optimization problem. (7)
  - Idea: Uses biased random walks to trade off between local and global views of the network. In some ways this is like an interpolation between a BFS and DFS search over the network.

![Screenshot 2019-10-29 at 9.16.04 PM.png](/assets/blog_resources/195140CE330EE82AC2A862976211BE73.png)

  Def: A biased fixed-length random walk R that given a node $u$ generates neighborhood $N_R(u)$ is defined by two parameters: (8)
  - Return parameter $p$: Return back to the previous node
      - As the random walk make a step, with prob $p$ you return to the last step. 
   - In-out parameter $q$:
      - Moving outwards (DFS) vs inwards (BFS)
      - Intuitively, $q$ is the 'ratio' of BFS vs DFS.
  
The biased 2nd order random walks explore network neirhborhoods in the following way: Suppose the random walk just traversed edge $(s_1, w)$ and is now at $w$. At every step, there are three possible actions:
  - Go back to the previous node (decrease distance 1)
  - Maintain the same distance to the starting node (simulates BFS).
  - Go to a node one farther from the starting node (simulates DFS).

![Screenshot 2019-10-29 at 9.18.19 PM.png](/assets/blog_resources/23088742F66B53C7428E08C89B101C4B.png)

![Screenshot 2019-10-29 at 9.23.13 PM.png](/assets/blog_resources/0D7B58F532CB7768E02C20BF0CF1CF7F.png)

  - We use $p$ and $q$ to put transition proabilities over the edges as sbove. 
    - a BFS-like walk has a low value of $p$, a DFS-like walk has a low value of $q$. THe difference between BFS and DFS is that BFS captures the micro-view of the neighborhood, while DFS captures the macro-view of the neighborhood. (Example- (8)) The neighborhood $N_R(u)$ are the nodes visited by the ramdom walk.

So the full algo is: 
### Algo (Node2Vec):
  1. Compute random walk probabilities given $p$ and $q$.
  2. Simulate r random walks of length $l$ starting from eahch node $u$
  3. Optimize the node2vec objective using SGD.
  
Node2vec has linear time-complexity, and all three steps are individually parallelizable. 

## Final Notes
### How to use embeddings:
  - Clustering/community detection: Cluster nodes/points based on $z_i$.
  - Node classificaiton: Predict label $f(z_i)$ of node $i$ based on $z_i$.
  - Link prediction: Predict edge $(i,j)$ based on $f(z_i,z_j)$
    - We can have $f(z_i,z_j)$ be the concatenation, avg, product, or differnece between the embeddings. 
    - Concatenate: $f(z_i,z_j) = g([z_i,z_j])$
    - Hadamard: $f(z_i,z_j) = g(z_i * z_j)$ (per-coordinate product)
    - Sum/Avg: $f(z_i,z_j) = g(z_i+z_j)$
    - Distance: $f(z_i,z_j) = g(||z_i-z_j||_2)$
  
### Which Approaches to use
Node2vec performs better on node calssification whiel multi-hop methods perform better on link prediction.
Random walk approaches are gnerally more efficient. 

### What if we wanted to embed entire graphs?
You can use this to go graph anomay detection, classifying social networks, analyze reddit comment threads, represent molecules as graphs and identify carcinogenic networks etc.
Two approaches to full graph-embedding:
  1. Simple idea: The embedding of the graph is simply the sum of the embeddings of the nodes.
  2. Introduce a 'virtual node' which is connected to all the nodes in the graoh to represent the (sub) graph and run a standard graph embedding technique.

### Other Random walk ideas:
  - Different kinds of biased random walks
    - Based on node attributes (Done et al 2017), based on learned weights (Abu-El-Hajia et al 2017)
  - Alternative optimization schemes
    - Directly optimized based on 1-hop and 2-hop random walk probabilites (LINE)
  - Network preprocessing techniques
    - Run random walks on modified versions of the original network. (Struct2vec, HARP)

## Appendix: Why random walks?
  1. Expressivity: Flexible stochastic definiton of node similarity that incorporates both local and higher-order neighborhood information
  2. Efficiency: Do not need to consider all node paits when training, only need to consider nodes that co-occur on ranodm walks.

#### Intuition Between Random Walks:
Find embedding of nodes to d-dimensional space so that node similarity is preserved.
Idea: Learn node embedding such that nearby nodes are close together in the network. Given a node $u$, how do we define nearby nodes? Define a node **neighborhood**: $N_R(u)$ is the neighborhood of $u$ defined by some strategy $R$. 

#### Feature Learning as Optimization:
Given $G(V,E)$, our goal is to learn a mapping $z: u \rightarrow \mathbb{R}^d$ to minimize the log-likelihood objective: 
$$max_z \sum_{u \in V} {\log P(N_R(u) | z_u)}$$ where $N_R(u)$ is the neighborhood of node u.
Given node $u$, we want to learn feature representations predictive of nodes in its neighborhood $N_R(u)$, under the assumption that condition likelihood factorizes over the set of neighbors:
$$log(P(N_R(u)|z_u) = \sum_{v \in N_s (u)} \log(P(z_v | z_u))$$ 




## Notes:
(1)

![Screenshot 2019-10-29 at 7.35.41 PM.png](/assets/blog_resources/DF288A68EAAD6CC14D6BAC1B47F06ED9.png)

(2) $N_R(u)$ can have repeat elements since nodes can be visited multipe times on random walks.

(3) Softmax works because the sum of the exponents $\approx$ the maximum of the exponents, the big guy eats up all the others.

(4) Note that this is a random sample over all nodes, not just nodes in the neighborhood.

(5)
So in actuality, in step 3. above, we optimize embeddings using Stochastic Gradient Descent according to 

$$\mathscr{L} = \sum_{u \in V} \sum_{v \in N_R(u)} {-\log( P(v|z_u))} $$
with $P(v|z_u)$ defined according to the softmax and efficiently appromated using negatie sampling. (4)

(6) This is very analogous to work2vec. In work2vec, we have a sliding window over words and we try to predict what words are near to a particular one to form the embedding. In Node2Vec, we have a random walk over nodes and we try to predict what nodes are close to a particular one to form the embedding. 

Specifically, node2vec is, we take a graph, we get a random walk over the graph, that gives us a sequence of nodes which is like a 'sentence', and then we run 'word2vec' on that sequence to determine what nodes should be in the neighborhood of a particular node.

Similarly to word2vec, each node can have 2 embeddings, a separate embedding for whether they are in the center of the set or in the context of the set.

(7) Key observation: Flexible notion of network neighborhood $N_R(u)$ of node $u$ leads to rich node embeddings. Use flexible, biased random walks that can trade off between local and global views of the network. 
Using the 1st order labeled random walk, develop biased 2nd order random walk R to generate netwokr neighborhood $N_R(u)$ of node $u$.

(8)![Screenshot 2019-10-29 at 9.28.41 PM.png](/assets/blog_resources/FDF7E352C0E69C13027837CD8899416B.png)

In the micro view (BFS, medium p high q), the yellow nodes are the fringe nodes, the blue nodes are connectors between the local nodes and the center nodes, and the blue nodes are the center nodes. In the micro view, you never have a view of the whole network, just what a node looks like in the context of its surroundings, so you get nodes with similar functions (similar types of connectedness) embedded to similar coordinates.

In the macro view (DFS, medium p low q), the nodes are similar to each other based on how similar they are in the context of the whole graph. So you see clusters of nodes (communities) embedded similarly in the graph.






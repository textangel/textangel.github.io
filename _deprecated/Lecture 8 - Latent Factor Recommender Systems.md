---
category: cs246
---

Netflix Prize

 - The winning entry factorized the problem into global (baseline), regional (SVD), and local levels (collab filtering).

![Screenshot 2019-10-25 at 7.01.10 PM.png](/assets/blog_resources/24F96AE0C0E6BBD710F62E7EB2C3A1BC.png)

Global:
- Baseline estimation: Avg rating + avg rating of movie above/below avg + avg rating of user above/below avg.

Regional:
  (SVD/Matrix Factorization and latent factors)
  
Local Neighborhood (k-nearest CollabFiltering/NN)

# The Global and Local Levels
So Global and Local together (with some problems) is:

![Screenshot 2019-10-25 at 7.07.40 PM.png](/assets/blog_resources/23D4398769536F52511B051961C19335.png)

We update this model to address the problems so that we replace $s_{ij}$ with some learned weights $w_{ij}$.

![Screenshot 2019-10-25 at 7.13.00 PM.png](/assets/blog_resources/9F5AF82B1EE6C405D276B482B8A6C4CB.png)

The error metric is RMSE so we minimize RMSE (or the square of that, SSE). So we just learn $w\_{ij}$ such that SSE on training data is mnimized (by grad descent, adam, etc)

![Screenshot 2019-10-25 at 7.16.49 PM.png](/assets/blog_resources/C176CA6F1B71955F3A1FC5CFA41F7BD6.png)

# The Regional Level (SVD/Matrix Factorization and latent factors)

We want to embed our movies in a low dimensional space so people who like similar movies are embeded together in this space.

Do "SVD" on the Netflix utility matrix. (not really SVD - we call it Matrix Factorization- we ignore empty values for reconstruction error) (1)

![Screenshot 2019-10-25 at 7.27.08 PM.png](/assets/blog_resources/C99E24ABAC21E58AD8D0BC0A880BE00A.png)

How to estimate the missing rating of user x for item i? Multiply the corresponding factors. 

### Modified SVD / Matrix Factorization:
We cannot actually use SVD because of missing entries, instead we learn the values to fill the embedding matrices through a learning algorithm. We no longer require P,Q to be orthogonal/unit length.

![Screenshot 2019-10-25 at 7.35.33 PM.png](/assets/blog_resources/B1F88D471DFD767686F3A024C452B12D.png)

I.E. predict $p$,$q$ using alternating least squares
The model deals with overfitting as we choose higher and higher values of latent factors $k$. (2)

We use l2 regularization to deal with overfitting. 

![Screenshot 2019-10-25 at 7.41.19 PM.png](/assets/blog_resources/DAF71A89152B9ED2F70C32F0AE9DE0DA.png) (3)

So in summary, the goal is:

![Screenshot 2019-10-25 at 7.50.55 PM.png](/assets/blog_resources/FF213792F4D90A33ACB4489C34CF765C.png)

We optimize it using alternating stochastic gradient descent (Alternating Least Squres ALS)

## Extending Latent Factor Model to Include Biases
  Quite simple, instead of only training to fit $r_{xi} = q_ip_x$, above, we also treat $b_x$, $b_i$ as learned values and fit with ALS:

![Screenshot 2019-10-25 at 7.59.19 PM.png](/assets/blog_resources/F248F1E9AB3886E15FA56921117FA156.png)

So, 

![Screenshot 2019-10-25 at 8.01.43 PM.png](/assets/blog_resources/4164A39F7E1EDCE7C827FC6B091BFD12.png)

The winning team also incorporated some data insights, factoring in metadata by change in rating over time, rating edge.

The winning model was the above model parameterized on time + throwing in a lot of metadata features in and hoping that it will work.

In recommender systems, what is really important is the regularization. The winning model in Netflix prize had over a billion parameters.

![Screenshot 2019-10-25 at 8.05.36 PM.png](/assets/blog_resources/76359032B628438BD9EE4B2D4BFEE480.png)

(1) We do SVD because SVD gives us exactly what we want - minimum SSE and RMSE for reconstruction error. However, we must modify SVD so that it is only operating over existing entries.

(2) Overfitting occurs when model has too much free parameters (too much freedom) that it starts fitting noise.


(3) Recap: Regularization:

![Screenshot 2019-10-25 at 7.41.19 PM.png](/assets/blog_resources/DAF71A89152B9ED2F70C32F0AE9DE0DA.png)

The 'length' to be penalized is the size of the parameteres that we use. If we are constrained to use rather small parameters for each value of p and q, the model better be very good and very optimized.

If lambda is large, the model doesnt care about the 'error' so much as making the size of the weights tiny. If I have tiny weights, then I cannot make big mistakes.

For users with sparse data, the error cannot be very high anyways (there are not that many error terms to sum over). Thus, the regularization focuses on penalizing picking p, q too large. For users with a large amount of rich data, the error term is relatively large relative to the regularization term. In this case the model will focus most on shrinking the error term.

Notes:
1) Note that the baseline predictor (mean + user bias + movie bias) is actually really, really good! NEVER underestimate the baseline predictor
2)

![Screenshot 2019-10-25 at 7.54.26 PM.png](/assets/blog_resources/7A4227269913A3322EFF8026CF7F29AD.png)
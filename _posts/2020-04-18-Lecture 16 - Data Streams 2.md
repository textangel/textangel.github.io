# Bloom Filters (Hash Filters)- Filtering a Data Stream
## Applications: 
- We know 1 billion 'whitelisted email addresses' for each user. If an email comes form one of these, it is NOT spam.
- Publish-subscribs system - You are collecing a lot of mesages (news articles). People express interst in certain sets of keywords. Determine whether each message matches the user's interest
- Content Filtering - You want ot make sure the user does not ee the same content twice.

## Setting
 - Every element of a data stream is a tuple. Given a list of keys S, determine which tuples of the stream are in S.
 - Obvious solution: Hash table lookup. But suppose we do not have enough memory to store all of S in a has table

## Bloom Filter - Use multiple hash functions!
- See (1) for first cut solution.
- Consider a set of keys $S$ that we want to filter such that $|S| = m$, and a single bit array $B$ of length $|B| = n$. Use $k$ independent hash functions $h_1,...,h_k$. 
- Initialization:
  - Set $B$ to all $0$s
  - Hash each element $s \in S$ using each hash function $h_i$, set $B[h_i(s)]=1$ (for each $i=1,..,k$) 
- At run-time, when a stream element with key $x$ arrives, if $B[h_j(x)] = 1$ for all $i=1,...,k$, then declare that $x$ in S. Otherwise, discard $x$.
- **Analysis**
  - Throwing $km$ darts at $n$ targets.
  - So fraction of 1s is $1-e^{-km/n}$.
  - But we have $k$ independent hash functions and we only let the element $x$ through if all $k$ hash element $x$ to a bucket of value 1. So false positive probability is $(1-e^{-km/n})^k$
  - There is an o**ptimal number of hash functions** $k = n/m \cdot \ln(2)$ which gives you the minimum error. Less than that and you are not using enouch hash functions, more than that and you are polluting hte bit vector with 1s. (2)
- Bloom filters have the same result whether you use one big B array or k small B arrays.

# Flajolet-Martin Algo - Counting Distinct Elements in a Stream
Problem: Data Stream contains items from a universe of elements chosed from a size of size N. Maintian a count of the number of distinct elemnts seen so far. 
Obvious Approach: Maintain the set of elements seen so far in a hash table. But suppose we do not have enough memory to store all the elements seen so far. Real problem: What if we do not have space to maintain the set of elements seen so far? We wish to estimate the count in an unbiased way. Acept the count may have a little error but limit the prob the error is large.

## Applications
  - How many different words are found among the Web pages being crawled at a site? 
  - How many different Web pages does each customer request in a week?
  - How many distinct products have we sold in the last week.
  

## Flajolet-Martin Algo - Counting Distinct Elements in a Stream
  - See (4) for motivation and first cut solution.
  - Use many has functions $h_i$ that maps each of $N$ elements to at least $\log_2 N$ bits.
  - For each hash function $h_i$, for each stream element $a$, compute the number of trailing 0s in its hash and store it; at the end of the process obtain $r_i$ which is the maximum number of trailing 0s seen so far for that hash function, and compute $R_i=2^{r_i}$
  - Partition your samples of $R_i$ into small groups. Take the median of the groups, and then take the average of the medians.

# AMS Method - Estimating Moments in a Stream
Suppose a stream has elements chosen from a set $A$ of $N$ values. Let $m_i$ be the number of times value $i$ occurs in the stream. The $k$th moment is $\sum_{i \in A} (m_i) ^k$.

##Special Cases
 - Oth moment- number of distinct elemnts - use Flajolet-Martin
 - 1st moment - count the number of elements (length of the stream)
 - 2nd moment - surprise number $S$ - a measure of how uneven the distribution is. 
 - 3rd moment - Skew, 4th moment - kurtosis (peakedness and tail weight)

## Example: 
  - Suppose we have a stream of length 100, 11 distinct values.
  - If the item **counts** are: 10,9,9,9,9,9,9,9,9,9,9, the distribution is very even, the second moment (surprise) S=910.
  - If the item **counts** are: 90,1,1,1,1,1,1,1,1,1,1, the distribution is very uneven,the second moment (surprise) S=8110.

## AMS method for the second moment:
  - Gives an unbiased estimate of a give moment (can also work for 3rd and 4th moemnt).
  - We pick and keep track of a limited many values $X$. For each variable $X$ we store $X.el$ and $X.val$
    - $X.el$ corresponds to the item $i$
    - $X.val$ corresponds to the count $m_i$ of item $i$.
      - Note that this requires a count in main memory so the number of $X$s is limited.
    - Our goal is to compute $S = \sum_i m_i^2$.
  - How to set $X.val$, $X.el$
    - Assume stream has length $n$. 
    - Pick some random time $t$ uniformly over the life of the stream. Let $i$ be the item of the stream at time $t$. Let $X.el = i$.
    - Then, we maintian a count $c$ of the number of $i$s in the stream starting from the chosen time $t$. Set $X.val=c$.
    - Then, the estimate of the second moment $\sum_i m_i^2$ is 
      $$S = f(X) = n (2 \cdot c -1)$$
    - In reality, we will keep track of multiple $X$s, $(X_1, X_2, ..., X_k)$ and our final estimate will be $S = 1/k \sum_j^k f(X_j)$.
  - Proof:
    - Let $c_t$ be the number of times item at time $t$ appears from time $t$ onward. 
      - (Slide Pic here)
    - Then $E[f(X)] = \frac{1}{n} \sum_{t=1}^n n(2 c_t -1)$. This summation counts over the times of the stream, but we can rearrange it into an equivalent form that counts over the _items_ of the stream $\frac{1}{n} \sum_i n \cdot ( (2m_i -1) +  (2m_i -3) + \cdots + 3 + 1)$. That is, when I first see the item I get $2m_i -1$, when I next see it I get $2m_i-3$ and so on and so forth until when I last see it, I get 1.
    - So $E[f(X)] = \frac{1}{n} \sum_i n \cdot (1 + 3 + \cdots + (2m_i -1)) = \frac{1}{n} \sum_i n \cdot m_i^2 = \sum_i \cdot m_i^2 = S$, the second moment as desired!
  - If we wish to estimate the $k$th moment, just change the function to sum over.
    - FOr k=2 use $n (2c-1)$
    - for k=3 use $n (3 c^2 -3c +1)$, etc
# Notes
(1)
## First Cut Solution
  - Given a set of keys S that we want to filter
  - reate a bit array B of n bits, itinally all 0. B can be very long (ie 8 billion bits).
  - Choose a has function h with range [0,n]
  - Hash each member of s in S to one of n bucksts and set that bit to 1 $B[h(s)]=1$
  - Hash each element a of the stream and output only those that hash that bit that was set to 1.
  - We get candidate values - create false positive but no false negatives.
  - **Example**: 
    - We have 1 billion email addresses and 8 billion bits. We hash each email address to a bit. Because hashes may overhap, a little less than 1/8 of the darts will be false positives. What's the exact number?
    - The exact number of false positives as a ratio of the whole is as follows: We have m darts and n targets. What is the probaility that a target gets at least one dart? $1-(1-1/n)^m =1-(1-1/n)^{n\cdot m/n} = 1-e^{-m/n}$. In our case, the value is $1-e^{-1/8} = 0.1175$.
    - 
(2)
- For m = 1 billion, n = 8 billion, the optimal is at $k=6$ where the prob of false positive is $(1-e^{-6 \cdot 1/8})^6 = 0.0216$, five times smaller than the naive value of $0.01175$!

(3)
- **Proof**
    - Intution: h(a) hashes a with equal prob to any of N values. So h(a) is a seeuqnce of $log_2 N$ bits, where $2^{-r}$ fration of all $a$s have a tail of $r$ zeroes. It takes about $2^r$ items before we see a value with $r$ trailing zeros.
  - Suppose $m$ is the number of distinct elemtns so far. We show that the probability of finding a tail of $r$ zeroes goes to 1 if $m >> 2^r$ and goes to 0 if $m << 2^r$, if $m$ is the number of distinct elements seen so far in the stream.
    - The prob of NOT seeing a tail of length r among m distinct elements: $(1-2^{-r})^m = (1-2^{-r})^{2^r(m2^{-r})} \approx e^{-m2^{-r}}$
    - If $m << 2^r$, then the above probabilty tends to 1.
    - If $m >> 2^r$, then the above probability tends to 0.
    - 
(4)
## First Cut Solution for Frajolet-Martin
  - Pick a hash function $h$ that maps each of the $N$ elements to at least $\log_2 N$ bits.
  - For each stream element $a$, let $r(a)$ be the number of trailing 0s in $h(a)$ = position of first 1 counting from the right. 
  - Record R = the maximum r(a) seen. Then the estimate of number of distinct elements seen is $2^R$. Proof given in (3).
  - One problem with this formulation: $E[2^R]$ is actually infinite, as prob halves when $R -> R+1$ but value doubles. SO you can't just compute one hash function and take its value as the estimate. Workaround involves using many hash functions $h_i$ and getting many samples of $R_i$.
    - But how would the samples of $R_i$ by combined?
      - _Average?_: Problematic. One term with very large value for $2^{R_i}$ will overpower the others.
      - _Median?_: Problematic. All estimates are a power of two, error is large.
      - Soution: Partition your samples into small groups. Take the median of the groups, and then take the average of the medians.
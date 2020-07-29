---
category: cs246
---

## Lecture 2 - Frequent Itemset Mining and Association Rules 
For motivation of problem see below (2).
For formal definition of problem see below (3)

##Association Rule Mining:
Problem: Find all association rules with support >= s and confidence >= c.
Solution:
1. Find all frequent itemises I though a frequent Itemsets algo (the harder part, below).
2. Rule generation. For every subset A of I, generate a rule A → I \ A
  - Since `I` is frequent, `A` is also frequent
	- Variant 1: Single pass to compute the rule confidence: `confidence(A,B→C,D) = support(A,B,C,D) / support(A,B)`
	- Variant 2: Observation: If A,B,C→D is below confidence, so is A,B→C,D. Can generate “bigger” rules from smaller ones!
3. Cross out rules that don’t fulfil the constraints. 

### Frequent Itemsets Algorithms** (for step one above)

Typically data is kept in flat files stored by disk, basket by basket.
#### Strategy:
Identify frequent items, and from frequent items, build frequent pairs, from those build frequent triples. (AVOID nested for loops!)
Computational Model:
The true cost of mining disk resident data is number of disk IOs, and in practice, association rule algos real the data in passes - all baskets read in turn.
So we want to minimise the number of passes the algorithm makes over the data (reading disk each time).
Main memory is the main bottleneck in this case. 
- As we read baskets, we need to count pairs of itemsets. The number of different things we can count is limited by main memory.
- Swapping counts in and out of main memory is a disaster. We only want to keep the frequency of an item in main memory if it is likely to be frequent.
	- We try to ignore the count of frequent items we don’t think will be frequent.

#### Frequent Pairs
- Frequent pairs is the most expensive task in frequent itemsets, because freq pairs common, freq triples rare.
- Approach. We always need to generate all itemises but we only want to count those tenses that in the end turn out to be frequent.
- Naive Algorithm:
	- Read file once, count in main memory the occurrences of each main memory.
	- Fails if (# items)^2 exeeds main memory. # items can be 100K (Wal-Mart), requiring 20GB memory or 10B (web pages), requiring terabytes, cannot do.
- So how do we keep track of pairs of items?
	- Approach 1: Use a triangular matrix. This is dense and uses too much memory O(n^2). Only better if most pairs occur.
		- Uses 4 bytes for each pair, regardless of whether or not they appear.
	- Approach 2: Keep a table of triples [I,j,c]= “the count of the pair of items {I,j} is c. This is sparse, so we save a lot of memory.
		- If integers and item ids are 4 bytes, we need ~12 bytes for pairs with count >0.
		- So if < 1/3 of all pairs appear, use approach 2.

####A-Priori Algorithm
(Dynamic Programming) - what if item pairs do not fit into memory?
A two pass approach called  limit the need for main memory.
Uses monotonicity of ‘frequent’ - if A is frequent, B is not frequent, A+B will never be frequent.
- Algo for each k
	- Pass 1: Read basket and count in main memory the # of occurrences of each individual item (O(n) memory).
	- Pass 2: Read baskets again and keep track of the count of only those pairs where both elements are frequent 
		- Requires O(frequent items ^2) memory.
- For each k, we construct two sets of k-tuples using A-priori
	- `Ck = candidate k-tuples` = those that might be frequent Sts (support >= s) based on information from the pass for k-1
	- `Lk = the set of truly frequent k-tuples`. By removing non-frequent k-tuples form Ck.

#### PCY Algorithm (Hash-based extension of A-Priori)
- How to use memory even more efficiently.
- In Pass 1 of A-priori, most memory is idle. We only store individual item counts. Can we use the idle memory to reduce the memory required in pass e?
- Pass 1 of PCY: In addition to item counts, maintain a hash table with as many buckets as fit in main memory.
	- Keep a count for each bucket into which pairs of items are hashed.
		- For each bucket just keep the count, not the actual pairs that hash to the bucket!
		- Observation: If a bucket contains a frequent pair, then the bucket is surely frequent. So for a bucket with total count less than s, none of its pairs can be frequent. The reverse is not true.
- Pass 2:  Only count pairs that hash to frequent buckets.

#### PseudoCode

```
#Original to A-Priori
		FOR (each basket) :
			FOR (each item in the basket) :
				add 1 to item’s count;
#new in PCY
			FOR (each pair of items) :
				hash the pair to a bucket;
				add 1 to the count for that bucket;
```

PCY Memory Use:

```
![Screenshot 2019-10-02 at 8.20.03 PM.png](/assets/blog_resources/FE93E52F62047360414BC741E899DDCB.png)

```

Note: We are not just interested in the presence of a pair, ut we need to see whether it is present at least s (support) times.

## Extensions: Other frequent Itemsets Algorithms
Approximate Frequent itemises in <=2 passes. Use 2 or fewer passes, but *may miss some frequent itemsets*.
Strategy:
- Simple Algorithm
	- 	Do random sampling. Sample a set that fits in main memory. Take a random sample of the market baskets. Run a-priori or one of its improvements in main memory. But reduce support threshold proportionally to match the sample size. It is recommended to reduce the threshold a little more than the proportional to catch false positives.
	- Optionally, go over the entire data once, to get rid of false positives. 
- Savasere-Omiecinski-Navathe (SON) Algorithm
	- Repeatedly read small subsets of baskets into main memory, and run an in-memory algorithm to find all frequent itemises. 
		- Note: we are not sampling, but processing the entire file in memory-sized chunks. An itemset becomes a candidate if it is found to be frequent tin any one or more subsets of the baskets.
	- On a second pass, count all candidate itemises and determine which are truly frequent.
	- Repeat both passes for each k.
- Toivonen’s Algorithm
	- Pass 1: Start with a random sample, but lower the threshold slightly for the sample (ie s/125 instead of s/100)
		- Find frequent itemises in teh simple.
		- Add to the itemises that are frequent in the sample the *negative border* of these itemsets. The negative border: An item is in the negative border if it is not frequent in the sample but all of its immediate subsets (delete exactly one element) are. 
	- Pass 2: Make sure that those itemises are actually frequent by going over all your data.
	- Repeat 1,2 for each k.
	-  Note: If no itemset from the negative border is frequent, we have found all the frequent itemsets. If we find something that infrequent, we must start over gain with another sample. So we choose the support threshold a bit higher so that we are less likely to miss it.

**Extensions**
- Assoc Rules with Intervcals
	- Men over 65 have 2 cars
- Association rules when items are in a taxonomu
	- Bread, Butter-> Fruitjam
	- BakedGoods, MilkProduct -> PreservedGoods
- Lower the support s as itemises get bigger
- More extensions to A-priori:
	- ‘multistage’ and ‘multihash’
- There Is research in this field

## Notes: 
(2) 
#### Motivation
Intuition:
Market basket model
		- Goal: identify items that are bought together by sufficiently many customers
		- Approach: Process the sales data collected with barcode scammers to find dependencies among items
		- A classic rule - diapers and beer. ‘People who bought X also bought Y’

- Given: 
	- A large set of items (things sold in a supermarket)
	- A large set of baskets. Each basket is a small subset of items
- Goal:  Discover association rules - people who bought {x,y,z} tend to buy {v , w}.
- A general many-to-many mapping between two types of things. We find connections between the ‘items’, not the baskets

#### Examples of items and baskets
- items=products/baskets =shopping basket
- items=words/baskets =documents
	- for word embeddings, words associated with each other
- items = basepairs/ baskets=genes
- items = drugs and side-effects/ baskets=patients
	- - requires extension, model the absence of an item as we’ll as the presence
Note: We can flip this around:
- baskets=sentences, items = documents in which those sentences appear
	- ie for plagiarism detection
	
(3) 
###**Formal Def of Problem**
	- Def: Support for items I: Number of baskets containing all items in I (as a fraction of total number of baskets)
	- Goal: Given a support threshold s, find all set of items that appear in at leas s basket - a *frequent itemset*

Example: 

**Items**= {milk, coke, pepsi, beer, juice} 
Support threshold = 3 baskets
`B1 ={m,c,b} B3 ={m,b} B5 ={m,p,b} B7 ={c,b,j}`
`B2 ={m,p,j} B4={c,j}`
`B6 ={m,c,b,j} B8 ={b,c}`
 			-> Frequent itemsets: `{m}, {c}, {b}, {j}, {m,b} , {b,c} , {c,j}.`

Def: Association Rule
An association rule `{i1, i2,...,ik} → j means: “if a basket contains all of i1,...,ik then it is likely to contain j”`
In practice there are many rules, want to find significant/interesting ones!

**Association Rule metrics**
Confidence, Support, Interestingness

- **Support**:  Support for items I: Number of baskets containing all items in I (as a fraction of total number of baskets)
- **Confidence** of association rule on `j` is the probability of `j` given ` I={i1,…,ik}`.
	`conf(I->j) = support(I u j) / support(I)` (1)
	- Not all high-confidence rules are interesting.  The rule X → milk may have high confidence for many itemsets X, because milk is just purchased very often (independent of X) and the confidence will be high.
- **Lift**: Lift(A -> B) refers to the increase in the ratio of sale of B when A is sold. Lift(A –> B) can be calculated by dividing Confidence(A -> B) divided by Support(B). Mathematically it can be represented as:
  `Lift(A→B) = (Confidence (A→B))/(Support (B))`
This is the same as the probability of all of the items in a rule occurring together (otherwise known as the support) divided by the product of the probabilities of the items on the left and right hand side occurring as if there was no association between them.

- **Interestingness** of an association rule `I->j`
	- `Interest(I -> j) =| conf(I -> j) - Pr[j] |`
	Interesting rules are those with high positive or negative interest values (usually above 0.5).


Q1: Why is main memory the computational bottleneck for frequent itemset computation?
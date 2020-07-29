**Distributed Word Representations**

There are a lot of ways to make really good word presentations using different corpora, different reweighting paradigms…

![Screenshot 2020-02-17 at 12.09.36 PM.png](/assets/blog_resources/3B8D6313B3B9EE298B841A628600AEA7.png)

- You can look at particular word windows, and also scale w.r.t distance from target word (i.e. the score is 1/n where n is the distance from target word).

- Larger flatter windows will capture more semantic windows. Smaller, more scaled windows capture more syntactic information.

- You often want to reweight your raw co-occurence counts; Often we use PMI (pointwise mutual information).

**
**

**Distance Measures**

 - Cosine distance

 - Matching based measures (Appendix 1) 

Generalizations w.r.t distiance measures. (For other generalizations, see 

Appendix 5)

 - Euclidean, Jaccard and Dice with raw counts will favor raw frequency over the distributional patterns. 

 - Euclidean with L2 norms is equivalent to cosine.

 - Jaccard and Dice are equivalent w.r.t ranking.

 - Both L2 norms and probabiltiy distrbutions can obscure differences in the amount/strength of evidence, which can have an effect on the realiability of coming/normed-euclidean, and KL Divergence. These shortcomings might be addressed through weighting scemes.

 - Skew is KL but with a prelinary step that gives special credence to the refernece distribution

**Reweighting**

Amplify the thigns that are important and trustworthy.

L2 norming and probability distribution (enforce sum=1) are two ways to do this. Another idea is to amplify counts smaller than expected counts and diminish counts (Appendix 3)

*
*

*Pointwise Mutual Information* (PMI) - observed / expected in log space. The most important reweighting factor. 

![Screenshot 2020-02-17 at 12.35.24 PM.png](/assets/blog_resources/4C27F8B3ABA69DFEA8690507D2B9A87D.png)

**Positive PMI (the default for generating word embeddings/vector space models) - **

 What’s actually used usually is `Positive PMI = ppm(X,ij)= max(0,pmi(X,i,j)`

 PMI is actually undefined when X\_ij = 0\. Instead of setting log(0)=0, we cut off all values at 0.

Problem with PMI - PMI tends to really amplify really small values that you cannot trust.

GloVE is basically regularized reduced-dimensional PMI.

**TfIdf -**

**Idf amplifies Tf values that occur in very few documents**

![Screenshot 2020-02-17 at 1.21.41 PM.png](/assets/blog_resources/54EC8F3073844644E76A2C62FECF4F3B.png)

![Screenshot 2020-02-17 at 1.12.01 PM.png](/assets/blog_resources/149E4CA39FB82D440E055FD9FCCDEF85.png)

Tf-Idf distributions tend to look like PPMI. (Appendix 4)

**Dimensionality Reduction**

**Latent Semantic Analysis (LSA) - 1990, aka Truncated SVD.**

Essentially SVD. 

**Autoencoders**

**
**

**GloVE**

Roughly speaking, the obectvei is ot learn vectors to works such that their dot product is proportitional to their probability of coocurrence. 

![Screenshot 2020-02-17 at 2.07.45 PM.png](/assets/blog_resources/A42F7C3790E1D09A5B177CEA6A437BE4.png)

**Retrofitting:** Distributional representations are powerful and easy to obtain, but they tend to reflect only similarity. Can we get word vectors which also reflect some sort of knowledge about the world (encapsulated in a knowledge graph?)

![Screenshot 2020-02-17 at 5.02.24 PM.png](/assets/blog_resources/91332366239EA143AF157B7D013BD383.png)

Extensions to Retrofitting

![Screenshot 2020-02-17 at 5.19.21 PM.png](/assets/blog_resources/F684448E249EAB62B873D6CFC71D67AE.png)

**Appendices**

**Appendix 1:**

Matching based distance measures.

 - Matching coefficient (sum smallest of two values along all dimensions)

 - Jaccard 

 - Dice (Intersection over union)

 - Overlap

![Screenshot 2020-02-17 at 12.21.14 PM.png](/assets/blog_resources/DBBDBF2AEFCE787F78CB6508E5ECF2AB.png)

**Appendix 2:**

You can convert the embedding vectors into a probability distribution, and then use KL distribution between columns. ONLY applications

KL Divergence

Note: KL is not symmetric and may blow up if $q\_i ~ 0$

![Screenshot 2020-02-17 at 12.24.43 PM.png](/assets/blog_resources/730D00686F5E332DB6C2683B21D7CD21.png)

![Screenshot 2020-02-17 at 12.24.35 PM.png](/assets/blog_resources/E4DCB384BFDE877281663105A49F1E3C.png)

Appendix 3:

![Screenshot 2020-02-17 at 12.33.34 PM.png](/assets/blog_resources/CA99E615F883845CD837FDFB932E73A6.png)

Appendix 4:

Cell-value distributions in word embedding matrix for different reweighing schemes

![Screenshot 2020-02-17 at 1.23.42 PM.png](/assets/blog_resources/6D123E5440BF977170F0D7BF034A9245.png)

Appendix 5:

![Screenshot 2020-02-17 at 1.53.54 PM.png](/assets/blog_resources/EC6B8C9D537A51C9D3817575BC50080B.png)

Appendix 6: Fasttext - naive way to get subword modeling in.

![Screenshot 2020-02-17 at 1.55.43 PM.png](/assets/blog_resources/F4E59AE538F15EFECC45D9B4436D81E5.png)

Appendix 7:

The point of all this is that you get better word vectors~

![Screenshot 2020-02-17 at 2.06.34 PM.png](/assets/blog_resources/95583B0160E93A9E29A7D64001A45650.png)

Appendix 8: How does GloVE work; toy example

![Screenshot 2020-02-17 at 3.28.07 PM.png](/assets/blog_resources/1905C35C9F103F32EAEF181561BC2362.png)

Glove Distributions are really nicely spaced out!

![Screenshot 2020-02-17 at 3.29.49 PM.png](/assets/blog_resources/3F3EFE22D2CD25F937C78138D781EFC5.png)

Appendix 9:

**What is NLU?**

Bill McCartney - Works AI /ML Org at Apple, Previously at Google, led NLU for Google Assistant

Chris Potts - Stanford Linguistics Prof

**Goals of NLU**

Develop artificial assistants. A harder task than NLP.

Brief History of NLU - 1960: Pattern Matching with small rule-sets (SLDLU)

1990s- Statistical NLP.

Recently in academia, a resurgence of NLU

Systems are impressive, but far from solved- big breakthroughs lie in future.

SIRI (Appendix 1) - Still doesn’t understand everything. 

Banks and Automated Trading

**NLU: Traditional Organization**

Big themes of NLU: Semantic Parsing, Learning

Requires - Coreference resolution, domain knowledge, discource knowledge, world knowledge.

Lexical Semantics: Meanings of Words

Compositional Semantics: Meanings of Sentenecs

Language in Context: Meanings of dialogies and discources.

Semantic representaions

 - Sentiment analysis

 - Vector space models (vectors/topic distributions)

 - Relation extraction (Larry Page, founded, Google)

 - Semantic Parsing 

![Screenshot 2020-02-16 at 11.10.09 PM.png](/assets/blog_resources/0F57ED13527320DA653CD02502B923B9.png)

Course goals - to make you the best NLU resourcher and practitioner. To support you in complerleting a porject that is worthy of a presentation at a top NLP conference.

**Assignments:** Exemplify best practices for NLU projects.

**Appendix 1**:

How does SIRI work?

 ![Screenshot 2020-02-16 at 10.59.17 PM.png](/assets/blog_resources/68DCDC05692DD6620D0720AB4A655741.png)
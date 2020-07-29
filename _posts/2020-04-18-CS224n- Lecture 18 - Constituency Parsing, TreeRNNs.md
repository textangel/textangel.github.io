TreeRNNs are hard to scale and don't necessarily work better than LSTMs and Transformers. 

Part of the tricks of being a good deep learning researcher is managing to get thigns done and not wasting a ton of time. So train on a smal model firs tand make sure you can overfit on them. Plot your training and dev errors over model iterations.

The spectrum of language in CS.

**Semantic Interpretation of Language**

Beyond word vectors: 

How can we work out the meaning of larger phrases?

*The snowboarder* is leaping over a mogul.

*A person on a snowboard* jumps into the air.

In the above phrase, *the snowboarder* and *a person on a snowboard* refer to the same thing, but they’re made up of a different number of words (2 vs 5). How can we have a representation that works for variable word lengths? =\> Recursive TreeRNNs.

We need a way to compose linguistic knowledge from smaller parts. Chomsky argues that the singular innovation that allowed human beings to model langauge is that we could model recursion (putting together bigger things from smaller parts).

We want a method of computing a embedded representation of phrases of any length in a compositional manner.

The meaning (vector) of a sentence is determined by

 (1) the meanings of its words and 

 (2) the rules that combine them

![Screenshot 2020-02-16 at 7.29.15 PM.png](/assets/blog_resources/7844253BEA09ACCA98DC4F5F415194F2.png)

Given some parse of a sentence, learn a vector space representation through compositionality. Run a NN through pairs of words to determine whether to combine them and what the new combined representation should be.

![Screenshot 2020-02-16 at 7.34.19 PM.png](/assets/blog_resources/34676575FB512387CE4F028DA14B2097.png)

**Version 1: **

Their first attempt to do this just used a simple one layer NN (Concatenating the embeddings of the two words), and use a greedy parser that uses the NN generated confidences in max-order. (Note adter you do a combination of two words you have to run the new phrase with its adjacent words to get the nexst level confidence)

See Process of Generation in Appendix 1.

![Screenshot 2020-02-16 at 7.33.31 PM.png](/assets/blog_resources/26B52DE2B06A0277A28EDF52D1C3C7A0.png)

They scored the tree by summing the parsing decision scores at each node. They optimized it against a max-margin objective. 

![Screenshot 2020-02-16 at 7.39.54 PM.png](/assets/blog_resources/3A4E094C721A9E4E7D577F38039B4DBC.png)

**Issues**:

Because it’s a recursive RNN, the weight vector in the composition is the same for all compositions, in particular, for all syntactic categories. This might be a problem.

![Screenshot 2020-02-16 at 7.45.48 PM.png](/assets/blog_resources/CDEE2B47616C3E962B3A340A0CC71006.png)

**Version 2:**

A treeRNN with different compsotition (weight) matric for differnet syntactic environments. These syntactic enviornments are given by a (probabilistic) constituency parser.

![Screenshot 2020-02-16 at 7.49.51 PM.png](/assets/blog_resources/147CA9B75F1AA4500AD45BBE88CA7562.png)

This model can learn where the important semantics of sentences are. When words are merged, the resultant embedding is more similar to the more important word that was merged. 

**Version 3:**

**Compositionality Through Recursive Matrix-Vector Spaces**

Proposal: A new composition function where words that act as operators (“very”) are matrices (modifiers) rather than vector.

To be general, each word has both a vector and a matrix representation. 

To get the new vector meaning of a phrase: To combine two words, we take their matrices and operate on the other’s vector, and push thorugh a NN to get the new 

![Screenshot 2020-02-16 at 8.02.10 PM.png](/assets/blog_resources/2364664AF6CE0E1A78217588990160CF.png)

To get the new matrix meaning of a phrase (which might have been a bit too simple and not too good): Concatenate their two matrices and multiply by a third weight matrix.

![Screenshot 2020-02-16 at 8.03.49 PM.png](/assets/blog_resources/15E24E7A354853E698488A5BA338E9B4.png)

This was very hard to compute because the matrices required n^2 number of parameters. They kept the dimensionality very small (=25).

**Version 4: Recursive Neural Tensor Network**.

(RNTN)

Inspired work in putting vector embeddings in knowledge graphs! Also led to the creation of the Stanford Sentiment Treebank! This model was applied to the problem of sentiment analysis. ![Screenshot 2020-02-16 at 8.08.33 PM.png](/assets/blog_resources/6048A374B986C5F01720474ED7142196.png)

Idea: Still have the representaiton be vectors, but despite that have a meaning for a phrase composed in a way that allowed the two vectors to act upon each other. =\> By allowing both additive and mediated multiplicative interactions of vectors.

The combination of the vectors is performed by multiplying them against a 3D tensor whose depth is the dimension of the word vector (this is like performing d separate attentions) at the same time. 

![Screenshot 2020-02-16 at 8.21.31 PM.png](/assets/blog_resources/9548BDC617F7016E38329A0BD3FDCF08.png)

RNTN was pretty good at classifying sentiment!

![Screenshot 2020-02-16 at 8.22.56 PM.png](/assets/blog_resources/F00D047B9AFEEAFAF62F86DDC5FC6952.png)

GPUs work really well if there is a uniform computation you gotta do, whether RNNs (apply the same recurrence every time) or CNNs.

**TreeRNNs now:**

Tree to tree Neural Networks for Program Translation with Attention (2018) - Programming language translation using TreeRNNs, works better than RNNs!

**Stanford Center for Human Centered Artificial Intelligence (HAI)**

Develoing AI technolgoies inspired by human intelligence

Guiding and forecasting the human and societal impact of AI

Designing AI applications that augment human capabilities

**Appendix 1:**

Generating the recursive parse:

![Screenshot 2020-02-16 at 7.35.42 PM.png](/assets/blog_resources/28949B6C1D31C2E98E3579F4800DE09D.png)

![Screenshot 2020-02-16 at 7.36.29 PM.png](/assets/blog_resources/F257500ED604232BA9B1E9ECE65D50AD.png)

![Screenshot 2020-02-16 at 7.36.42 PM.png](/assets/blog_resources/7DC07CA3A1DB17EA89FFF0D61223BCCA.png)

**Appendix 2:**![Screenshot 2020-02-16 at 7.56.19 PM.png](/assets/blog_resources/FF60190BB3AEC1E552CC3FC15B551B61.png)

**
**
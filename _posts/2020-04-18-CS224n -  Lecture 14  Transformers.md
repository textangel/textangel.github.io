**Transformers**

Attention is all you need (2017), Aswani, Shazeer et al.

Idea: We want parallelization but RNNs are inherently sequential. Despite GRUs and LSTMs, RNNs still need attention mechaism to deal with long range dependencies - path length between states growth with sequence otherwise.

But if attention gives us access to any state, maybe we can just use attention and don’t need the RNN? ![Screenshot 2020-01-25 at 10.29.36 PM.png](/assets/blog_resources/0B27AB23A96A8B9806EED6AB86517242.png)

Recommended resource to learn about Transformers:

[The Annotated Transformer by Sasha Rush] (<http://nlp.seas.harvard.edu/2018/04/03/attention.html>)

A Jupyter Notebook using PyTorch that explains everything!

**Dot Product Attention**

****

Inputs: A query $q$ and a set of key-value (k-v) pairs to an output. Query, key, values and output are all vectors.
Ourput is weighted sum of values, where the weight of each value is computed by a dot product of query and corresopnding key, and looking up the weighting for each corresponding value. Queries and keys have the same dimensionality $d_k$ and value has dimensionality $d_v$.

$$A(q,K,V) = \sum_i \frac{e^{q \cdot k_i}}{\sum_j e^{q \cdot k_j}} v_i$$
In matrix form, this is
$$A(Q,K,V)=softmax(QK^T)V$$

![Screenshot 2020-01-25 at 10.43.26 PM.png](/assets/blog_resources/862EF8EADF52D2B852494B4614B44EB2.png)

The attention score is the weighted sum of the values weighted by the attention score, which squashes all the individula reprenstations into one. **Scaled Dot-Product Attention**

****

Problem: As $d\_k$ gets large, the variance of $q^Tk$ increases, so some values inside the softmax gets large, so the softmax gets incredibly peaked, and its graident gets smaller.
Solution: Scale by length of query/key vectors: (Pic in Appendix 4)
$$A(Q,K,V) = softmax \left(\frac{QK^T}{\sqrt{d_k}}\right)V$$****

**Self-Attention in the Encoder**

In the encoder, everything in the attention input is our word vectors: the queries, keys and values are all our word vectors. In other words, the word vectors themselves select each other. 

We’ll see in the decoder why we separate them in the definition.

**Multi-head Attention**

Problem with simple self-attention: Only one way for words to interact with one another. Maybe we wnat multiple types of attention, some to capture syntax, some semantics, some other dependencies.

Solution: Multi-head attention

First map Q, K, V into h=8 many lower dimensional spaces via W matrices. Then apply attention, then concatenate ourputs and pipe through linear layer. (Picture in Appendix 4)
\begin{align}
MultiHead(Q,K,V) = & \; Concat(head_1,...,head_h)W^O \\
\text{where } head_i = &\; Attention(QW_i^Q, KW_i^K, VW_i^V)
\end{align}
The multi-head attention was one of the successful innovations that made the transformer a successful architecture.

### Complete Transformer Block

![Screenshot 2020-01-25 at 10.59.18 PM.png](/assets/blog_resources/957EF8A0808FA3B4E3503A397BBBC7AD.png)

There’s no free lunch. You now no longer have recurrent informaiton carried along a sequence. You’ve got a word at some position which can be casting attention on other words, so if you like to have information carried along the chain, you ahve to walk the first step of the chain, and then you need to have another lauer that walks another step of the chain. So you’re getting rid of the recurrence but you’re substituting some depth to allow thigns to walk along multiple hops. But nevertheless, that is highly advantageous in GPU architectures which can compute everything at once.

**Positional Encoding**

Note that their actual word representaitons are byte-pair encodings.

The positional sinusoids are indices in sinusoids that are moving at different speeds, so that close by positions would have very similar values for the multiple sinusoids.

![Screenshot 2020-01-25 at 11.04.58 PM.png](/assets/blog_resources/99A4019C11A43D274D378387C592E2BA.png)

**Complete Transformer Encoder**

![Screenshot 2020-01-25 at 11.06.31 PM.png](/assets/blog_resources/81BFF110A05B430EF7DCFE19C2095D57.png)

Interpretation: At each stage you can look with multi-headed attention at various places in the sentence, accumulate information, push it up to the next layer, and if you do that 6 times, you can be starting to progressively push information along the sequence in either direction to calculate values that are of interest.

The attention heads attend to each other in interprable ways, and can correspond to modifiers of the word, close by words, or anaphora resolution. (Appendix 6)

**Transformer Decoder**

![Screenshot 2020-01-25 at 11.10.53 PM.png](/assets/blog_resources/3C7C65486B3D47F087FFD07F339BA6AF.png)

Each decoder state self-attends with all of its fellow decoder states as well as with all the encoder states. 

**Aswani’s Talk on Transformer**

Inspired PixelCNN.

RNN doesnt have an explicit way to model hierearchy which is very important in language.

Inspiration: Recursive self-attention: Convolutional Neural Networks

Long-distance dependencies require many layers

Attention - content based memory retrieval mechanism.

Compare your word embedding with all your words, and then refactor the embedding as a weighted combaination of all the words, and then add a feedforward layer to compute all the interaction features for you.

Self-attention:

* Get a constant ‘path-length’ between any two positions.
* You get gating/multiplicative interactions just by virtue of attention having a softmax at the end.

![Screenshot 2020-01-27 at 10.04.14 PM.png](/assets/blog_resources/4B17819DF9F2F131F0EB5887CFA0AD43.png)

On the decoder side, they mimic a language model using self-attention, by masking future input by masking the input in the softmax to -inf

**They went for simplicity and speed in the attention mechanism they used**

Linearly transform the position we want to re-encode into the **query. (1)**

And then linearly transform every position in the position as the **key**. The output is an attention distribution, which is a softmax over scaled-dot-product outputs. The attention distribution is used to weight the output **value, **which is simply also th eencoding of eevry position, to get our attention output, which is an interpolation over the original embeddings weighted by the attention distribution. 

The linear transform projects the original 512 dimensional representaiton into 64 dimensions. They did this because they used 8 attention heads instaed of 1, so the total number of FLOPs of computation remained 64\*8 = 512.

This can be dome really fast in a GPU with just 2 matmuls and a softmax. 

(1) This linear transform is equivalent to projecting them into a space where dot product is a good metric for similarity.

In Convolutions, because you had different linear transformations based on different word distances from each other, you can pick out effects for the word ‘who’ and the word ’to whom’ etc. Get subject and object and verb.

However, if you ahve a single attention layer, you average all the attention heads together so you lose the information of distinctness. So they have multiple attention heads. 

Transformer trained 3x faster than the others. 

For short sequences when dimension \> length, attention has a very favorable convolutional profile. 

![Screenshot 2020-01-27 at 10.12.12 PM.png](/assets/blog_resources/8F59561499AE6D6DA68D0D7CEE3CE0E8.png)

Tensor2Tensor

Sockeye (Amazon Transformer)

**The importance of residual connections:**

Residuals carry positional information to higher layers, among other information

![Screenshot 2020-01-27 at 10.18.46 PM.png](/assets/blog_resources/C8ADDA9E8E165030A63AD2C85795AEC3.png)

**Image Transformer**

**Probabilstic Image Generation: **Autoregressive image modeling (not GANs)

RNNs and CNNs are state of the art (PixelRNN, PixelCNN)

CNNs are informporating gating now matching RNN in quality

In images you often want symmetry, like synnetry in a face. likely increasinly importnat with increasing image size, but modeling long-range depencies with CNNs require wither

 - Many layers making training harder

 - Large Kernels at large parameter/computational cost.

Old Images about using similarity between image patches:

Texture Synthesis with Self-Similarity, Non-local Means

Attention is a ‘soft’ local similarity.

Attention is cheap ONLY if length \<\< dim! SO attention is expensive for images

![Screenshot 2020-01-27 at 10.24.35 PM.png](/assets/blog_resources/CAACF48213369AE256320E6FCB890FA1.png)

But you can combine locality with self-attention.

Restruct the attention windows to be lcoal neighborhoods. Good assumption for images because of spacial locality. 

**We had two differnt types of rasterzations for the ImageTransformer**

We had a 1d rasterization attending to a larger memory block in this rasterized fashion.

![Screenshot 2020-01-27 at 10.26.06 PM.png](/assets/blog_resources/5F8DC354FAEBF5D42FE584CACE6F19C3.png)![Screenshot 2020-01-27 at 10.27.02 PM.png](/assets/blog_resources/646EA194D2E7860935AA8AA066D5D56E.png)

Tasks: Super Resoultion and Unconditional / COnditional Image Generation.

The factorizastion of your prbabilstic generative model just depends on how you rasterize. 

For 1D rasterization we went first rows then columns, in 2d rasterization we went for each block and inside of each block we rasterized. 

Not at GAN level. They can get structured objects really well

On Super Resolution - there’s a lock of conditioning information, because there’s a lot of conditioning. 

Anotehr test we did was image genration. The question is, do you get diversity? And does the image understand structure about the world?

So we did image completion - becasue you lock down a lot of probability so its very easy to sample, and see if the image generates with a bit of diversity

![Screenshot 2020-01-27 at 10.34.34 PM.png](/assets/blog_resources/290B799CECE4955611005C3D01B68340.png)

**MusicTransformer**

![Screenshot 2020-01-27 at 10.37.51 PM.png](/assets/blog_resources/4D2AB770C78C7175461CD4BBF219ABF0.png)

![Screenshot 2020-01-27 at 10.45.24 PM.png](/assets/blog_resources/6A946E2A0253C0A242F2B5729E6400EA.png)

**Relative Attention (Difference Between Music Transformer and Ordinary Transformer)**

You can think of convolution as a fixed filters moving around that captures the relative distance (one-before, two-before…) These are kind of like a rigid structure that allows you to bring in the distance information very explicitly. 

You can imagine relative attention with multiple-heads to be some combination of these. On one hand you can access all of the past history (not just a convolutional kernel window) very directly, on the other hand you also know how you would relate to this history, capturing, for exampe, translational invariants. 

We think one of the reasons in one the the above exaxmples that music transformer was able to generate beyond the length it was trained on in a very coherent way is that it was able to rely on these translational invarants to carry the relational information forward.

![Screenshot 2020-01-27 at 10.52.29 PM.png](/assets/blog_resources/E68FEDB2E08F69EE0F53058B452E0373.png)

MusicTransformer’s addition over the Transformer was adding a positional emnedding to the attention - so things two timesteps away or three time steps away get treated differnetly and if there’s any 

The extra positional embedding is relative to the distance.

In machine translation in NLP, relative positions have been shown to help the translation.

![Screenshot 2020-01-28 at 10.32.05 PM.png](/assets/blog_resources/99CAF57C11A5C3339D9E4E74FDF1788F.png)![Screenshot 2020-01-27 at 10.57.50 PM.png](/assets/blog_resources/1842F3D0CB3E5B3457183B02F5D88F44.png)

Previously, relative attention was calculated for every pair x distance triple, but this was very expensive: For every pair of tokens, you have their dot-product attention, and then you also look up a distance embedding for the distance between the pair which is applied to every pair. So you have a 3d tensor for the whole token-pair-distance embeddings.

![Screenshot 2020-01-28 at 10.34.44 PM.png](/assets/blog_resources/E981FCE563E67C45C1457CBE613F63D7.png)![Screenshot 2020-01-28 at 10.37.17 PM.png](/assets/blog_resources/2CF367D2D3C3C759BD3261CB079B965F.png)![Screenshot 2020-01-28 at 10.37.53 PM.png](/assets/blog_resources/354B0C9F16562CC7EFADC6D114B8AC7E.png)

Colvolutions and Relative Attention has Translational Equivariance - a picture of a dog produces the same comvolutions no matter where in the image it is. 

![Screenshot 2020-01-28 at 10.40.50 PM.png](/assets/blog_resources/125E7F13433C5E67D6D44799833262F6.png)

Graph Library

![Screenshot 2020-01-28 at 10.44.20 PM.png](/assets/blog_resources/16926C0941431C288C80F821D6FB3945.png)
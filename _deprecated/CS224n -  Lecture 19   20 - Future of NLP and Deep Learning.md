---
category: cs224n
---

**Lecture 20 - Future of NLP and Deep Learning**

Kevin Clarke- Areas recently taking off

**
**

**Harnessing Unlabeled Data - ****Backtranslation and unsupervised machine translation**

Monolingual text is easier to acquire! Use a large corpus of unlabeled data and use it to help our billingual translation model.

**Pre-training**: 1\. Separately train encoder and decoder as langauge models 2\. Then train jointly on billingual data. Super effective.

Problem with pre-retraining: no “interaction” ebtween the two sentences, 

**Self-Training**: label unlabeled data to get noisy training examples. (Not used very common, its a bad idea, but it inspires back-translation).

![Screenshot 2020-02-16 at 9.13.14 PM.png](resources/C9F49EDD3CDD2C546032B62927C39246.png)

**Back-Translation**

![Screenshot 2020-02-16 at 9.14.32 PM.png](resources/433DC17F3D21BC9BCE249C2D1D628524.png)

 Back-translation helps *a lot*.![Screenshot 2020-02-16 at 9.16.59 PM.png](resources/581EA023BCF29A6638EDB40B0FD11CCC.png)

**What if there is no billingual data?**

**Word to word unsupervised translation**

Unsupervised Word Translation using cross-lingual word embeddings - shard embeddings space for both languages. Keep word vectors close to their translations from monolingual corpora.

FIrst run word2vec on monolingual corpora, getting word embeddings X and Y.

Learn an orthogonal (rotation) metric such that WX ~ Y.

Learn W with adversarial training: 

Dsicriminator: Predict if an embedding is from Y or it is a transformed embedding Wx originally from X. Train W so the discriminator gets “confused”.

![Screenshot 2020-02-16 at 9.21.17 PM.png](resources/1B3A7B61C0B35DC0BF0BABE7CFA17636.png)

**Sentence to sentence unsupervised translation**

Model: same encoder-decoder model (without attention) used for both languages

 - Initialize with cross-lingual word embedding, such that the input and ourput can be English or French. Feed in a special token \<Fr\> that tells the model to generate in French (or english, or whatever)

![Screenshot 2020-02-16 at 9.23.32 PM.png](resources/CF75DC2A3356386EE78EC8046A0CEF50.png)

Now, this model can be used both as a translation model as well as an autoencoder, because the input can be English or French.

**
**

Train it on two objectives:

Training Objective 1: De-noising auto encoder

 - Scramble up the words a little bit and ask the model to reproduce the original sentence before it was scrambled.

 - This ensures that the model can contain all the informaiton about the sentence in the final embedding from the vector produced by the encoder. 

Training Objective 2: Back translation

First translate fr -\> en

Then use as a “supervised example" to train “en -\> fr"

 - Why does this work? Cross lingual embeddings and shared encoder gives model a starting point.

![Screenshot 2020-02-16 at 9.30.12 PM.png](resources/CB7EDDBBBE3999B7968D941010DEAC2E.png)

**Results**

![Screenshot 2020-02-16 at 9.30.45 PM.png](resources/B0AC220EDAB42B518DEE69E7B5526A7A.png)

**
**

**Notes: Attribute transfer: **

You can use the same appraoch above to do attribute transfer, where the attributes now are not English/French but for example, relaxed, annoyed. Data from Twitter.

![Screenshot 2020-02-16 at 9.31.37 PM.png](resources/195C7EEE076FD30C547B1C72B25C452E.png)

**Notes: **Doesn’t work so well in languages with very different grammars

**
**

**Notes: Cross-Lingual BERT**

Google concatenated a whole bunch of corpora in differnet langauges and then trained one BERT language model using BERT’s maskLM method.

Facebook porposed to combine this MaskedLM objective with translation

![Screenshot 2020-02-16 at 9.35.53 PM.png](resources/4412781F2D84A58130722EE9DA6BA1EB.png)

Leads to very very substantial improvements in unsupervised machine translation.

They took cross-lingual BERT and take it as input to an unsupervised machine translation system, and got the 

current SOTA in unsupervised machine translation.

![Screenshot 2020-02-16 at 9.36.47 PM.png](resources/1241912AEDB232F814035ECBFDA0C745.png)

**Huge Models: GPT-2, ****Scaled up pre-training**

![Screenshot 2020-02-16 at 9.39.06 PM.png](resources/A21D13553241DC812C8926FBC7E8F261.png)

![Screenshot 2020-02-16 at 9.39.33 PM.png](resources/B2AC704D1B71BB98A6664ECD104BA705.png)

StyleGAN is in-between the size of EMLO and BERT

![Screenshot 2020-02-16 at 9.40.01 PM.png](resources/9219B13816DB99E0CEC5C360BE0E1B80.png)

![Screenshot 2020-02-16 at 9.40.35 PM.png](resources/9CB2949106F720B961CC29AB83B4D7A5.png)

![Screenshot 2020-02-16 at 9.41.01 PM.png](resources/8AD4BE8A23C3D1BE2E91F82C74364EB1.png)

Data Parallelism - Each GPU has a copy of the model, split the minibatch across these models. So if you have 16 GPUs, each sees a batch size of 32, you can aggregate the gradeints of these 16 GPUs and you effectively get a batch size of 512 - allows you to train models much faster.

Model Parallelism - Models get so big they can’t fit on a single GPU. In this case, you ahve to split up the model across multiple compute units, that’s what’s done for models the size of GPT-2\. There are new frameworks such as Mesh-Tensorflow which are basically designed to make this model parallelism easier.

**GPT2**

Just a really big Transformer ML, trained on 40GB of text (10x larger than what previous langauge models has been trained on), quite a lot of effort going into making sure the dataset is good quality; take webpages form reddit links with high karma.

![Screenshot 2020-02-16 at 9.46.12 PM.png](resources/70DED24BD108DEC54B03C40AD80F2F72.png)

![Screenshot 2020-02-16 at 9.46.53 PM.png](resources/38496D748261D447A426A13F5E0F75BF.png)

![Screenshot 2020-02-16 at 9.47.57 PM.png](resources/86910073E019E9CDD14209F3018256C4.png)

X-axis is log(model size), Y-axis is accuracy. Dotted lines are existing works on these tasks. (How could it be doing translation?- Appendix 1)

GPT2 is much worse at Question answering - it only gets 4% accuracy. However, look at GPT2 QA results, it seems like GPT2 has learned some world knowledge about the world.

![Screenshot 2020-02-16 at 9.51.30 PM.png](resources/CC8449BA6F8B21FAD591A3AB6EF1410E.png)

![Screenshot 2020-02-16 at 9.52.49 PM.png](resources/213EDEBF06B831D21A737428297166A8.png)Maybe 1 Trillion parameter models will be possible in ~10 years.

Social Ramifications of GPT2 (Appendix 2).

**What’s next for NLP?**

**A lot of the current research “trends” are reactions to BERT. What did BERT “solve" and what do we work on next?**

![Screenshot 2020-02-16 at 10.02.51 PM.png](resources/42FB41C7CDCBC2F0437AA8C606C0F0CC.png)

![Screenshot 2020-02-16 at 10.03.53 PM.png](resources/249907EC80C657CAD9EA79D64A96C15A.png)

![Screenshot 2020-02-16 at 10.04.14 PM.png](resources/80EFC9B8CFDFC5BE665F4657ECEC3588.png)

Architecture engineering isn’t providing gains for time improvement!

We need harder NLU tasks. BERT has almost solved SQuAD

![Screenshot 2020-02-16 at 10.05.01 PM.png](resources/BD31E411AE07C3669C0D975365808A5C.png) 

**New Datasets:**

![Screenshot 2020-02-16 at 10.06.36 PM.png](resources/4AC697A36BDCECBF55FD9A7034842159.png)**
**

![Screenshot 2020-02-16 at 10.07.34 PM.png](resources/DD1A7154260AEFC71354D6F9C558BA5A.png)

![Screenshot 2020-02-16 at 10.07.49 PM.png](resources/7B54F790EFF2C6D987F93FB49E7C3A85.png)

**New Areas of NLP**

**Multi-Task Learning - **Another fronteir of NLP is getting one model to perform many tasks. GLUE and DecaNLP are recent examples

Multi-task learning yields improvements on top of BERT.

![Screenshot 2020-02-16 at 10.09.40 PM.png](resources/5C932ED9E7154ED4C71A9D6BAD55750B.png)

BERT is a very large model, a better way to use that model is to get it to do many things at once.

**Low-Resource Settings**

Models that don’t require lots of compute power (can’t use BERT) - especially for mobile devices.

Low-resource langauges

Low-data settings (few-shot learning). 

**Interpreting/Understanding Models**

Can we get explainations for model predictions?

Can we understand what models like BERT know and why they work so well?

Rapidly growing area in NLP, very important for some applications (e.g. healthcare).

Usually tested using Diagnostic/Probing Classifiers - take a model like BERT and run it through several tasks, each of which test for one particular thing, like “does this model know about syntax, or POS”?

This is done by training another classifier on top of BERT to do, say, POS tagging, but we only backprop into the diagnostic classifier itself (we are treating the output of BERT as a fixed input).

![Screenshot 2020-02-16 at 10.13.23 PM.png](resources/1570F0ACF0D16E4B4D036BFA5FC9B296.png)

![Screenshot 2020-02-16 at 10.15.09 PM.png](resources/CC32D41D14DA6B07CDAA218F2FD349FD.png)

The different layers of BERT seem to be representing pretty well the differnet layers of lingustic structure. For example, dependency parsing is a medium-level tasks, and it seems like the medium layers of BERT help most with dependency parsing. 

![Screenshot 2020-02-16 at 10.15.46 PM.png](resources/BFA6157CE5D7143E750F8D44888B67EE.png)

**NLP in Industry**

NLP is rapidly growing in industry:

 - Dialogue (Chatbots, customer service)

 - Health Care: Understanding health records, understanding biomedical literature.

**
**

**Lecture 19: Bias in AI**
Bias in AI - Margaret Mitchell GoogleAI

Prototype - there is “exemplar’ features of objects, i.e. “yellow” bananas.

Human reporting bias - huamns report things not in acordance with real world frequencies

![Screenshot 2020-02-16 at 8.36.44 PM.png](resources/2C26B1794AB821DEF1B347ECF1AE2540.png)

![Screenshot 2020-02-16 at 8.36.57 PM.png](resources/1A38D01024F6FCC5FE465A81D481D1AC.png)

**Appendices**

Appendix 1:

How can GPT-2 do EN-Fr Translation when it was trained on a giant corpus of English data?

Because in some parts of the English corpus there were bits of French, and quite amazingly it picked up bits and pieces of how to translate into french even though that wasn’t the inded object of the training.

![Screenshot 2020-02-16 at 9.49.54 PM.png](resources/363198A6A04E3CC1CCA800BB6FE341F1.png)

Appendix 2:

Debate about should GPT-2 Release their model

![Screenshot 2020-02-16 at 9.54.49 PM.png](resources/6856877A000EA721CB66C3AD2A3CE942.png)

Appendix 3: NLP in high stakes decision making

 - Judicial decisions, hiring, grading tests.




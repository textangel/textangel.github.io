---
category: cs224n
---

**Lingusistics Aside**

In Appendix (1). Not really important but simply because it is cool.**
**

**
**

**Purely Character Level NMT Models Aside**

In Appendix (2). There’s been a bunch of work on this but it’s not the most promising. This is more for intellectual interest. Most people in practice use subword models. 

**
**

**Sub-Word Models**

**Byte-Pair Encoding **

Neural Machine Translation of Rate Words and Subword units- Sennrich, Haddow, Birch ACL 2016a

In NLP BPE is a word segmentation algorithm done though botton up clustering. Iterate the following unitl you reach a target vocabulary size: (Appendix 3)

1. Start with a unigram vocabulary of all unicode characters in data.
2. Most frequent ngram pairs -\> a new ngram which is added to our vocabulary (see below slides)

Then, for each word in the text, do deterministic longest piece segmentation of words and that is our new vocabulary and our new sequence (Note that segmentaiton is only done within words identified by some tokenizer for NLP, usually Moses Tokenizer.) 

If we keep running BPE, we get a vocabulary of common clumps of English letters (most commonly important morphlogical tokens, short words, etc).

The segmented words form the new primitives that you input to whatever model you are using.

![Screenshot 2020-01-25 at 1.34.39 PM.png](resources/B0CD8A7BC6EB2D95F97AD7F337D0006A.png)

![Screenshot 2020-01-25 at 1.35.00 PM.png](resources/CF672757D8D7A9256690825C3857E583.png)

BPE is an example of a **word piece model**, where we** **use the exact same architecture as the word model, but the primitive words are actually pieces of words.

**Google Wordpiece/Sentencepiece model**

Google NMT uses a variant of BPE, which was V1 a wordpiece model and V2 a sentencepiece model.

Rather than add char n-gram based on count, they use a greedy approximation to maximizing language model log likelihood to choose the pieces. The next n-gram to add is the next n-gram that maximally reduces perplexity. 

Wordiece model v1 tokenized within words

Sentencepiece model v2 works from raw text, because Google didn’t want to use a separate tokenizer for each language.

 - Whitespace is retained as a special token (\_) and grouped normally

 - You can get the original sequence out by re joining pieces and recoding them to spaces.

BERT uses a variant of the wordpiece model

- (Relatively) common words are in the vocabulary (at, fairfax, 1910s)

- Other words are built from wordpieces

 - hypatia = h \#\#yp \#\#ati \#\#a

- If you’re using BERT in an otherwise word-based model, you have to deal with this

**Character-Level to Build Word-Level Embeddings**

Learning Character Level Representations for POS Tagging (Dos Santos and Zadrozny 2014, early)

Convolution over characters to generate word embeddings. The task they used the word embeeddings for was that they used a fixed window of word embeddings for PoS tagging.

Character-based LSTM to build word representations

Ling Luis, Marujo, et al Finding Function in Form: Compositional Character Models 2015, early.

Build word embeddings from characters but from final states of bi-LSTMs (slow)

**Character-Aware Neural Langauge Models**

Kim, Jernite, Sontag, Rush 2015, early.

They do convolutions over character embeddings, and do max-pooling-over-time over the outputs of the character embeddings, which what it effectively does is it chooses what n-grams best represent the meaning of the word.

They then feed that through a highway network, and the output of that at a word level goes into an LSTM network which they use for a langauge model. 

![Screenshot 2020-01-25 at 1.54.56 PM.png](resources/FEC0D8D7150E1A14B87D48F0EE854060.png)

This one has a lot of slides so I will put less important middle slides in appendix (4).

Their char-level languague model works just as well as a word-level LSTM model, but with way less parameters (becuase vocab size is smaller so embedding space can be smaller). The space complixity is never so big of a deal. What is a big deal is that their learned embeddings are somewhat better than original word-level word embeddings and they are able to deal with OOV words quite well.

![Screenshot 2020-01-25 at 1.58.00 PM.png](resources/612FA0CB50F013574AB61DE7FB8858DA.png)

Interesting insights in to the effects of a character based model. The below table shows three models, and the list is the words that are most similar to the given word.

* The LSTM word model puts together somewhat similar words based on their appearences in the text.
* The char model before the highway network, if we just did the CNN and the maxpool, only remembers thigns about characters, and outputs the words that are spelled similarly to the target word.
* But after the highway network, the highway layers are successfully transforming those character level sequences into something that does have semantic meaning. So then the most similar words are words that are semantically similar to the original word, preserving more character-based features. So words that aren’t in the vocab of the model can be captured as well (second-slide)

![Screenshot 2020-01-25 at 1.59.56 PM.png](resources/A80AE48815ED68BD8831D2F27E1708F9.png)

![Screenshot 2020-01-25 at 2.10.24 PM.png](resources/3D0D480EC00EF9BDB6D4585292366A0F.png)**
**

The last column can only be done well by the character level model.

Take-aways:

* CNNs + highway network over characters can extract rich semantic and structural information
* Key thoguht: You can compose ‘building blocks’ to obtain nuanced and powerful models!

**
**

**Hybrid architectures**

A best of both worlds architecture:

* Translate mostly at the word level
* Only go to the character level when needed

**Achieving Open Vocabulary Neural Machine Translation with Hybrid Word-Character Models **(Luong and Manning ACL2016) - Pretty successful (+2 BLEU points)

At training time they decide on a vocabulary of 16000 words, and everything not in that vocabulary gets mapped to \<unk\>. 

Run a standard seq2seq lstm (they actually used a 4 layer one for the encoder and 4 layers for the decoder) with attention. 

But for all words that aren’t in the vocabulary, train a character level LSTM with the letters of the unknown word as input and the last hidden state is given to be the hidden state in the word-level representation. (I.e. Work out a char level representaiton of unkonw words using a character level LSTM)

Also, when we decode, we have a softmax with vocabulary of 16000, and if it generates \<unk\>, we take the representation of \<unk\> and feed it as the initial input to a decoder character level LSTM and then we have the character level LSTM generate a character seqeunce until it generates the STOP sequence.

(So there are 3 models: the encoder \<unk\> LSTM, the seq2seq encoder-decoder at the word level, and the decoder \<unk\> LSTM)

We add the loss of the word level model and both character level models during training.

The decoder uses word-level beam search and also a character level beam search for \<unk\>.

![Screenshot 2020-01-25 at 2.28.38 PM.png](resources/C09ECEFA9759E09FD8A698FB228383D8.png)

Hybrid NMT (Luong and Manning 2016) in 2016 for 20.7 which was 2.5 BLEU points better than the SOTA at the time. 

One disadvantage, is that the hybrid model doesnt have any representation. The purely character level model is able to use the character level sequence as the conditioning context very effectily, whereas in the hybrid model, although they feel the hidden state of the word level model as the starting hidden state of the character model , it doesn’t have any representation further back than that then what’s in teh word model, so it doesnt do as good of a job of capturing context that allows it to do translation with things like names. (Appendix 6)

**Chars for Word Embeddings**

A Joint Model for Word Embedding and Word Morphology (Cao and Rei 2016)

* Same objective as word2vec, but using characters
* bi-directional LSTM to compute embedding
* Model attempts to capure morphology
* Model can infer roots of words.

**FastText Embeddings**

Enriching Word Vectors with Subword Information

Bojanowski, Grave, Joulin, Mikolov. FAIR 2016

Aim: A next-generation efficient word2vec-like word representation library, but better for rare words and langauges with lots of morphology.

An extension of the w2v skip-gram model with character n-grams.

Represent word as char n-grams augmented with boundary sumbols and as whole word, so 

where = \<wh, the, her, ere, re\>, \<where\>. 

 - Note that \<her\> or \<her is different from ‘her’. Prefix, suffixed and whole words are special

So ‘where’ is represented by the above 6 tokens. Recall that in word2vec, you have a center word representation and a context word representation and you learn these representations based on how often center words appear by context words. In this version, they train the system for center vectors corresopnding to all 6 of these representations, and then when they obtain final representations, sum the results. 

So their word in context score is:

$s(w,c) = \sum_{r \in G(w)} z^T_gv_c$

s

 - Detail: (In actuality instead of sharing representaiton for all n-grams, they use a hashing trick to have a fixed number of vectors)

That word embedding actually workds pretty successfully. For langauges with more morphology, they are getting better metrics than the old Word2Vec. So FastText is the best word embeddings to use these days.

**Practical Takeaways**

- Use BPE (when possible)

- FastText word embeddings used to be the best in 2017 but they have been replaced by comtextual word vectors (ala BERT). Everyone shuold be using them, not traditional word vectors.

**Appendix**

(1)

**Lingustics Aside**

**Phonetics and Phonology **

Phonetics is the sound stream. Phonology posits a small set of distinctive units: **phonemes** or distinctive features. A perhaps universal typology bit language particular realization. The basic building blocks for langauge are sounds, and the way we write the language (in Eng) roughly corrsponds to the sounds.

[In reality the space of all sounds a human can make is continuous, but humans percieve them as residing in discrete categories, which is language-specific. Thus cognitive science people speak of **categorical perception. **Differences within a category are percieved as shrunk and differences between categories are enlarged). 

**Morphology: Parts of words**

Sounds are the basic building blocks but sounds carry no meaning on their own.

Traditionally, we have **morphemes** as the smallest semantic unit. Example: [[un [[fortun(e)]ROOT ate]STEM] ly]WORD

Morphology is parts of words and this is the minimal level that have meaning.
Deep learning: Morphology is little studied, one attempt with RNNs is (Luong, Socher and Manning 2013). But not taken on widely, because parsing out the semantic meaning of words by hand is rather hard.

You can get the same results using character level n-grams!

**Words in writing systems**

Writing systems vary in how they represent words, or don’t

No word segmentation (no spaces, e.g. in Chinese).

Clitics? 

* (I (to you, past tense) in french is `Je vous ai`, but this functions like one word)
* In arabic (“so+said+we+it” is joined together into one word)

Compound nouns? 

* In Eng it’s separated- “life insurance company employee"
* (Like massive german compunds)

**Models below the word level**

Need to handle large, open vocabulary

* Rich, dynamic vocabulary
* Transliteration
* Informal Spelling

**Character Level Models**

1\. Word embeddings can be composed from character embeddings

* Generates embeddings for unknown words
* Similar spellings share similar embeddings
* Solves OOV problem

2\. Connected langauge can be processed as characters

Both methods have proved to work very successfuly! Somewhat surprisingly, as traditionally, phonemes/letters weren’t a semantic unit, but DL models compose groups.

Note that human langauge systems are different!

- Phonemic

- Fossilized phonemic (English)

- Syllabic

- Ideographic

- Mixed (Japanese)

![Screenshot 2020-01-25 at 12.46.56 PM.png](resources/74E0879217C522299B67A3E015E520F1.png)

![Screenshot 2020-01-25 at 12.54.00 PM.png](resources/CC397D996A4B8434311666EB37B0BCC8.png)

**
**

**(2) **

**Purely Character Level NMT Models**

**Character level decoder only **(Chung, Cho, Bengio A character-level decoder without explicit segmentation for neural machine translation. 2016)

**English-Czech WNT2015 (Luong and Manning 2015)**

 - Czech has big horibble words with complicated morphology, so a character model is fitting.

 - It was a basic old 2015 seq2seq with attention with some handling for UNK. It worked well against the word-level baseline, but it was SLOOW - 3 weeks to train, not that fast at runtime. Using LSTM on character based models means that your sequences get very long (7x longer) and you are backpropagating through time a lot further back, so in this one they were backpropping over 600 timesteps back.

**Fully Character-Level Neural Machine Translation without Explicit Segmentation**

Jason Lee, Kyunghyun Cho, Thomas Hoffman 2017\. Encoder as below, decoder is a character level GRU. Note that what happened in their results is that using the character level source side GRU decoder gave them a lot of value, while using this complicated character level encoder described below gave them little to no value at all.

Encoder

* On the encoder side you started off with a letter sequence of character embeddings.
* When using convolutions of 4,3, and 5 characters, you get subword representations.
* And then do max-pooling with stride 5 to get maxpool representations for pieces of the text for each of the 3,4, and 5 convolutions.
* Then feed it through multiple layers of highway network To get sentence chunk embeddings
* And then feed that through a single layer bidirectional GRU to get the source sentence embedding.

![Screenshot 2020-01-25 at 1.15.55 PM.png](resources/A39FA7CD6E99EBBA0B9C30DDD703118C.png)

Revising Character Based Neural Machine Translation Translatin with Capacity and Compression.

Cherry, Foster, Bapna, Firat, Machery GoogleAI (2018), in Appendix (3)

For smaller (shallower) models you are better off with words, and for deeper (\>4 layers) on both En-Fr and En-Cze Char level models work better.

 Eng to Czech character level model is very good idea. For Eng-Fr the character level model does very little over the word level model.

The time it takes to train is much much slower for the character level models.

**
**

**(3)**

![Screenshot 2020-01-25 at 1.22.20 PM.png](resources/549A1E3C8A1C4E399E5B6A0384F1832D.png)**
**

(4) BPE was originally a compression algorithm that acted on bytes

as follows:

 Take the most byte pair -\> add a new byte to my new dictionary of possible values.

Some people (not many, this is not recommended) have actually worked with BPE with literal bytes underlying unicode text, to get around the fact that utf8 has ~20,000 characters which is a huge vocabulary to work with.

(5) Other sides from **Character-Aware Neural Langauge Models, **Kim, Jernite, Sontag, Rush 2015.

![Screenshot 2020-01-25 at 1.56.54 PM.png](resources/CAB49960E6709C177829F25D4A530852.png)

![Screenshot 2020-01-25 at 1.57.22 PM.png](resources/DD34896A33E68C377DFF9A2EED3A85A2.png)

(6)

![Screenshot 2020-01-25 at 2.39.40 PM.png](resources/F683F36BC04BE606B6B53D37C38720DB.png)

The basic word based model was translating \<unk\> as po=after because when it sees an \<unk\> it will look at its attention head and get the source word that has the most attention from the \<unk\>, and either copy it or translate it as a unigram. Unfortunately, the \<unk\> was showing the most attention on ‘after’ rather than ‘diagnosis’ so what got translated is ‘after after’ and the word diagnosis is just lost. The hybrid model works beautifully. 

![Screenshot 2020-01-25 at 2.40.07 PM.png](resources/AD070BDF6A205FCC7C9703E34976C6D7.png)

One disadvantage, is that while the purely character level model is able to use the character level sequence as the conditioning context very effectily, in the hybrid model, although they feel the hidden state of the word level model as the starting hidden state of the character model, it doesn’t have any representation further back than that then what’s in they word model, so it doesnt do as good of a job of capturing context that allows it to do translation with things like names. The embedding for Shani was fed into the character level model, which had some sense that it was a name but not much else context, and unforuntaley generated G instead of S at the first step, resulting in a snowball that results in Graham.
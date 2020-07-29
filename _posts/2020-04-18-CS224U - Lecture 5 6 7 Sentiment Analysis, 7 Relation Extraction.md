**Supervised Sentiment Analysis**

Sentiment as a deep and important NLU problem

General practical tips for sentiment analysis

![Screenshot 2020-02-18 at 8.25.59 AM.png](/assets/blog_resources/2BDC8A54AA9253270BF7F36924AB135B.png)

![Screenshot 2020-02-18 at 8.27.28 AM.png](/assets/blog_resources/588018C8B4345FDC8BA5F945765FF2F7.png)

nltk.tokenize.casual.TweetTokenizer (tokenizes tweets)

Don’t do stemming when you’re doing sentiment analysis. 

Do tree-based NEG marking for phrases after negative words.

![Screenshot 2020-02-18 at 9.13.56 AM.png](/assets/blog_resources/9BB6ADEA576B57640EABEDC942656608.png)

**Stanford Sentiment Treebank**

Contains data on full sentiment trees

![Screenshot 2020-02-18 at 8.40.27 AM.png](/assets/blog_resources/C7255A1D71256F577CFDD377C2E2539C.png)

Appendix (1) Ideas for hand-built features for setntiment problems

As nltk.tree.Tree objects

Generally you want as output score macro avg f1 scores

sklearn.feature\_extraction.DictVectorizer maps list of dicts to matrices

**Comparison of Classifiers**

If you are comparing a model M and a baseline nodel B, and want to see if B consistently beats M:

 - Run a Wilcoxon Signed-Rank test (scipy.stats.wilcoxon) for situations in which you can afford to repeatedly assess B and M on different train/test splits.

 - For situations where you can’t repeatedly assess B and M, McNemar’s test is a reasonable alternative, and it operates on the two models’ confusion matrices.

**Appendix (1)**

Ideas for hand-built features for setntiment problems

![Screenshot 2020-02-18 at 9.15.15 AM.png](/assets/blog_resources/8681C4AA8F62E664EEDDD8C3DAF0A5FB.png)

**Relation Extraction**

Retrofitting on WordNet + LSA get high scores on bakeout

**Relation Extraction using Distant Supervision**

In Relation Extraction, the output is a relational triple.

Relation Extraction - extracting relations (for example the following) from text

(founders, SpaceX, Elon Musk)
(has_spouse, Elon Musk, Falulah_Riley)
(worked_at, Elon_Musk, Tesla_Motors)

If we extract enough realtions from text, we can build a large knowledge graph.

Freebase -\> Knowledge Base became foundation of Google Knowledge Graph

Micosoft Satori Knowledge Graph

Apple Lattice Knowledge Graph

**Supervised Learning**

Requires annotated data

**Distant Supervision**

We will sidestep the need to hand-annotate data.

We will use existing relation triples from a knowledge base.

Suppose we already have the triple (founder, SpaceX, Elon\_Musk). We search for a large corpus for any sentence in which ‘SpaceX’ and “Elon Musk” appear, making the (unreliable!) assumption that app sentences express the founder relation, and then use them as training data for a learned model to identify new instances of the founder relation.

This is a powerful idea with two limitations

1\. We require an existing knowledge base with relations we care about.

2\. It is not true that every sentence with ‘spaceX’ and ‘Elon Musk’ has the ‘founder’ relation. 

 - i.e. the data is noisy. However, this disadvantage is vastly outweighed by the increased data size.

—

Relation extraction works best with corpus with sentences with at least two entities, which are already tagged with its unique identifier (found through a NER parser). This solves the problems of polysemy (“New York” -\> “New York State”; “New York” -\> “New York City”)

synonym (“Big Apple”, “New York” -\> “New York City”), 

**Databases**

1\. Wikilinks

The corpus we'll use for this project is derived from the [Wikilinks dataset](https://code.google.com/archive/p/wiki-links/) [announced by Google in 2013](https://research.googleblog.com/2013/03/learning-from-big-data-40-million.html). This dataset contains over 40M mentions of 3M distinct entities spanning 10M webpages. It provides entity resolutions by mapping each entity mention to a Wikipedia URL.

Note that the Wikilinks dataset does not have the relation. So it has (Elon\_Musk, Tesla\_Motors) but not the relation between them - you have to figure that out from actual words.

2\. Freebase and derivatives

**The Knowledge Base (extention of Freebase, deprecated)**

In the form

(founder, SpaceX, Elon\_Musk)

Freebase has billions of triples

**Problem Formulation**

What is the input to the prediction? A pair of entity metions, or a pair of entities apart from specific mentions.

Output: A single relation or multiple relations?

1\. We will generally all pair of entities, take all sentences where (Elon\_Musk, Tesla\_Motors) co-occur, and use those sentences to generate a feature representation for the relation between Elon\_Musk and Tesla\_Motors. 

Go through the triples in knowledge base and see how many relations per triple is in database.

2\. Sample negative examples that occur together in a sentence but don’t occur with any relation which is in teh knowledge base.

3\. Use multi-label classification because a pair of entities may have more than one relation between them. Use the **binary relevance method** to do so - factor your prediction problem into separate classification problems for each relation.

F score is a harmonic weighting between precision and recall - weights F1 fives equal weight to precision and recall, but F0.5 gives more weight for precision.
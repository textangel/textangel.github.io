**Semantic Parsing**

Semantic parsing is the translation of natural language into a formal meaning representation on which a machine can act.How do we parse statements into a formal theorem? 

Ie, turn natural language into

 - SQL queries

 - a custom procedural language for robot control

**How SIRI queries work**

![Screenshot 2020-02-20 at 3.01.19 PM.png](/assets/blog_resources/F60B56271CA7F0A331034115BD2D7B1F.png)

**Approach to Semantic Parsing**

![Screenshot 2020-02-20 at 3.03.02 PM.png](/assets/blog_resources/2EE1B4F484BFA6316C58CE2F01A10AD6.png)

**1\. Parse the text**

![Screenshot 2020-02-20 at 3.25.38 PM.png](/assets/blog_resources/161BE1B0179B87E07239F2F17C2719D2.png)

**2\. Grammar in Semantic Parsing:**

The syntactic part of the grammer is a fairly conventional context-free grammar. These rules have semantic attachments (machine interpretations) in green below.

For example: 

![Screenshot 2020-02-20 at 3.28.18 PM.png](/assets/blog_resources/751051C9E4543ED7720AA23EB20A503A.png)

Note: these grammers are often not deterministic, where there are multiple possible parses per query.

It’s prettying common for there to be ~100s of possible parses to every query.

We need a scoring model to determine which is the optimal parse (trained softmax classifier)

**3\. Grammar Induction:**

For small domains we can manually specify grammar rules, but that doesn’t work for large domains. 

Instead, we induce grammar rules from training data.

One strategy: reduce grammar induction to learning:

* Generate all possible rules (ways of combining the symbols of your semantic representation into syntax rules) - you can generate a huge variety of differnet parses - most of them are bad
* Then use standard learning methods to train rule weights
* But exponential blowup =\> requires pruning strategies.

A very interesting research field!

Note: SippyCup - “playground’ learner language for semantic parsing.

**
**

**Data**

Google spends a lot of money on human annotators for data labelling, and it is very expensive. There are some hacks to make it cheaper:

i.e. One solution: Learn from indirect supervision, using the results of evaluating the semantics (learning from denotations, i.e. the answer to the query, instead of the query itself). 

**Appendices**

**Appendix 1: **

**Challenges in and History of Semantic Parsing**

Challenge: anaphoric readings

Challenge: scope ambiguity

 - No more than three sculptures may be exhibited in any room

 - There cannot be three sculptures may be exhbited in any particular room

 - There cannot be more than three sculptures in any room

 - At most three scuptures have the property that they may be exhbited in any room

You also have to know that a scupture cannot be in two rooms at once.

**History**

**SHRDLU; Blocks World**

Is an example of Semantic parsing

**CHAT-80**, Natural Language interface to geographic facts.

**Appendix 2:**

**Scoring Model for Possible Parses**

![Screenshot 2020-02-20 at 3.36.53 PM.png](/assets/blog_resources/B41E92B366172126965BBA5A154F9635.png)**
**

Learn the scoring model parameters using the EM algorithm

![Screenshot 2020-02-20 at 3.37.32 PM.png](/assets/blog_resources/5E5C5B5F977E2572A264C38C1BAD8A59.png)

x1 and y1 are the inputs and target sementics above. 

There could be multiple parse trees (z) that yield the same semantics, so we have to treat them as latent variables.

That’s what the EM algorithm is good at. 

 E step - use the current parameters to get the n best list of possible parses

 M step - change the weights to put more probabilty on the lements of the n best list that actually match the target semantics.

![Screenshot 2020-02-20 at 3.56.11 PM.png](/assets/blog_resources/7DBDF3072AF27F05974A7FA92EBAB9E9.png)
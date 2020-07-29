Natural language inference

Natural language inference is the task of determining whether a “hypothesis” is true (entailment), false (contradiction), or undetermined (neutral) given a context string.

**Datasets**

SNLI- Dataset from tagged Flickr images, data that describes the event in the image.

MultiNLI - Train premises frawn from five genres: 

 - Fiction, Government, Slate, Telephone, Travel, test set available on Kaggle on different Genres.

![Screenshot 2020-02-18 at 7.14.13 PM.png](/assets/blog_resources/BBD9F8F3F0667695F2F4F620621C27B4.png)

**Hand Built Features**

 - Word\_overlap\_phi

 - Edit distance

 - word differences/ word overlap

 - alignment based features

 - negation

 - quantifier relations

 - NER features 

**Chained Models**

Chain together the query and the hypothesis

NLI’s baselines use hypothesis only (query only, no context).

![Screenshot 2020-02-18 at 7.15.04 PM.png](/assets/blog_resources/A01FE44212786E6E0B6146CFE9FAD7D5.png)

**Grounded Language Understanding**

**
**

**Issues of Indexicality in Language**

 - What does “I”, “we”, “three days ago” mean?

 - “We won” - who is “we”?

 The answer to those questions are dependent on what goals the speaker has - which is why you need lingustic grounding.

 There’s a lot of disambiguation work, and a lot of contextual gounding necessary (Appendix 1)

**Semantic parsers **consume langauge, construct ricj latent representaitons, and predict into structured output spaces. 

How to do grounding - Use Encoder-Decoder.

![Screenshot 2020-02-20 at 11.54.07 AM.png](/assets/blog_resources/20FF1EE37FEF01E5DFE3F2384CC454C4.png)

![Screenshot 2020-02-20 at 2.04.25 PM.png](/assets/blog_resources/8031778E1D26169B123B6AF30DD5A589.png)

FAIR negotiation dataset - 5808 dialogues grounded in 2236 unique scenarios. 

![Screenshot 2020-02-20 at 2.09.17 PM.png](/assets/blog_resources/AED53A1E05E2C662F1B872451BE247EF.png)

The Rational Speech Acts Model

![Screenshot 2020-02-20 at 2.16.36 PM.png](/assets/blog_resources/F5F6757AA465026D66269076050E6FC2.png)

![Screenshot 2020-02-20 at 2.15.18 PM.png](/assets/blog_resources/0A4B7C4B5E9A90490D06EEE2E6C8C3C4.png)**
**

(Example in Appendix 2)

Appendix 1.

![Screenshot 2020-02-20 at 11.41.47 AM.png](/assets/blog_resources/01BA1206A346EB87B489F834BC20AA8D.png)

Winograd sentences require information about the world.

The city councilmen refused the demonstrators a permit because they [feared/advocated] violence.

Appendix 2:

![Screenshot 2020-02-20 at 2.16.56 PM.png](/assets/blog_resources/71619897DB19B5C0D4D8560A2A36278C.png)

![Screenshot 2020-02-20 at 2.17.00 PM.png](/assets/blog_resources/8BE72DAB93239FEC33920C864CE35D6A.png)

![Screenshot 2020-02-20 at 2.17.04 PM.png](/assets/blog_resources/B7DE4719ACF913FA01517338337B9E73.png)

![Screenshot 2020-02-20 at 2.17.07 PM.png](/assets/blog_resources/75F29D07123FEB93B229BDBAC541DE27.png)

A lot of lingusitic problems in pragmatics have been solved by this model, but very high bias and very hard for computers becuse of the number of concepts in the world is very large.

![Screenshot 2020-02-20 at 2.19.20 PM.png](/assets/blog_resources/11699C734AF090BCCE016A901FC5C7C2.png)
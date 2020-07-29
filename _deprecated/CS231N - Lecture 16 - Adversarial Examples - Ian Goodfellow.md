---
category: cs231n
---

Ian Goodfellow spent most of his career looking at adversarial examples.

Adversarial Examples are not due to overfitting: [i.e. overfitting to parameter contours that the network only found in gibbon images].

Surprisingly, it has somethign deeper to do with CNNs themselves.

Goodfellow et al found that the same fooled image was able to output the wrong label on many different types of models, including the ones it was not trained on. In addition, the attack vector which is added to the original image to get the adversarial example could be added to any clean example and we would get an adversarial example in most cases.

**Adversarial Examples are from Underfitting (Excessive Linearity) (Appendix 1)**

The mapping from inputs to NN to outputs are piecewise linear. (The mapping from parameters to outputs are highly nonlinear becuase a lot of parameters are multiplied together).

The issue with linear functions (or piecewise linear functions) is that examples very far from the training data will be classified with veyr high probability, even though no data was ever seen in that subspace, just because of linear extrapolation. 

They picked a random vector with which to adjust the image, and plotted the classification scores when adding different levels of the noise to the image. Because the model uses ReLU, the output is a piecewise lienar function of the input. But in fact, the output is very linear.

Small changes to individual pixels can add up to big changes after multiplying through all the layers of the neural network. You can make small changes that are almost inperceptible but that move you very far when measured by the L2 norm. When we make adversarial examples we limit the change to any particular pixel to a certain band (called the **maxnorm**) so that the changes cannot be concentrated in one place. The overall L2 norm difference might still be large. An easy way to do this is with the **fast gradient sign method**. (Apendix 1)

![Screenshot 2020-03-28 at 5.48.14 PM.png](/assets/blog_resources/AE68467D0C2A6C82F79DB1DD95676BC4.png)

**Appendix 1:**

All models are extremely easy to fool, not just deep models. Linear models, decision trees can all be fooled in this way.

Suppose we have an image on the left is 60% classified as a Panda. After adding the attack adjustment, it is classified with 99.9% confidence that it is a gibbon.

![Screenshot 2020-03-28 at 5.44.17 PM.png](/assets/blog_resources/9D5A95D44D29C770E1A8CFA2C58B059A.png)

![Screenshot 2020-03-28 at 5.44.44 PM.png](/assets/blog_resources/8F6C973E657AA9A638BBC99B6D5DB7A2.png)

![Screenshot 2020-03-28 at 5.45.33 PM.png](/assets/blog_resources/2FE92671C994E563F0E7D0D61DE01FA7.png)

![Screenshot 2020-03-28 at 6.02.15 PM.png](/assets/blog_resources/2E63E21E02FB19704B2DD3018A525D40.png)

The Fast Gradient Sign Method has identified a direction where if we get a large dot product with that direction, we can get an adversarial example. 

Adversarial examples live in linear subspaces. Nearly every linear example is near an adversarial example, and once you move into that subspace, it remains an adversarial example however far you go.

Security implications: you don’t need to get the magnitude right to make an adversarial example: You only need to get the direciton right. Once you move more or less approx to that direction you can fool the model.

![Screenshot 2020-03-28 at 6.05.21 PM.png](/assets/blog_resources/500ED69BC305ABB0E17D86AC4AB9ED60.png)

Adversarial Examples are not Noise - adding noise preserves classification decision in most cases.

![Screenshot 2020-03-28 at 6.09.50 PM.png](/assets/blog_resources/1F4C160A8DE7EBEB181EBDB6BC992E72.png)

In high dimensional spaces, if you choose a reference vector and a random vector in that space, the random vector will on average have a zero dot product with the reference vector. On average, random examples have zero effect on the cost, but adversatial examples are chosen to maximize it.

The adversarial subspace have on average 25 dimensions on MNIST.

Adversarial examples work across multiple models because the overlap between the adversarial subspaces are quite large. Even though they are only 25 dimensional compared to a 3000-dimensional image, the adversarial subspaces tend to be highly dependent on the image classes, for example “pointing from one class centroid to another class centroid”. This increases the probability that the adversarial subspaces of two models overlap. If two different models have a very large adversarial subspace, you know you can transfer adversarial examples fron one to another.

![Screenshot 2020-03-28 at 6.12.21 PM.png](/assets/blog_resources/B1B0156A1280548F9182DB041CD4D7AE.png)

Moden ML ALgorithms are like Clever Hans, the horse that folled everyone by doing arithmetic. They are wrong almost everywhere.

They are correct on naturally occuring input, that is, input that is distributed i.i.d. according to the same distribution of the data they were trained on. But outside of that data manifold, on random data, they are wrong almost everywhere. 

![Screenshot 2020-03-28 at 6.17.28 PM.png](/assets/blog_resources/75354C686FFE7D24254DCE58B9F74209.png)

Pink Box - classified as somethign rather than nothing.

Yellow Box - Fast Gradient Sign Method can make it think it’s an airplane.

**Universal Adversarial Pertubations**

Below: Logistic regression model that discriminates between 7 and 3\. the FGSM attack on the image is just adding the sign of the weights.

When the sign of weights is positive the image looks like the most quintisential 7, when negative, it will look like the most quintisential 3.

![Screenshot 2020-03-28 at 6.29.13 PM.png](/assets/blog_resources/C6A3F68D58EAE39D21BE4DC5AF87169C.png)![Screenshot 2020-03-28 at 6.32.32 PM.png](/assets/blog_resources/124CE782AD79B27AE8BBDB62E8A7CB20.png)

You can do the same thing on ImageNet. This one uses the daisy class. This filter when added to any image is able to make the image be interpreted as a daisy.

Quadratic networks (**shallow rbf network**) is able to resist adversarial examples really well.

But Deep RBF networks are essentially super hard to train, the quadratic term makes the gradient 0 almost always. 

**Adversarial Examples Transferability**

Adversarial exampels generaize from dataset to dataset. Imaging an attacker wants to fool a model, but they don’t know what model architecture is, they don’t know what algorithm (decision tree or DNN), and they also don’t know the parameters.

![Screenshot 2020-03-28 at 6.35.56 PM.png](/assets/blog_resources/F3ECE5A39B5C76C0BCA6564F2BA08A26.png)

They can train their own model to create an attack, make adversarial examples on your model, and with high probability fool the original model without any access to it. 

![Screenshot 2020-03-28 at 6.36.15 PM.png](/assets/blog_resources/851D2BB1879B9A3D83FE76E23427D35F.png)![Screenshot 2020-03-28 at 6.38.43 PM.png](/assets/blog_resources/3B6512B75A01403ED2C776495DD131F0.png)

Dawn Song’s group - 

If you do an Ensemble of many models, and make an adversarial examples, theres a very high probability that your adversarial example will generalize to new models.

You can print out adversarial examples and the new physical version will also fool computer vision models.

![Screenshot 2020-03-28 at 6.42.20 PM.png](/assets/blog_resources/0956F686CCA6C4ACEA4CDE2CC80A222A.png)

![Screenshot 2020-03-28 at 6.43.22 PM.png](/assets/blog_resources/C1292EBE60465AA741760AB51FC0BEDA.png)

![Screenshot 2020-03-28 at 6.43.37 PM.png](/assets/blog_resources/1766BFD3DC6189893636D601B3C3FE5B.png)

Left: Mixture of two Gaussians, Right: Mixture of two Laplacians, Model: Logistic Regresson

**Adversarial Training**

A partial solution: Training on adversarial examples can make models partially more robust. Adversarial training is a very data hungry and compute hungry process, because with each iteration you need to update the adversarial examples with whatever it is that the model has learned recently.

![Screenshot 2020-03-28 at 6.45.52 PM.png](/assets/blog_resources/A7DF6C6B24BEC02D0AA80868AEC2F514.png)

Adversarial training provides regularization and semi-supervised learning.

**Virtual Adversarial Training: **

If we solve adversarial examples, we could do model-based optimization.

**High level Idea:**

Neural networks are not just brittle from overfitting (see a bunch of cases and not generalise becase you don’t correctly parameterize the input distribution), they are also brittle from underfitting. 

Could human thought processes be the same? Overfitting produces nonsense cognition but underfitting means we study everything on a small manifold which is the real world but we dont realize that that is on a much bigger manifold/space.


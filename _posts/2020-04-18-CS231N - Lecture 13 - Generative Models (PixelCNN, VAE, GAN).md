Unsupervised learning allows us to use a lot more data, since unlabelled data is very abundant.

**
**

**Generative Models**

Generative Models do densitiy estimation of our underlying data distribution. 

 - We can do explicit density estimation (explicitly define and solve for p(x))

 - Or we can do implicit density estimation - model that can sample from p(x) without explicitly defining it. (Appendix 1a)

**PixelRNN, PixelCNN**

(Appendix 1b)

Pros: Decently good quality images

Cons: Quite slow to generate pixel by pixel. 

These are an explicit density estimation generative model (called a fully visible belief network).

This model uses the chain rule to decompose likelihood of image x into product of the probabilities of each pixel given the previous pixels.

**PixelRNN** generates image pixels starting from corner, dependency on previous pixels modeled using an LSTM. (This is slow becauses it uses sequential generation!)

**PixelCNN** was introduced to speed things up a but. We still generate image pixels starting from the corner, but deprenency on previous pixels now modeled using a CNN over context region. Maximise the likelihood of the training image. (Training time is faster, but test time still must proceed sequentially -\> still slow)

**Variational Autoencoders**

(Intuition in Appendix 1c.1., Details in Appendix 1.c.2)

Pros: Allows inference of q(z|x), where z is e.g. the amount of smile in an image, which can be a useful feature representaiton for other tasks. 

Cons: Samples blurrier and lower qulaity compared to state of the art (GANs).

Variational Autoencoders is an approximate density generative model. We optimise an intractible density fucntion using a tractable lower bound as a proxy.

**Autoencoders** are models (generally CNN for images) that compress the representations of inputs by being trained to compress a representaiton and then regenerate itself. The architecture is a encoder-decoder, the encoder is 4-layer convolutional network, decoder is 4-layer **upconvolutional** network. If the original image is x, we call the compressed feature representaiton z.

The loss function is L2 loss of the reconstructed image w.r.t. the original image 

|| x - hat(x) || ^2

In autoencoders, the compressed feature representaion z we start with is discrete, and we can generate a different image for each z.

**In variational autoencoders**, we take a distribition over the values of z. For this reason, we do not produce z directly from x in the encoder, but produce the mean and diagonal covariance of z given x.

Then, we don’t produce x directly from z in the decoder, but produce the mean and diagonal covariance of x given z. (See Appendix 1c)

**Generative Adversarial Network (GAN)**

(Appendix 1.d)

Direct implicit density (no explicit density function) sampling.

**Appendix 1a: Intro to Generative Models**

![IMG\_0128.PNG](/assets/blog_resources/1E40222EC9441190C58FE8E500CEF30A.png)

![Screenshot 2020-03-21 at 10.30.35 AM.png](/assets/blog_resources/AF7976FA724274756EFED89C7C925F8F.png)

**Appendix 1b: **

PixelRNN, PixelCNN

This model uses the chain rule to decompose likelihood of image x into product of the probabilities of each pixel given the previous pixels.

![Screenshot 2020-03-21 at 10.34.38 AM.png](/assets/blog_resources/9A15C742FC477957EEE2B3F0EB78A1BC.png)

![IMG\_0134.PNG](/assets/blog_resources/5AFBA744AC98A5233A9B80DE0C6321C1.png)

![Screenshot 2020-03-21 at 10.38.19 AM.png](/assets/blog_resources/A5868552185A288680EFECA092ABD461.png)

![IMG\_0136.PNG](/assets/blog_resources/EC4A6E508E8CB2C910EB6FB6A1DD8F39.png)

![IMG\_0138.PNG](/assets/blog_resources/54ECDF6014B4001F9DCC8F5ADBF72075.png)

**Appendix 1c.1: ****Variational Autoencoders (VAE) Intuition**

<https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73>

At this point, a natural question that comes in mind is “what is the link between autoencoders and content generation?”. Indeed, once the autoencoder has been trained, we have both an encoder and a decoder but still no real way to produce any new content. At first sight, we could be tempted to think that, if the latent space is regular enough (well “organized” by the encoder during the training process), we could take a point randomly from that latent space and decode it to get a new content. The decoder would then act more or less like the generator of a Generative Adversarial Network.**
**

To illustrate this point, let’s consider the example we gave previously in which we described an encoder and a decoder powerful enough to put any N initial training data onto the real axis (each data point being encoded as a real value) and decode them without any reconstruction loss. In such case, the high degree of freedom of the autoencoder that makes possible to encode and decode with no information loss (despite the low dimensionality of the latent space) **leads to a severe overfitting** implying that some points of the latent space will give meaningless content once decoded. If this one dimensional example has been voluntarily chosen to be quite extreme, we can notice that the problem of the autoencoders latent space regularity is much more general than that and deserve a special attention.

![Screenshot 2020-03-21 at 12.02.58 PM.png](/assets/blog_resources/FA7C99A2084EB58DF93C7D60FCBFD1EE.png)

When thinking about it for a minute, this lack of structure among the encoded data into the latent space is pretty normal. Indeed, nothing in the task the autoencoder is trained for enforce to get such organisation:** the autoencoder is solely trained to encode and decode with as few loss as possible, no matter how the latent space is organised.**

One possible solution to obtain such regularity is to introduce explicit regularisation during the training process. **Thus, as we briefly mentioned in the introduction of this post, a variational autoencoder can be defined as being an autoencoder whose training is regularised to avoid overfitting and ensure that the latent space has good properties that enable generative process. **In particular, the latent space representation of the image is forced to be very close to a standard normal distribution.

…

**The Reparameterization Sampling trick**

The overall architecture is then obtained by concatenating the encoder and the decoder parts. However we still need to be very careful about the way we sample from the distribution returned by the encoder during the training. The sampling process has to be expressed in a way that allows the error to be backpropagated through the network. A simple trick, called **reparametrisation trick**, is used to make the gradient descent possible despite the random sampling that occurs halfway of the architecture and consists in using the fact that if z is a random variable following a Gaussian distribution with mean g(x) and with covariance h(x) then it can be expressed as

![Screenshot 2020-03-21 at 12.09.25 PM.png](/assets/blog_resources/FC2E7C98EC8200BDC732F47D082EB295.png)

**
**

**Appendix 1c.2: ****Variational**** Autoencoders (VAE) Details****
**

VAEs are approximate density models - which is, we do not optimize the density directly, instead we optimize an approximate density which is a lower bound on the density.

![IMG\_0139.PNG](/assets/blog_resources/721ACEF02B194B611B3B517FA32BA9B5.png)

**Intro: Autoencoders:**

Autoencoders are models that compress the representations of inputs by being trained to compress a representaiton and then regenerate itself. Encoder-decoder architecture, decoder is 4-layer upconvolutional network.

The loss function is L2 loss of the reconstructed image w.r.t. the original image 

|| x - hat(x) || ^2

![IMG\_0142.PNG](/assets/blog_resources/98150BBE4E33163191CB67DF00CEFB2C.png)

After training, we throw away the decoder. The encoder now can generate the low-dimensionality feature representation of the image. We can use the encoder outputs as the inputs to some supervised model (make sure to do some fine-tuning on the new objective).

![Screenshot 2020-03-21 at 10.52.36 AM.png](/assets/blog_resources/11FC2562FDF296507ADA4AA076160C42.png)![IMG\_0143.PNG](/assets/blog_resources/0BF1B7A32027117F602BD64C8B77B1EE.png)

**Variational Autoencoders:**

We assume training data is generated from underlying unobserved representaiton z (where z is latent factors used to generate).

We first sample from a prior (like a gaussian) that determines our expected distribution of the latent factor. Then, given the sampled latent factor, we same from the space image space conditional on z. (z is like how much pose, how much smile).

Sample from p(z) using Gaussian.

Sample from p(x|z) using neural network.

![IMG\_0145.PNG](/assets/blog_resources/2CD62C8954C5E57B8A431170B2C087EF.png)**
**

![Screenshot 2020-03-21 at 10.59.29 AM.png](/assets/blog_resources/FC8FBF2FA6E918ABBBB622E328F23222.png)

![Screenshot 2020-03-21 at 10.59.34 AM.png](/assets/blog_resources/099A1173C2735C8C96CFDE274C1B4B7F.png)

![Screenshot 2020-03-21 at 11.01.32 AM.png](/assets/blog_resources/4D07627EB3248C8A0D607EB37C5296BF.png)

Problem: The integral is intractible (the computation cannot be computed in reasonable time).

Solution. Instead of bothering with p(x) (See below slide) which we use to compute the posterior density p(z|s), what we really care about is the posterior density itself. So instaed we define an additional encoder network q(z|x) that approximates p(z|x) directly. 

![IMG\_0150.PNG](/assets/blog_resources/11C08FAC992E7C5C0A4494EA00507E72.png)

In autoencoders, the compressed feature representaion z we start with is discrete, and we can generate a different image for each z.

In variational autoencoders, we take a distribition over the values of z. For this reason, we do not produce z directly from x in the encoder, but produce the mean and diagonal covariance of z given x.

Then, we don’t produce x directly from z in the decoder, but produce the mean and diagonal covariance of x given z.

![Screenshot 2020-03-21 at 11.35.39 AM.png](/assets/blog_resources/9830AD5C3A85EB5C99069005D69C4946.png)

![Screenshot 2020-03-21 at 11.38.27 AM.png](/assets/blog_resources/92BEEE14702ED168B63FA15098ACD197.png)

![Screenshot 2020-03-21 at 11.38.40 AM.png](/assets/blog_resources/33756B08C60CB46F50F35E4652AE7762.png)

![Screenshot 2020-03-21 at 11.39.57 AM.png](/assets/blog_resources/C517CC7B451420DED371A2898E4AB337.png)

![Screenshot 2020-03-21 at 11.41.11 AM.png](/assets/blog_resources/5225B50257A1339139759704B8513819.png)

**Training VAEs**

1\. Take a minibatch of input data

![Screenshot 2020-03-21 at 11.44.33 AM.png](/assets/blog_resources/2775B04C1DAB7561288B5373D6EF0C07.png)**
**

2\. Pass it through encoder network q(z|x) to get mu\_{z|x}, sigma\_{z|x}. [One value for each minibatch?] Use q(z|x) to compute the second (KL) term of the loss. 

![Screenshot 2020-03-21 at 11.44.51 AM.png](/assets/blog_resources/A593065E496CD0D4A841D569EF3179B7.png)

3\. Sample z from z|x~N(mu\_{z|x}, Sigma\_{z|x}). Pass z through decoder network p(x|z) to get mu\_{x|z}, Sigma\_{x|z}. Note that we make the sampling step differentiable through the reparameterization trick (see Kingma’s original paper)

![Screenshot 2020-03-21 at 11.46.07 AM.png](/assets/blog_resources/526792F01624668A0F3BFC5B78DAE7C0.png)

4\. Sample x from x | z ~ N(mu\_{x|z}, Sigma\_{x|z})![IMG\_0160.PNG](/assets/blog_resources/AC73B833994DB5551C2D9B75A4C19BD5.png)

**Generating Data from VAEs**

Use just the decoder network. We sample z from some prior, like a normal gaussian, get the mean and variance of x |z, and sample x from that distribution.

![IMG\_0161.PNG](/assets/blog_resources/03C1663489043D84C37E2D04853AF898.png)

Different dimensions of z encoder interprable factors of variation [how to we pick a z such that this occurs?]

![Screenshot 2020-03-21 at 11.54.41 AM.png](/assets/blog_resources/72497BAC6C636DAA25999499BC1EF04F.png)![IMG\_0163.PNG](/assets/blog_resources/C556A8B4630C926713432E6EA9CE8D22.png)

![IMG\_0164.PNG](/assets/blog_resources/48A4247754416C97576757D9055B9A47.png)

**Appendix 1.d- ****Generative Adversarial Networks**

![IMG\_0169.PNG](/assets/blog_resources/DBDE2630B7CF63237F8767C90659C6DC.png)

![IMG\_0172.PNG](/assets/blog_resources/834298E584BA792C7DC3F24362D92E1A.png)

![IMG\_0174.PNG](/assets/blog_resources/92D2384F72B825BAD37A365A3B3845FE.png)

![IMG\_0176.PNG](/assets/blog_resources/6DA02A29B8915A301DFCEF816CB50723.png)

![IMG\_0178.PNG](/assets/blog_resources/FFB41D8A77E266B34603A9FB7B414518.png)

Wasserstein GAN- more stable GAN for training (read)

Convolutional Architectures for GANs

![Screenshot 2020-03-21 at 12.24.33 PM.png](/assets/blog_resources/AF647589DBACB3404C6346DC79692896.png)

![Screenshot 2020-03-21 at 1.35.37 PM.png](/assets/blog_resources/39AB6BB335644C66BBA4331D32A77F72.png)

Since like the variational autoencoder the GAN a low-dimensional feature space representation of the image (the prior, the input to the generator), we can use that representation to interpolate between image latent spaces.

![IMG\_0180.PNG](/assets/blog_resources/CF99E2334EA27BF6D02B5063CDDCF73D.png)

![Screenshot 2020-03-21 at 1.37.41 PM.png](/assets/blog_resources/6688A62F5150F38FE93324A46B740840.png)

![IMG\_0182.PNG](/assets/blog_resources/C903B4A245977B91F96CE44DD5EDEA11.png)

![IMG\_0184.PNG](/assets/blog_resources/0C01D3B1A5944EC094D46CD286AB58A9.png)

![Screenshot 2020-03-21 at 1.44.27 PM.png](/assets/blog_resources/C44BACDBB7D6EFB56EAF4374A3FF8BAE.png)
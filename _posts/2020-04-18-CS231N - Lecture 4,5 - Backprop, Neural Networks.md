**Backprop**

See (1)

**History of NN**

Rosenblatt Perceptron

Widrow and Hoff: 1960 Adaline /Madaline - multi-layered perceptrion

1986 Rumelhart - Backprop

Hinton and Salakhutdinov 2006

Deep belief networks for acoustic recognition (Adbel-rahman Mohamed) 2010

AlexNet 2012

**CNNS**

Hubel and Weisel - 1959 Cat Neocortex -found neurons had hierarchical orientation

Fukushima 1980 - Neuroconitron . Alternating lauers of simple (pooling) and complex cells

LeNet - 1980- Handwriting recognition for zip codes. Applied Backprop to CNNs

ALexNet - 2012

Now - Classificaiton, Image Retrieveal, Object Detection, Object Segmentation, Video, Facial Recognition, Pose Recognition, Game Playing

Medical images, classification of galaxies, aerial maps, Image captioning, style transfer

**
**

![Screenshot 2020-02-25 at 11.56.46 AM.png](/assets/blog_resources/27B946627A98D8F6AC747687E33C95EF.png)**
**

Note that each filter produces a dot product with the entire depth of the image, and produces one number as output (the results of the dot product for each layer of the image are added together).

Use multiple filters to stack depth in the next layer.

![Screenshot 2020-02-25 at 11.59.46 AM.png](/assets/blog_resources/DEC111AF97909199529D664B8F6466B1.png)**
**

![Screenshot 2020-02-25 at 12.00.13 PM.png](/assets/blog_resources/7AEFCD160E86A816E833BA47B66F0F3F.png)

![Screenshot 2020-02-25 at 12.00.37 PM.png](/assets/blog_resources/BD6290A18F647B23255D2798D7AB4CF8.png)

(2)

Code for Convolutions in Torch (Appendix 4)

Dimensionality of the Convolution (3 for details)

You can slide the convolutional filter also with different stride.** Output size = N-F/stride + 1\. **Stride is roughly the resolution in which you are looking at your image. It shrinks the image.

We zero-pad our image so that we can maintain the image size. Note that the number of filters is usually a power of 2.

Example:

Input volume: 32x32x3, 10 5x5 fitlers with stride 1, pad 2 

=\> 5x5x10 x depth (3) p arameters = 750 parameters + 10 bias terms = 760 parameters

**Pooling Layers**

Dimensionality of the Pooling Layer (3 for details, but basically 2x2 and 3x3 pooling is all that people ever use)**
**

Compress the prepresentations, downsampling them to make them smaller and more manageable.

Example: Max-pool - take the max activation in each region.

Note that pooling is performed over the whole depth (pooling perserves depth).

Interpretation of max-pooling: the activation is “now much the neuron fired in this region”. Max-pooling gives the maximum of how much each filter is active in the local region (for each filter, because filters are counted in depth and max-pooling is performed over the whole depth).\\

Note: You can also use stride to use down-sampling, and is getting more popular, but pooling is preferred. 

![Screenshot 2020-02-25 at 12.27.42 PM.png](/assets/blog_resources/A26AAFC006AFE02067A78ABFBE8E1EEB.png)

Trends:

Trend towards smaller filters and deeper architectures, treidn towards getting rid of POOL/FC layers (just CONV)

Typical architectures look like

**[(CONV-RELU) \*N -POOL?]\*M-(FC-RELU) \*K, Softmax**

where N is usually up to ~5, M is large, K between 0 and 2,

but recent advances such as ResNet/GoogLeNet challenge this paradigm

**Appendices**

**Appendix 1: Computing Backprop**

I draw my gradient flow diagrams like this.

If there are any values, here called *statistics*, aggregated across the batch (like means, std deviations etc. These are 1d and are affected by every instance in the batch, likewise their value affects the backprop of every instance in the batch.) I will **underline** them.

Every time an aggregation is done, I mark that flow with a squiggle ~~.

 - Squiggled edges require re-expansion of the (1D) gradient flowing in back into a 2D form. In Usually that means initializing a 2D matrix and having the 1D gradient affect it somehow. In the case of the average function, the appropriate gradient is 1/N \* np.ones((N,D)) \* d\_out

Every time a deaggregation is done (the aggregated statistic is applied to all elements of the batch somehow), I mark that flow with a dash —.

 - Dashed edges take the gradient flowing in and sums it across elements (in numpy, axis=0) to get the gradient of the statistic. 

![IMG\_0219.jpg](/assets/blog_resources/F5F653DF8462614DAB448DFBF8F07850.jpg)

In the below image, we show the flor of gradients. Note that whenever the original value is underlined, the gradient must also be underlined (a 1d vector). When we move from not underlined to underlined we use np.sum(…, axis=0). When we move from underlined to not underlined, we mark it as expanded and in numpy we initialize a 2D matrix (e.g. np.ones(N,D)) and adjust it according to the original gradient. 

In each gradient computation, 

dL/dY1 = dL/dY2 \* dY2/dY1

where the first term is the gradient flowing back and the second term is the chain rule derivative of this step. In particular, if we have the flowed back gradient at Y2, we take the flowed back gradient and multiply it by a chain rule derivative with respect to the variables in the previous step Y1\. For eample in the image below, given the gradient d\_1\_std at (1/N) the gradient at √(v+e) is d\_1\_std \* ( - std ^(-2)). Thus we use the value at the previous step and plug it in to the derivative of the current step, and multiply that by the backflowing gradient.

![IMG\_0220.jpg](/assets/blog_resources/B8430A26D9343900018743211BA92D12.jpg)**
**

**Appendix 1: Backprop**

- Gradient of Softmax Loss:
  If we have a softmax function $p_j = \frac{e^{o_j}}{\sum_k s^{o_k}}$, then it has a loss function of the form $L = - \sum_j y_j \log(p_j)$.
  The derivative of this loss is $\frac{\partial L}{\partial o_i} = p_i - y_i$ for the target class and $p_i$ for the other classes.
- Gradient of Softmax Function:
Note that the gradient of the Softmax function (not its negative log likelihood loss) with output $i$ $S_i$ is $D_jS_i = S_i(\delta_{ij} - S_j)$, where $\delta_{ij}$ is the Kronecker delta function ($delta_{ij}=1[i=j]$)

![IMG\_9926.PNG](/assets/blog_resources/CFE3AD22D324EBA88364BD6192D1E39D.png)

**Vectorized gradients**

The full Jacobian used to compute the gradient is obviously a matrix. However, it is often sufficient to store a vector if most of the elements dont affect one another.

For functions like

$$f(x) = max(0,x)$$
Only $x_i$ affects $x_i$, so the Jacobian is diagonal. We only have to store the effects of $x_i$ on $x_i$ ($O(n)$ space) and not any other $x_j$.

![IMG\_9931.PNG](/assets/blog_resources/4D66A15067B2EC13480FCD43B2B2DA23.png)![IMG\_9932.PNG](/assets/blog_resources/344AAD4707A5FAEBC3EA83C604EBCB25.png)![IMG\_9936.PNG](/assets/blog_resources/5FA12ED6B42838B2DEF6AD84865E8F3A.png)

Appendix 2:

![Screenshot 2020-02-25 at 12.05.07 PM.png](/assets/blog_resources/4508A4B5BD30C65C771304B9935CDAEA.png)

Play Around wiht Andrej Karparthy’s ConvNet JS Demo!

<https://cs.stanford.edu/people/karpathy/convnetjs/demo/cifar10.html>

![Screenshot 2020-02-25 at 12.55.26 PM.png](/assets/blog_resources/0FA9BC0B95B1E9C7FB593FB94AE7AA47.png)

Appendix 3:

**Dimensionality of the Convolutional Layer**

![Screenshot 2020-02-25 at 12.19.02 PM.png](/assets/blog_resources/51E8118561CA96EBDF039681E08D92E7.png)

Note: 1x1 convolutions make perfect sense; they change the depth of the layer.

**Dimensionality of the Pooling Layer**

![Screenshot 2020-02-25 at 12.32.44 PM.png](/assets/blog_resources/41114512D51E0606D1ED8C1996B87E19.png)**
**

Appendix 4: 

**Convolutions in Torch**

![Screenshot 2020-02-25 at 12.21.00 PM.png](/assets/blog_resources/D0192A260A8605CE7F20CA9E76424747.png)
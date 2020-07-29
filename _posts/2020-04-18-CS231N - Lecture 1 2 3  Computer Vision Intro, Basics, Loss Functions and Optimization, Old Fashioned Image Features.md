**Computer Vision Intro**

**History of Computer Vision**

The onset of vision started the Cambrian Evolution. 

In Humans, 50% of our neurons are involved in visual processing.

Hubel and Weisel - Cat visual neocortex.

Block World - first Vision paper

1966 - The Summer Vision Project

David Marr - MIT vision scientist, wrote book \<Vision\>

Generalized Cylinder, Pictoral Structure

Object segmentation (Normalized Cut 1997)

Face Detection (Viola and Jones)

SIFT Features (Lowe, 1999)

1998 - LeCun et al 1998\. First CNN paper for recognizing digits

Spatial Pyramid Matching (2006)

Human Gesture Recognition (Histogram of Gradients HoG 2005, Deformable Part Model 2009)

**Old Fashioned Image Features**

Color Histogram - have a huge spectrum of color buckets and for each pixel, map the pixel into the bucket to get a histogram of colors (Appendix 6)

Histogram of Oriented Gradients (HoG) - divide image into 8x8 pixelm regions, within each region quantize edge direction into 9 bins, which is a histogram of occurence of 9 difference types of edges in this 8x8 pixel region (9d vector). Formerly very popularly used for object recognition. (Appendix 6)

Bag of Words - Extract random patches from images, cluster patches to form “codebook” of visual words”, and encode images of “bags” of these patches. The “words” tend to cover different types of edges. (Appendix 6)

**Datasets:**

Object Recognition was the blocking problem in Computer Vision (PASCAL Visual Object Challenge, 20 object categories)

CIFAR 10 (small test set - 10 classes, 50,000 training images, 10000 test images)

ImageNET - Recognize most of the objects in the world (downloaded billions of data off the internet, organized by wordNET, then labeled using Mechanical Turk) - 1000 classes, 1.4 million images (2)

**CV Problems**

Image Classification 

Object Detection

Action Classification

Image captioning

Sementic Segmentation

AR/VR

**Naive Baselines**

 - Toy Naive Baseline: k- Nearest Neighbors with L1 distance - Never used in practice because pixel distance between images is not semantically important, and because of the curse of dimensionality (Appendix 1)

 - Toy Naive Baseline: Linear Classification

**Loss Functions and Optimization:**

In classification generally these two types are used:

SVM Loss (Appendix 4)

Softmax loss (Appendix 4)

You also have to do regularization (Appendix 5)

**
**

**Appendix 1:**

L1 metric for k-Nearest Neighbors

![Screenshot 2020-02-23 at 10.44.10 PM.png](/assets/blog_resources/6F223AC67DC8F51127DCB432E3DA5EDC.png)

L1 distance depends on the coordinate system (so if each feature has a defined distance, maybe L1 is best)

You can also use L2 distance. 

**K-Nearest Neighbors**: Increasing k smoothes your decision boundary. Classification based on majority vote between k nearest neighbors.

![Screenshot 2020-02-23 at 10.47.16 PM.png](/assets/blog_resources/9E7233F4094C31FA3008565E1A27BEC7.png)

**Appendix 2: State of the Art on ImageNet**

![Screenshot 2020-02-23 at 10.11.10 PM.png](/assets/blog_resources/ABBC2081A80BB76E4AB4BA8C71EF72B3.png)**
**

**
**

![Screenshot 2020-02-23 at 10.09.45 PM.png](/assets/blog_resources/3C0CD18124519890E9B58F88F528C7E5.png)

![Screenshot 2020-02-23 at 10.08.31 PM.png](/assets/blog_resources/051EAFD94A97DB7D28EFF6DA7C2C64BB.png)

<https://paperswithcode.com/sota/image-classification-on-imagenet>

**Appendix 3:**

Image Retrieval Using Scene Graphs

![Screenshot 2020-02-23 at 10.14.35 PM.png](/assets/blog_resources/785C8DCEF2B650CB354846163EFDE442.png)

Appendix 4: SVM Loss

![Screenshot 2020-02-24 at 10.40.47 AM.png](/assets/blog_resources/32510388F73AE313CC206B91D627F169.png)

![Screenshot 2020-02-24 at 10.44.32 AM.png](/assets/blog_resources/22FD4F9EF664C8316532450F4740B297.png)

![Screenshot 2020-02-24 at 11.13.54 AM.png](/assets/blog_resources/E749F00F3C84BD9E074747DBE7280AB4.png)

![Screenshot 2020-02-24 at 11.16.17 AM.png](/assets/blog_resources/1F37A5E6852F9ACFE15D9615B51303C2.png)![Screenshot 2020-02-24 at 11.39.00 AM.png](/assets/blog_resources/D2ACD583F4C569698C1A88530E707441.png)

**Appendix 5: Regularization**

![Screenshot 2020-02-24 at 10.55.59 AM.png](/assets/blog_resources/56524F105C46B44ACA5012AEAE445ADA.png)**
**

![Screenshot 2020-02-24 at 11.03.51 AM.png](/assets/blog_resources/8359A90CE4E036773EB8F82E52A596D5.png)

**Appendix 6**: **Old Fashioned Image Features**

![Screenshot 2020-02-24 at 2.45.54 PM.png](/assets/blog_resources/2FE5C321A0FE669231AE6114F5B9B314.png)

![Screenshot 2020-02-24 at 2.46.13 PM.png](/assets/blog_resources/7AFC2288AB93B7A8A177559013FB797A.png)

![Screenshot 2020-02-24 at 2.51.08 PM.png](/assets/blog_resources/1C4037E568DC9DBEC39F596F7BB49156.png)
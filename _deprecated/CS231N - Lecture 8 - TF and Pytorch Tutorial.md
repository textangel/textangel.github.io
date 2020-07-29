---
category: cs231n
---

\* Note this is as of 2017, may be outdated.

**CPU vs GPU** (Appendix 3)

**Deep Learning Framework**

Allows you to easily build big computational graphs, easily compute gradients, and have optimizations done for you. Numpy can’t run on GPU and you have to compute your own gradients.

**Tensorflow - Static computational graph, better for deployment**

Appendix 1

You need to learn a new functional graph language to work with tf. However, The language is very optimized.

First define computational graph, then run the graph many times (Appendix 3) - Allows graph optimization.

Once graph is built, can serialize it and run it without the code that built the graph. You can train it in Python and deploy it in C++ or something. 

Requires special tensorflow control flow operator to determine control logic in graph before we run it. 

Requires special tensorflow control flow looping constraicts `tf.foldl` determine loop logic in graph before we run it (functional programming)

**PyTorch- Dynamic computational graph, better for iteration**

Appendix 2

You can use the imperative programming logic that you’re used to.

Creates new computataional graph each iteration (dynamic computational graph). Always need to keep the original code around if you want to keep using the graph. 

However, code is a lot cleaner for conditionals and loops. 

Recurrent Networks or Recursive Networks use different length graphs. This is harder to do in TF (static graph) than PyTorch (dynamic graph)

Facebook has Caffe2 for deployment, where you fill out a model template and then can deploy it.

**High Level Comparison Between the Three**

![Screenshot 2020-02-26 at 1.19.38 PM.png](/assets/blog_resources/3864195E6052DD85FAE9FA58BEA06C9C.png)

**Appendices**

**Appendix 1:** 

Tensorflow

**
**

**Vanilla Tensorflow Code (copies tf.placeholder from CPU to GPU each call, inefficinent )**

![Screenshot 2020-02-26 at 1.20.59 PM.png](/assets/blog_resources/285FF9138FED213DC5626310A7DA538B.png)

![Screenshot 2020-02-26 at 1.21.43 PM.png](/assets/blog_resources/0F56858CDDC3DB12C5DF1709DD6C3D98.png)

![Screenshot 2020-02-26 at 1.22.06 PM.png](/assets/blog_resources/F99B8FD66C4AAE51B4200305D986FE8D.png)

![Screenshot 2020-02-26 at 1.23.02 PM.png](/assets/blog_resources/34C101A41B486069EA2E0D6D954689EF.png)**
**

![Screenshot 2020-02-26 at 1.23.20 PM.png](/assets/blog_resources/B5BD532E7452CA3FDC791E5101FDFBE7.png)**
**

![Screenshot 2020-02-26 at 1.23.26 PM.png](/assets/blog_resources/A65044A5135BDDB66D4298F949AA967B.png)**
**

![Screenshot 2020-02-26 at 1.23.39 PM.png](/assets/blog_resources/91DDED52D8F8141B72D99BB901E34EE8.png)**
**

![Screenshot 2020-02-26 at 1.25.03 PM.png](/assets/blog_resources/D421D556E49DE3ACCD9E6F337EC5DC66.png)

**
**

**Using tf.Variable to avoid copying from CPU to GPU each call**

![Screenshot 2020-02-26 at 1.25.14 PM.png](/assets/blog_resources/B4B011DA2F87553FBCC541B3F7104C48.png)**
**

You have to use tf.assign to update tf.Variables

![Screenshot 2020-02-26 at 1.25.45 PM.png](/assets/blog_resources/7930F04934BDFA4EED0CFEB990C9B32B.png)**
**

![Screenshot 2020-02-26 at 1.26.07 PM.png](/assets/blog_resources/D29988118C518DB277255388789DB189.png)**
**

![Screenshot 2020-02-26 at 1.26.31 PM.png](/assets/blog_resources/E4B45AE6551339DEAEF0A39804B5D966.png)**
**

**Adding Dummies to make sure that intermediate updates are computed**

Tf will not compute any values not necessary to computing the outputs. Therefore, you have to make sure that everything that you want to compute is explicitly output (and set as ‘\_’ ). This helps the tf optimizer know what to compute.

![Screenshot 2020-02-26 at 1.27.20 PM.png](/assets/blog_resources/857F6ED0E0DFFC65AFA19EA53508F5EA.png)**
**

**Use tf.train (GradientDescentOptimizer) instead of writing the update step yourself**

![Screenshot 2020-02-26 at 1.29.32 PM.png](/assets/blog_resources/E3E4ED7F90F8B36704D19E9343569D32.png)**
**

**Use tf.loss (mean\_squared\_error) instaed of computing the loss yourself**

![Screenshot 2020-02-26 at 1.30.45 PM.png](/assets/blog_resources/819B4838BD581699D589FA5EB23E853F.png)**
**

**Use tf.layers to automatically set up weights (and bias), initializations, nonlinearity instaed of computing those yourself**

![Screenshot 2020-02-26 at 1.31.26 PM.png](/assets/blog_resources/2A131A1D24A596B071F5ABA513A1CF48.png)**
**

**Other Nice tf libraries**

![Screenshot 2020-02-26 at 1.32.33 PM.png](/assets/blog_resources/7F9C10538EBC8C93FC85815A1BC1E78F.png)

![Screenshot 2020-02-26 at 1.33.45 PM.png](/assets/blog_resources/72D6D674251B3B3CA13C7B5081883A45.png)

![Screenshot 2020-02-26 at 1.34.04 PM.png](/assets/blog_resources/F7E090851E3E13D449944A633C3124EF.png)

![Screenshot 2020-02-26 at 1.34.09 PM.png](/assets/blog_resources/1C8C695E064F4220777DB2BE2792DC29.png)**
**

**
**

**Appendix 2:**

**Pytorch**

![Screenshot 2020-02-26 at 1.34.57 PM.png](/assets/blog_resources/AFEFAFF3A0FA80DFF187035B62827948.png)

**Pytorch Tensors are basically numpy arrays that live in CPU/GPU (.cuda)**

![Screenshot 2020-02-26 at 1.35.19 PM.png](/assets/blog_resources/B0F9F636DECB727DFB6F2E2F12771B94.png)

![Screenshot 2020-02-26 at 1.36.01 PM.png](/assets/blog_resources/5C8F3423AE6159BAD6435A66FF49065C.png)

PyTorch Variabes are Nodes in a computational graph. They include a **.data** and a **.grad** attribute.

Pytorch Variables and Tensors have exactly the same API

![Screenshot 2020-02-26 at 1.36.20 PM.png](/assets/blog_resources/F658D0E15EE7FE71ED29760BCA7DCC7D.png)

![Screenshot 2020-02-26 at 1.36.32 PM.png](/assets/blog_resources/5A27649D9BFF3CDB18D0737D9AD2AF8D.png)

![Screenshot 2020-02-26 at 1.36.40 PM.png](/assets/blog_resources/EF9403E223F1BFF130E97EF2D3EA8967.png)

![Screenshot 2020-02-26 at 1.36.50 PM.png](/assets/blog_resources/2496EE65465AC2B0100C0E6ADF025C95.png)

Make sure you zero out the gradients before you recompute them!

![Screenshot 2020-02-26 at 1.37.00 PM.png](/assets/blog_resources/D89782931F7BBA400EC50DD08E44761A.png)

You can make custom autograd functions in pytorch, although usually you won’t need to.

![Screenshot 2020-02-26 at 1.37.23 PM.png](/assets/blog_resources/30EFC749B93A28D571EB6565819D4BD8.png)

![Screenshot 2020-02-26 at 1.37.40 PM.png](/assets/blog_resources/6B86E868D416DFECD2FCB2B46DC023F8.png)

**Use pytorch.nn as a high level wrapper for working with neural nets, like Keras**

![Screenshot 2020-02-26 at 1.37.50 PM.png](/assets/blog_resources/46C25B311D77ADAF1F824830C92F50B1.png)

![Screenshot 2020-02-26 at 1.38.16 PM.png](/assets/blog_resources/EE9B507FC02CC84B51C796D694F23C03.png)

![Screenshot 2020-02-26 at 1.38.22 PM.png](/assets/blog_resources/23833726858257D16C5D96C10FF5F7C2.png)

![Screenshot 2020-02-26 at 1.38.36 PM.png](/assets/blog_resources/5473BB6410DF6D4004515FD28166F02B.png)

**Use pytorch.optim to select different optimizers**

![Screenshot 2020-02-26 at 1.38.50 PM.png](/assets/blog_resources/DE6492153F1C700DFAED1970C61D7422.png)![Screenshot 2020-02-26 at 1.38.57 PM.png](/assets/blog_resources/E3EE5CDABF308EBB4FA2AA9700CFAC09.png)

**Define Modules with torch.nn.module**

![Screenshot 2020-02-26 at 1.39.05 PM.png](/assets/blog_resources/0B60FB267980EEF34C309788098840E8.png)

![Screenshot 2020-02-26 at 1.39.16 PM.png](/assets/blog_resources/686183EEC378400CAD334BCBB5FBDEA1.png)

![Screenshot 2020-02-26 at 1.39.24 PM.png](/assets/blog_resources/F298030A511B29EA3F530DA75B132AA0.png)

![Screenshot 2020-02-26 at 1.39.28 PM.png](/assets/blog_resources/47B61DBA717A7A0B0561AB336146F37B.png)

![Screenshot 2020-02-26 at 1.39.40 PM.png](/assets/blog_resources/ED8D99392C2A885075515BC846174A33.png)

**Use DataLoader to get minibatching, shuffling, multithreading funcitonality**

![Screenshot 2020-02-26 at 1.39.58 PM.png](/assets/blog_resources/4242B3277D98CA11910BD3E2A015FB1C.png)

![Screenshot 2020-02-26 at 1.40.11 PM.png](/assets/blog_resources/B63F79E0D360B1B77BA19190C38A14A7.png)

**
**

**Use Pytorch Pretrained Models (for vision it’s torchvision)**

![Screenshot 2020-02-26 at 1.40.20 PM.png](/assets/blog_resources/AC8DD74FDAB9AFEF882879733F482D41.png)

![Screenshot 2020-02-26 at 1.40.31 PM.png](/assets/blog_resources/736A077B98FC2F371A770F234DA7E9CD.png)

**Tensorflow vs PyTorch: Static Graphs vs Dynamic Graphs. Loops and Conditionals**

![Screenshot 2020-02-26 at 1.41.33 PM.png](/assets/blog_resources/0AE206FD9AE4B8004B80DBF047B97FAF.png)

![Screenshot 2020-02-26 at 1.44.50 PM.png](/assets/blog_resources/DDA0BFB0F1B75B3342DC17CABB140DD2.png)

![Screenshot 2020-02-26 at 1.47.16 PM.png](/assets/blog_resources/F2011BF676983C3CC152C66215D1F722.png)

**Appendix 3**

CUDA - Write C-like code that runs directly on the GPU

 - Higher level APIs - cuBLAS, cuFFT, cuDNN, etc

 - Use cuDNN, 3x faster than unoptimized CUDA.

OpenCL

 - Similar to UCA but runs on anything

Data Reads can be bottleneck in training. Solutions:

 - Read all data into RAM

 - Use SSD instead of HDD

 - Use multiple CPU threads and prefetch data and buffer it in RAM.

GPU has their own RAM on the chip**
**

![Screenshot 2020-02-26 at 1.07.49 PM.png](/assets/blog_resources/4460549F76F458332ACBC6AF3FB02B04.png)

![Screenshot 2020-02-26 at 1.12.33 PM.png](/assets/blog_resources/3080A49C06E071026ED121A1E2C2E268.png)

Using cuDNN optimizations gives 3x speed over vanilla CUDA
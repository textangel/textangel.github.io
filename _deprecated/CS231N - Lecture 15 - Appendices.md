---
category: cs231n
---

**Appendix 1: ****Algorithms for Efficient Inference**** **

**Appendix 1.1** ** **Deep Compression (aka, Pruning, Quantization, and Huffman Encoding)

![Screenshot 2020-03-28 at 1.37.35 PM.png](/assets/blog_resources/C80D31DB9576727051D8AAA48A03EF3A.png)

![Screenshot 2020-03-28 at 1.37.45 PM.png](/assets/blog_resources/0385D241A19AC26B5C2CDE86253B4B68.png)

![Screenshot 2020-03-28 at 1.40.40 PM.png](/assets/blog_resources/D242C15A4FB6CDEA443DC7A7AD82A02C.png)

Pruning + Weight Sharing (Trained Quantization)

![Screenshot 2020-03-28 at 2.05.42 PM.png](/assets/blog_resources/4371287C483FCFF7E5B44298A72A6CEE.png)

![Screenshot 2020-03-28 at 2.07.39 PM.png](/assets/blog_resources/BAAE9FE2788E5D92B0F36EBE124CF499.png)

![Screenshot 2020-03-28 at 2.07.53 PM.png](/assets/blog_resources/8708B2A3210FF989844BBA603B183CCE.png)

![Screenshot 2020-03-28 at 2.09.52 PM.png](/assets/blog_resources/11F1FA914A2D49F296CC422F954A5899.png)

![Screenshot 2020-03-28 at 2.09.59 PM.png](/assets/blog_resources/02D844554C8037890257D4E2E2EE36A8.png)

**Appendix 1.2:** Results of Quantization on Accuracy

![Screenshot 2020-03-28 at 2.24.12 PM.png](/assets/blog_resources/7268589BC8621A69D67789245696668A.png)

**Appendix 1.3:** Effects of Low Rank Approximations on Accuracy

![Screenshot 2020-03-28 at 2.29.59 PM.png](/assets/blog_resources/37970C24886F1F9F765A712BAC2BEC80.png)

For fully connected layers, the below paper proposes tobreak down one fully connected layer into lots of fully connected layers.

![Screenshot 2020-03-28 at 2.32.06 PM.png](/assets/blog_resources/20AEC28FCCE3B683287C8D6E3EFE3342.png)

**Appendix 1.4:** Binary /Ternary Weights at Inference Slides

![Screenshot 2020-03-28 at 2.36.09 PM.png](/assets/blog_resources/2ED6D1AB6C3BCA2F3B276D5FCABCA301.png)

![Screenshot 2020-03-28 at 2.36.16 PM.png](/assets/blog_resources/E6F5FEE5E97A4788761C1FEEA2FACE80.png)

![Screenshot 2020-03-28 at 2.36.25 PM.png](/assets/blog_resources/912626CA48928E81E6EA674B7E299418.png)

**Appendix 1.5 Speedup from Winograd Convolutions**

More details about the Winograd Convolution: <https://www.youtube.com/watch?v=Xh2hBMUYKAE>.

![Screenshot 2020-03-28 at 2.46.01 PM.png](/assets/blog_resources/B19FCBF5977A4269C7E0E1DE9BF8A71D.png)**
**

![Screenshot 2020-03-28 at 2.42.47 PM.png](/assets/blog_resources/68BD10ED5F0676CE419D3FE595E329BB.png)



**Appendix 2\. Optimal Hardware for Efficient Inference****
**

2.1 Google TPU

![Screenshot 2020-03-28 at 2.56.55 PM.png](/assets/blog_resources/29859D8C7BA2404FF33583C3F7BCC02C.png)

This is Google’s NN workload in 2017 - 61% is dedicated to running Multi-layer perceptrons (for ads). LSTM (for Google langauge services) are 29%.

![Screenshot 2020-03-28 at 2.57.01 PM.png](/assets/blog_resources/E2E9C36B35D5CF62A43078712D057CD9.png)

**Performacne Bottlenecks**:

In the left (sloped) region, you are bottle-necked by memory intensity.

X axis is ratio between FLOPs and memory bandwith overhead (FLOPS/byte of memory, listed as TPU Opt/Weight Byte in above table)

Y axis is the actual attainable peak performance.

If you’re only doing small operations, you are bottlenecked by memory bandwith, not arithmetic speed. Most NN do not have full utilization!

![Screenshot 2020-03-28 at 3.00.50 PM.png](/assets/blog_resources/936033707A22F0719D6C41B6B33E803D.png)

![Screenshot 2020-03-28 at 3.00.55 PM.png](/assets/blog_resources/0B665E15138650B7401DD9CC5B378022.png)

CNNs saturate the peak performance of TPU, but MLP and LSTM only use ~10% of potential throughput, because you cannot batch for real-time use cases.

![Screenshot 2020-03-28 at 3.01.01 PM.png](/assets/blog_resources/37DCC0187B2A7C5814453BDAE5795857.png)

![Screenshot 2020-03-28 at 3.03.37 PM.png](/assets/blog_resources/52834A2E7F0D217068B7BF904BE3BA51.png)

=\> Intro to EIE.

![Screenshot 2020-03-28 at 3.22.59 PM.png](/assets/blog_resources/960A2FB962CE729021C804C2756F33D7.png)![Screenshot 2020-03-28 at 3.23.04 PM.png](/assets/blog_resources/D169E8155CBB11898A01462AE1B9245B.png)

**
**

**Appendix N: Misc**

Where is time and energy consumed by large models? **in the memory access**

To make deep learning more efficient, we should design hardware that improves memory access efficiency.

![Screenshot 2020-03-18 at 4.12.52 PM.png](/assets/blog_resources/93F18E598158D0EF8D82237BC77F4577.png)

**About Hardware:**

![Screenshot 2020-03-18 at 4.13.17 PM.png](/assets/blog_resources/081296164A6AA37EF1E86563192A88EC.png)

![Screenshot 2020-03-18 at 4.14.06 PM.png](/assets/blog_resources/099940F88BD38CE08012992310FA67D7.png)

![Screenshot 2020-03-28 at 2.18.33 PM.png](/assets/blog_resources/61ACB92D05426B03DDE6917C6E3B6505.png)

Takeaway: use small representations for your numbers!

**About SqeezeNet:**

**Han** et al put out SqueezeNet. They used 1) Squeeze (1x1 conv) layers to reduce feature depth before convolution branches, 2) no fully connected layers. Everything is fully convolutional and the last layer is global average pooling. SqueezeNet is 50x smaller than AlexNet in its basic form and 510x smaller than AlexNet in compressed form.** **Model inference is ~3x faster on CPU and GPU.




**CNN History - **

**
**

**AlexNet**

![IMG\_0001.PNG](/assets/blog_resources/0DB08D7046462D6BD9AD636F7212E9F8.png)

![IMG\_0002.PNG](/assets/blog_resources/6CD19C6D474C41708F5DECC5DFF9AA2A.png)

![IMG\_0003.PNG](/assets/blog_resources/0C620FA2AF842D2D77FA1E4A1912FCDA.png)

![IMG\_0005.PNG](/assets/blog_resources/86054C019012CFBB74BFE54CC3E10A5C.png)

![IMG\_0006.PNG](/assets/blog_resources/5FA93B280D55160E66494DD6083C3237.png)

![IMG\_0007.PNG](/assets/blog_resources/372F2B21885FBB738E343550F9C1741B.png)

**ZFNet**

Basically just improving hypterparameters over AlexNet (changing stride size, changing the number of filters). Not really worth knowing.

**VGG**

Smaller Filters, Deeper networks - They essentially used 3x3 conv layers for everything, and tried to see if they could stack it deeper.

All their Pools were 2x2 MAX POOL stride 2, they did

 ((3x3 conv) x2 + Pool) x 3 + ((3x3 conv) x (3 or 4) + Pool) x2 + FC x3

The number of filters doubles in each group of CONV layers.

Note: 3x3 conv stacked twice has same receptive field as 7x7 single conv layer. However, it has fewer parameters (3 \* 3^2C^2 vs 7^2C^2) 

The reason why as you go deeper into the network the number of filters increases (you don’t have to do this) is because people want to keep a constant amount of comute, after downpooling you perform less compute so you can use more filters to keep it about constant, but you don’t have to do this.

![IMG\_0011.PNG](/assets/blog_resources/259BC15FCB49F920208DA2B0091AE965.png)

![Screenshot 2020-02-28 at 6.37.11 PM.png](/assets/blog_resources/31CAB09FB06D682342F616E54A6CA976.png)

Uses a lot of memory! ~100MB / image \*2\. Total params - 138M params!

![IMG\_0014.PNG](/assets/blog_resources/8563050998F3E69FC2856E561667AE99.png)

**
**

**GoogLeNet** 

Really looked at computational efficiency. Tried to design an architecture that was computationally efficient. 

They saved a lot of parameters by using no fully connected layers, and an efficient “inception” module. - only 5 million parameters! 1/20 the amount of VGG.

![IMG\_0015.PNG](/assets/blog_resources/DAF3C404B99E4ABE84D08BFFC6CEFEFB.png)
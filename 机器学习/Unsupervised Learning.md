---
title: Unsupervised Learning
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


### Unsupervised Learning

![](./images/1582162524374.png)
```
化繁为简：假如有很多种 input，找一个 function，把像树的 image，output 抽象的树。本来比较复杂的 input，变成简单的 output。
无中生有：找一个 function，随机给一个 input，
```

#### Clustering

![](./images/1582162571449.png)
```
有一大堆的 image，将 image 分成一类一类的。本来有些不同的 image，都用同一个 class 来表示，做到了化繁为简。
K-means：有一大堆的 data，都是 unlabell，{x1....,xn...xN}，每一个 image x 都代表着一张 image，把它做成 K 个 clusters。
怎么做呢？
先找 cluster 的 center，假设每一个 object 都要 vector 表示。长度是一样的，有 K 个 clusters，就有 K 个 center。
初始的 cluster 从哪儿来？
从 training data 里面 random K 个 object 出来。就是 K 个 center。

对所有在 training data 里面的 x，决定说现在的每一个 object 属于 1~K 的哪一个 cluster。
```
>**Hierarchical Agglomerative Clustering(HAC)**

![](./images/1582031330335.png)
![](./images/1582031358392.png)
![](./images/1582028002497.png)
```
step 1: 先建一个 tree

假设现在有 5 个 example，先做一个 tree structure，把这 5 个 example 两两算相似度，然后挑最相似的 pair 出来，然后把他们 merge 起来，平均起来，得到一个新的 vector，这个 vector 同时代表第一个和第二个 example。

step 2: pick a threshold

根据切割位置不同，相似度的个数也不同。
```

#### Distributed Representation

![](./images/1582028055198.png)
```
光只做 cluster，是非常卡的，在做 cluster 的时候，就是以偏概全，因为每一个 object 都必须要属于某一个 cluster。应该用一个 vector 表示 object。这个 vector 里每一个 dimension 就代表了某一种特质。某一种 attribute。这件事情就叫做 Distributed Representation。
如果原来的 object 是一个 high dimensional 的东西，现在用 attribute 来描述，它就从比较高维的空间变成比较低维的空间。这件事情就叫做 Dimension Reduction。
一样的事情，不一样的称呼。
```

#### Dimension Reduction

![](./images/1582188155072.png)
```
在 2D 的空间就可以描述 3D 的 information，简化了问题。
```
![](./images/1582028130449.png)
```
In MNIST,a digit is 28×28 dims.
Most 28×28 dim vectors are not digits
根据角度变化知到 3 在 28x28 dims 的样子。
```
![](./images/1582028165719.png)
```
怎么做 Dimension Reduction？
找到一个 function，input x，output z，
The dimension of z would be smaller than x。
在 Dimension Reduction 里面的方法
方法 1：Feature selection
	把 data 的分布放在二维平面上，根据集中的 x2 的 dimension 位置。select x2，不见得总是有用。
方法 2：Principle component analysis(PCA)
	function 是一个简单的 linear function，input x 和 output z 之间的关系就是 linear transform，z = Wx，(W 是 matrix)，根据一大堆的 x 把 W 找出来。
```

#### PCA

![](./images/1582028221783.png)

![](./images/1582028314628.png)

![](./images/1582028371584.png)

![](./images/1582028447401.png)

![](./images/1582162733527.png)

![](./images/1582162758868.png)

>**PCA - decorrelation**

![](./images/1582162796595.png)


#### PCA - Aother Point of View

![](./images/1582183523618.png)
```
这些数字是由一些 basic component 组成的，这些 basic component 可能代表着笔画，把 basic component 加起来就可以得到一个数字。
basic component 写作成 (u1,u2,u3,u4,u5)，就是一个一个的 vector，也就是  28x28 dimension 的 vector。
formulation 表示 x ≈ c1u1 + c2u2 + ··· + cKuK + x拔 (u：component)
x 代表了某一张 image 的 pixel
x拔 所有 image 的平均
每一张 image 就是一堆 component 的 linear convolution，再加上 image 的平均组成的。
可以用 (c1,c2,···,cK) 表示一张 image，假设 component 的数目远比 pixel 的数目少的话，就可以用这些 component 的 weight 来描述这张 image。
```
![](./images/1582183559677.png)
```
x 等于一堆 component 的 linear convolution 再加上 平均。
这些 linear convolution 的结果写作 x^
现在不知道 (u1,u2,···,uK) 的 vector 是什么，怎么找 K 个 vector 出来呢？
要做的事情是找 K 个 vector 使得 x^ 和 (x - x拔) 越接近越好。它们中间的差没有办法用 component 来描述的部分叫做 reconstruction error。
find K 个 vector 可以 minimize 这个个 error。
```
![](./images/1582183601153.png)
```
minimize matrix 之间的 error。
```
![](./images/1582183637997.png)
```
线代中的拆分
```
![](./images/1582184758324.png)
![](./images/1582183706525.png)
```
cK 是下标。
PCA 可以表示成一个 NN，这个 NN 只有一个 hidden layer，这个 hidden layer 是一个 linear activation function。
```

#### Weakness of PCA

![](./images/1582185045044.png)
```
· unsupervised
	给它一大堆点，没有 label，对 PCA 来说，假设把 project 到一维上，PCA 会找一个可以让 data variance 最大的 dimension。
· linear
	PCA 做 dimension reduction 是无法把曲面拉直。
```
>**PCA - Pokemon**

![](./images/1582185088114.png)
```
计算每一个 principle component 的 λ。
```
![](./images/1582185119668.png)
```
每一行都代表 宝可梦 的强度。
```
![](./images/1582185149448.png)

>**PCA - MNIST**

![](./images/1582186132606.png)
```
可以把每一张数字都拆成 component weight * component，每一个 component 都是一张 image，都是 28x28 dimension 的 vector。
用这些 component 做 linear convolution 就可以得到所有的 digit。
这些 component 就叫做 Eigen-digit(特征数)。
这些 component 都是 covariance matrix 的 eigen vector，Eigen-digit 做 linear convolution 就可以得到各种不同的 digit。
```

>**PCA - Face**

![](./images/1582186158003.png)
```
找 face component，叫做 Eigen-face。
把这些 face 做 linear convolution 就可以得到所有的 face。
PCA 找出的是 component，把很多 component linear convolution 之后，它会变成一个 digit，一个 face。
但现在几乎找出的都是完整的脸。
```

>**What happens to PCA?**

![](./images/1582186199692.png)
```
在 PCA 里面，linear convolution 的 weight 可以任何值，可正可负。当我们用 principal component 组成一张 image 的时候，可以把这些 component 相加或者相减。这回导致找出的一个 component 不见得是一个图的 basic component。
可以先画一张复杂的图，再把多余的剪掉。
如果想要得到类似笔画的东西，要用另外一种技术叫做 Non-negative matrix factorization(NMF)。
PCA 可以看作是对 matrix x 做 SED(矩阵分解技术)，分解出来的两个值可以是正的，可以是负的。
NMF 会强迫所有的 component 的 weight 都是正的。都是正的好处是现在一张 image 必须由 component 叠加得到。会让每一个 dimension 都是正的。
```

>**NMF on MNIST**

![](./images/1582186221943.png)
```
清晰很多。
```

>**NMF on Face**

![](./images/1582186257644.png)
```
比较像脸的一部分，而不是整张脸。
```


### Matrix Factorization
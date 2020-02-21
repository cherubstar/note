---
title: Unsupervised Learning
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

## Unsupervised Learning: Linear Methods

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

![](./images/1582188229465.png)
```
有时候会有两种 object，他们之间受到某种共同的 latent factor 操控的。
在这个 matrix 上面每一个 table 的 block 并不是随机出现。
每个人跟每个角色之间有共性，有一些共同的 factor 操控。
```
![](./images/1582188264592.png)
```
每一个角色都是平面上的一个点，每一个角色都可以用一个 vector 来描述。
如果某一个人的属性跟某一个角色所具有的属性是 match 的话，就会买对应的公仔。
```
![](./images/1582188293211.png)
```
无法观察一个的属性，没有办法直接知道每一个动漫任务的属性。
```
![](./images/1582188335785.png)
```
怎么凭这个关系来推论每一个人跟动漫人物的 latent factor。
```
![](./images/1582188363489.png)
```
有一些 information 是不知道的。
```
![](./images/1582188410296.png)
![](./images/1582188437164.png)

#### More about Matrix Factorization

![](./images/1582188469111.png)
```
要做的更精确的话，加上
bA：otakus 有多喜欢买公仔
b1：角色有多流行
这些跟属性无关。
```
#### Matrix Factorization for Topic analysis

![](./images/1582188503890.png)

#### More Related Approaches Not Introuced

![](./images/1582188535355.png)


## Unsupervised Learning: Word Embedding

```
Word Embedding 是 Dimension Reduction 中的非常好的应用
```
![](./images/1582202398358.png)
```
1-of-N Encoding 是最 difficult 的做法表示一个 word。
这个 vector 的 dimension 就是 word 可能有的数目。
建一个 word class，把不同的 words 有同样的性质的 words class 成一群一群的。然后就用那个 class 所属的 word 表示这个 words。
需要一个 word embedding，把每一个 word 都 project 到 high dimensional  space 上面，
```

### Word Embedding

![](./images/1582202430217.png)
```
怎么做 Word Embedding 呢？
Word Embedding 是 unsupervised problem。
怎么让 machine 知道每一个词汇的含义呢？
让 machine 阅读大量的文章。
```
![](./images/1582202464654.png)
```
learn 一个 NN，找一个 function，input 一个词汇(比如：apple)，output 是那个词汇对应的 word embedding。
现在只有 input，没有 output，不知道 word embedding 的样子。只有单向。
不能用 auto-encoder。
```
![](./images/1582202488113.png)
```
根据 context (上下文)。
```

#### How to exploit the context?

![](./images/1582202520196.png)
```
有两个方法
· Count based
如果有两个词汇 Wi,Wj 常常在同一篇文章内出现，它们的 word vector 用 V(Wi),V(Wj) 表示，如果 Wi,Wj 常常一起出现的话，V(Wi),V(Wj) 会比较接近。
假设已知 V(Wi),V(Wj)，计算 V(Wi),V(Wj) 的 inner product。
假设 Nij 是 Wi,Wj 在 same document 出现的次数。
V(Wi),V(Wj) 和 Nij 越接近越好。
```

#### Prediction based

![](./images/1582202556018.png)
```
learn 一个 NN，做的事情是 prediction
假设有一个 sentence，Given 前一个 word，predict 下一个可能出现的 word。
input 是 1-of-N Encoding 的 vector，output 是下一个 word wi 是某一个 word 的几率。
把第一个 hidden layer 的 input z 拿出来，input 不同的 1-of-N Encoding，z 就会不一样。z 来代表词汇。
```
![](./images/1582202601172.png)
```
这两个不同的词汇必须把它们 project 到(必须要通过参数 weight 转换以后)同样的空间，这样在 output 的时候才可能有同样的几率。
所以当 learn 一个 prediction model 的时候，考虑 word 的 context 这件事情，就自动的被考虑在 prediction model 里面，所以把 prediction model 的 hidden layer 拿出来，就可以得到我们想要找的这种 word embedding 的特性。
```

#### Prediction-based — Sharing Paramters

![](./images/1582202640170.png)
```
如果只用 wi-1 去 predict wi，有点弱，比较难。
可以拓展这个问题，可以拓展 N 个词汇在 predict 之前。
wi-1 和 wi 的第一个 dimension 和 第一个 hidden layer 的 neuron 使用的 weight 是同样的。为什么这样做呢？
1、如果不这么做，如果把同一个 word 放在不同的位置，通过 transform 之后，得到的 embedding 就会不一样。
2、减少了参数的使用量。
```
![](./images/1582202678044.png)
```
用 formulation 表示
xi-1 和 x-2 的 length 都是 |V|
z 的 length 是 |Z|
z = W1*xi-2 + W2*xi-1
W1 和 W2 的 weight matrix 都是 |Z|*|V| 的
强制让 W1 = W1 = W，等于一摸一样的 matrix。
要得到一个 word vector 的时候，就把一个 1-of-N Encoding 乘以 W，就可以的那个 word 的 word embedding。
```
![](./images/1582202738278.png)
```
怎么让 weight 的参数一样呢？
需要 Given wi and wj the same initialization，一样的初始值。
做偏微分，减去同样的 wi 和 wj 的偏微分
```

#### Prediction-based — Training

![](./images/1582202770028.png)
```
这个 NN training 完全是 unsupervised，collect 一堆文字 data。
希望 output 跟 '就' 的 1-of-N Encoding 的 cross-entropy 越小越好。
```

#### Prediction-based — Various Architectures

![](./images/1582202820586.png)
```
· Continuous bag of word(CBOW) model
拿 wi-1 和 wi+1 来 predict wi。
· Skip-gram
拿 wi 来 predict wi-1 和 wi+1。
这个 NN 不是 Deep 的，其实是一个 linear hidden layer。
```
![](./images/1582202853115.png)
```
word vector 有一些有趣的特性，如果把同样类型的 word vector 摆在一起，会有种固定的关系。
```
![](./images/1582202884580.png)
```
如果某一个东西属于另外一个东西的话，把它们的 word vector 相减，它们的结果会是很类似的。
```
![](./images/1582202916566.png)
```
用 word vector 可以做一些简单的推论
· Characteristics
· Solving analogies
```
#### Multi-lingual Embedding

![](./images/1582202951439.png)
```
可以把不同语言的 word vector 拉在一起。
```

>**Multi-domain Embedding**

![](./images/1582202990870.png)
```
可以对 image 做 embedding，先找到了一组 word vector，接下来 learn 一个 model，input 一张 image，output 是跟 word vector 一样 dimension 的 vector。
```

#### Document Embedding

![](./images/1582203020029.png)

>**Semantic Embedding**

![](./images/1582203042604.png)
```
怎么把 document 变成一个 vector？
把一个 document 变成 Bag-of-word，用 auto-encoding learn 出这个 document 的 Semantic Embedding。
只用 Bag-of-word 描述一篇 document 是不够的。
```

>**Beyond Bag of Word**

![](./images/1582203070244.png)
```
只用 Bag-of-word 描述一篇 document 是不够的。
因为词汇的顺序代表了很重要的含义。

1.white blood cells destroying an infection
2.an infection destroying white blood cells
它们的 Bag-of-word 是一样的，但顺序不一样，语义就不一样了。
```
![](./images/1582203106719.png)

## Unsupervised Learning: Neighbor Embedding

### Maniford Learning

![](./images/1582212612983.png)
```
非线性的降维
data point 可能是在高维空间里面的 manifold。
Maniford Learning 要做的事情是把 S 形的曲面展开。
把塞在高位空间中的低维空间摊平，摊平的好处是，现在把低维空间摊平以后，然后做降维，就可以在 manifold 上面用 equipment distance 来计算点跟点之间的距离。对 supervised learning 有帮助。
```

#### Locally Linear Embedding(LLE)

![](./images/1582212641778.png)
```
在原来的空间里面，点的分布如图所示，某一个点是 xi，先选出 xi 的neighbor xj，接下来找 xi 和 xj 的关系，写作 wij。
wij 是怎么找出来的呢？
假设说每一个 xi 都可以用它的 neighbor 做 linear convolution 以后组合而成。wij 就是 xj 组合 xi 的时候的  linear convolution 的 weight。
找一组 wij，这组 wij 对 xi 所有 neighbor xj 做 weight-sun 的时候，跟 xi 越接近越好。
然后就做 dimension reduction。把原来所有的 xi 和 xj 转成 zi 和 zj。中间的关系 wij 是不变的。
```
![](./images/1582214198274.png)
![](./images/1582212682180.png)
```
wij 在原来的 space 上面找完之后就不再改变了。
接下来为每一个 xi,xj 找另外一个 vector，因为要做 dimension reduction，新的 vector 要比原来的 dimension 还要小。
zi,zj 是另外的 vector。
wij 是已知的，找一组 z，让 zj 透过 wij 做 weight-sun 以后，可以和 zi 越接近越好。
```
![](./images/1582212710442.png)
```
K 太小，结果不太好
K 太大，结果不太好
```

#### Laplacian Eigenmaps

![](./images/1582212731316.png)
```
想要比较两个点的距离，算之间的 equipment distance 是不够的，要看的是在这个 high density region 之间的 distance。如果两个点之间有 high density 的 collection，那才是真正的接近。可以用 graph 描述这个事情。
可以把 data point construct 一个 graph，计算 data point 两两之间的相似度。
```
![](./images/1582212762978.png)
```
wij：两个 dat point xi,xj 在图上是相连的，那 wij 就是 xi,xj 的相似程度，如果不相连，wij = 0。
```
![](./images/1582212803328.png)
```
可以 apply 在 unsupervised task 上面。
```
### T-distributed Stochastic Neighbor Embedding(t-SNE)

![](./images/1582212832926.png)
```
前面的问题是假设相近的点应该要是接近的，但没有假设说不相近的点没有要接近。没有假设说不相近的点要分开。
```
![](./images/1582212885351.png)
```
把原来 data point x 变成 low dimension vector z。
在原来的 x space 上面，会计算所有的点的 pair (xi,xj) 之间的 similarity，写成 S(xi,xj)。接下来会做 normalization。
会计算一个 P(xj|xi)，分子是 S(xi,xj)，分母是 summation 所有的除 xi 以外所有其他的点的 S(xi,xk)。
假设已经找到了 low dimension representation z。把 x 变成 z 的话，也可以计算 (zi,zj) 的 similarity。
做 normalization 是有必要的，因为不知道在高维空间中算出来的距离 S(xi.xj) 和 S'(zi,zj) 的 skill 是不是一样。如果有做 normalization，最后就可以把 P(xj|xi) 和 Q(zj|zi) 变成几率，这个时候它们的值就会介于(0,1)之间，skill 就会一样。

接下来要做的事情就是现在还不知道 (zi,zj) 的值是多少，希望找一组 (zi,zj)，可以让 distribution P(xj|xi) 和 Q(zj|zi) 越接近越好。
怎么衡量两个 distribution 的 similarity 呢？
KL divergence。
要做的事情就是找一组 z，它可以做到 xi 对其他 point 的 distribution 和 zi 对其他 point 的 distribution 之间的 KL divergence 越小越好。
```

#### t-SNE — Similarity Measure

![](./images/1582212923605.png)
```
在原来的 data point 上面，similarity 的选择是 exp(-||xi - xj||2)，evaluate similarity 的方式是计算 xi,xj 的 equipment distance，取负号，在取 expectation，因为它可以确保说非常相近的点，只要距离一拉开，similarity 就会变的很小。
在 t-SNE 之前，有一个 function 叫做 SNE。
在 data point 里面 space 上面 evaluatation 的 manager，在新的 space 上面用同样的 manager 就可以了。
t-SNE 在 dimension reduction 以后的 space，选的 manager 是不一样的。

假设橙色线上的点是原来的点，做 dimension reduction 以后，要怎么维持原来的 space 呢？
变成在同水平的蓝色线上的两个点。
如果要维持原来的几率的话，就把它们变成图片所示。
如果本来距离比较近，它们的影响是比较小的，如果本来就有一段距离，从原来的 distribution 变成 t distribution 以后，会被拉的很远。
```
![](./images/1582278017538.png)
```
会把 data point 聚集成一群一群的，如果 data point 本来只要有一个 gap，做完 t-SNE 以后，就会把那个 gap 强化，gap 就会变的特别明显。 
```

## Unsupervised Learning: Deep Auto-encoder


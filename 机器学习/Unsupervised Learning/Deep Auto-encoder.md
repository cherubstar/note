---
title: Deep Auto-encoder
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


### Auto-encoder

![](./images/1582292392376.png)
```
首先找一个 encoder，input (比如影响辨识) 28x28=784 dimension 的 digit，这个 encoder 可能就是一个 NN，它的 output 就是一个 code，code 要远比 784 维要小的。类似有压缩的效果。这个 code 代表了原来 input 某种 compact，某种精简的，有效的 representation。
现在做的 unsupervised learning，可以找到一大堆的 image 当成 NN encoder 的 input，但并不知道 code 样子。
learn 一个 NN，需要 input 和 output，只有 input，没有办法 learn 它。
那需要一个 decoder，input 一个 code，output 就是根据 code 的 information 的 image。
encoder 和 decoder 单独都无法 train，可以接起来一起 train。
```
### Starting from PCA

![](./images/1582292423428.png)
```
component weight 就是 hidden layer，在 PCA 里面它是 linear，中间的 hidden layer 又叫做 Bottleneck later，因为 component 的数目通常要比 input dimension 的数目小的多。进而达到 dimension reduction 的效果。

hidden layer 的 output 就是要找的 code。
```
### Deep Auto-encoder

![](./images/1582292634891.png)
```
把 input 做 encoder 变成 bottle 达到 output，再把 bottle 达到 output 做 decoder 变回原来的 image。
```
![](./images/1582292756698.png)
```
中间 component dimension 是 30 维的。
使用 Deep Auto-encoder 的效果比较好。
```
![](./images/1582292782233.png)
```
中间 component dimension 是 2 维的。
```
![](./images/1582292932045.png)
```
pass
```

### Auto-encoder — Text Retrieval

![](./images/1582292970994.png)
```
可以用在文字处理上，把一篇文章压成一个 code。
要做文字的搜寻。
· Vector Space Model
把每一篇文章都表示成空间中的一个 vector，就是空间中的一个点。
使用者 input 一个查询的词汇，把查询的词汇也变成空间中的一个点。
接下来就是计算输入的查询词汇和每一篇 document 它们之间的 inner product 或者 cosine similarity。如果距离最近，cosine similarity 的结果最大。

怎么把 document 变成一个 vector 呢？
· Bag-of-word
如果有 10万 个词汇，这个 vector 的 size 就有 10万 维。
```
![](./images/1582292995594.png)
```
用 Auto-encoder 把语义考虑进来，input 一个是吧 document 变成的 vector，通过一个 encoder，把它压成 2 维。
如果要做搜寻的时候，input 一个词汇，把 query 通过 encoder，把它变成 2 维的 vector，假设 query 落在某个位置上，就可以知道 query 是跟 Energy markets 有关。
用 LSA 的话得不到类似的结果。
```

### Auto-encoder — Similar Image Search

![](./images/1582293016847.png)
```
Auto-encoder 用在 image 的搜寻方面，以图找图。
假设第一张图就是要找到对象，image query，计算 image query 跟其他 database 里面的 image 的 similarity，在 pixel 的相似程度。
但得不到太好的结果。
```
![](./images/1582293043323.png)
```
把每一张 image 变成一个 code，然后在 code 上面做搜寻，做这件事情是 unsupervised 的，collect 多少 data 都可以。

32x32 dimension 的 image 用 256 dimension 的 vector 来描述它。
再把这个 code 通过另外一个 decoder，变回原来的 image。
```
![](./images/1582293071207.png)
```
如果不是在 pixel 算相似度，而是在 code 算相似度的话，会得到比较好的结果。
```

### Auto-encoder — Pre-training DNN

![](./images/1582293121124.png)
```
有没有好的方法找到一组好的 initialization，找比较好的 initialization 的 function 就叫做 pre-training。
如果 hidden layer 要比 input 还要大的时候，要加一个很强的 regularization
先 learn 一个 auto-encoder，learn 好之后，把从 784维 到 1000维 的 weight W1 保留下里，把它 fix 住。
```
![](./images/1582293152039.png)
```
接下来就把所有 database 里面的 digit 通通变成 1000维 的 vector，再 learn 另外一个 auto-encoder，把 1000维 的 vector 变成 1000维 的 code，再把 1000维 的 code 转回 1000维 的 vector。再把 W2 保留下来，fix 住。
```
![](./images/1582299402620.png)
```
保留 W3。
```
![](./images/1582293193654.png)
```
W1,W2,W3 就等于是在 learn NN 的时候的 initialization，最后在 random initial 500 到 10 的 weight。再用 backpropagation 调一边。
```
![](./images/1582293249363.png)
```
有一种方法可以让 auto-encoder 做的更好。
· De-noising auto-encoder
把原来的 input x 加上一些 noise 变成 x'，然后把 x' encoder 以后变成 code c，再把 c decoder 回来，变成 y。
在做一般的 auto-encoder 时候，希望 input 和 output 越接近越好
现在在 De-noising auto-encoder，是要让 output 跟原来的 input(在加了 noise 之前的 input) 越接近越好。
这样 learn 出来的结果是比较 robust。
· Contractive auto-encoder
在 learn code 的时候加上一个 constraint，这个 constraint 在当 input 有变化的时候，对 code 的影响是被 minimize 的。
```

>**Learn More**

![](./images/1582293784774.png)
![](./images/1582293805134.png)

### Auto-encoder for CNN

![](./images/1582293832888.png)
```
如果 encoder 的部分是在做 Convolution 和 Pooling
那 decoder 的部分是在做 Deconvolution 和 Unpooling
```

#### CNN - Unpooling

![](./images/1582293871995.png)
```
在做 Pooling 的时候，image 就变成原来的 1/4
做 Unpooling 的时候，会做另一件事情，会记得刚刚在做 Pooling 的时候是从哪里取值的。从哪里取值，那个地方会是白色的。反之是黑色的。
把比较小的 matrix 变成原来的 4 倍。记录 Pooling 的位置就派上用场。
```

#### CNN - Deconvolution

![](./images/1582293908222.png)
```
Deconvolution 就是 Convolution
这是一维的 Convolution
input 有 5 个 dimension，fiter size 是 3。
```

### Sequence-to-sequence Auto-encoder

![](./images/1582293924719.png)
```
pass
```

>**Next ...** 

![](./images/1582293959460.png)
![](./images/1582293985743.png)
---
title: Unsupervised Conditional Generation
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581604679444.png)
```
Conditional Generation 可以是 supervised 的，也可以是 unsupervised。
假设有一个 domain x 是 real image，domain y 是梵高的画作，可以 learn 一个 generator，给它一张 real image，它 output image 看起来像梵高的画作。在 training 的时候，不需要 label data。
可以做语音，影像等。
```
![](./images/1581604837140.png)
```
1、Direct Transformation
input 和 output 不能差太多，这个 generator，给它一个 input，output只能小改。可以做图片画风转换。
2、Projection to Common Space
如果要转的 input 和 output 差距很大。比如把真人图片转化成动画人物，先 learn 一个 encoder，input 真人 image 给 encoder，encoder 将 image 的特征抽取出来。
```

### Direct Transformation

![](./images/1582461960903.png)
```
没有 domain x 和 domain y 的 link，generator 怎么知道产生 domain y 的 image？
需要一个 y domain 的 discriminator，这个 discriminator 看过很多 y domain 的 image，所以给它一张 image，它可以鉴别是 x domain image 还是 y domain image。
generator 要做的事情是骗过 discriminator，那 generator 产生出来的 iamge 就会像是 y domain image。
问题：generator 可以产生一张像梵高的画作，但可以产生和 input 无关的东西。
所以 generator 不只是骗过 discriminator 就好，同时 generator 的 input 和 output 有一定程度的关系。
```

![](./images/1582461739472.png)
```
1、第一个方法就是不管它，直接 train 下去。
如果 generator 比较 shallow，input 和 output 特别像
如果 generator 很深，就可以让 input 和 output 不一样，那就需要做额外的处理，免得让 input 和 output 变得完全不一样。
```
![](./images/1582461700017.png)
```
2、第二个方法就是拿一个 pre-trained 好的 network，比如 VGG，然后把 generator 的 input 和 output 都给 pre-trained 好的 network，output 出 embedding。
在 train 的时候，generator 一方面是想要骗过 discriminator，让 output 的 image 看起来像梵高的画作，同时 generator 还有另一个任务，希望 pre-trained 的 model 它的 embedding output 不要差太多。
好处：如果 embedding 不会差太多，generator 的 input 和 output 就不会差太多。
```
![](./images/1582461635564.png)
```
3、第三个方法就是 CycleGAN，在 CycleGAN 里面要 train 一个 x domain 到 有domain 的 generator，同时 train 一个 y domain 到 x domain 的 generator。
目的：input 一张风景 image，第一个 generator 将它转成 y domain 的图，第二个 generator 将 y domain 的图还原成原来一摸一样的图。现在除了要让 generator 骗过 discriminator 以外，同时要让 input 和 output 越像越好。
```
![](./images/1582462074675.png)
```
CycleGAN 可以做双向的，再 train 另外一个 GAN，将 y domain 的图转成 x domain 的图，同时需要 discriminator 确保这个 generator 产生的图像是 x domain 的图，接下来再把 x domain 的图转回 y domainn 的图。一样希望 input 和 output 越接近越好。
```

>**Issue of Cycle Consistency**

![](./images/1582461526143.png)
```
CycleGAN 存在一些弊端
```
![](./images/1581606337427.png)

>**StarGAN**

![](./images/1581606389332.png)
```
多个 domain 互转，StarGAN 只 learn 了一个 generator，在多个 domain 间的互转。
```
![](./images/1581606512788.png)
```
learn 一个 discriminator，这个 discriminator 会做两件事
1、首先事给 discriminator 一张 image，discriminator 鉴别这张 image 是 real 还是 fake 的。
2、鉴别这张 image 来自哪一个 domain。
在 StarGAN 中只需要 learn 一个 generator 就好，这个 generator 的 input 是一张 image 和目标 domain，就会把新的 image 生成出来，接下来把 iamge 丢给同一个 generator，然后告诉 generator 现在原来的 image 是哪一个 domain，再用 generator 合回原来的图片。经过两次转换后还原成原来的 image。
```
>**StarGAN - Example**

![](./images/1582462156314.png)
![](./images/1582462206394.png)


### Projection to Common Space

![](./images/1582462680327.png)
![](./images/1582462324877.png)
```
把 input object 投影到 latent space，再用 decoder 把它合回来。
如何在真人 domain x 和 动画 domain y 相互转化呢？
需要一个 x domain 的 encoder，将真人头像的 feature 抽取出来
需要一个 y domain 的 encoder，将动画头像的 feature 抽取出来
x domain 的 encoder 回抽出它的 latent vector
y domain 的 encoder 回抽出它的 latent vector
如果丢进 x domain 的 decoder，就会产生真实人物的人脸
如果丢进 y domain 的 decoder，就会产生动画人物的人脸

希望这样做：
给它一张真实人物的 image，透过 x doamin 的 encoder，抽出 latent representation，这个 latent representation 是一个 vector，期待这个 vector 的每一个 dimension 就代表了 input 这张 image 的某种特征，接下来由 y domain 的 decoder input 这个 vector，根据这个 vector 表示的人脸的特征，得出一张 y domain 的图。
```
![](./images/1582462368661.png)
```
但是这是一个 unsupervised problem，
x domain 的 encoder 和 decoder 组成一个 Auto-encoder
y domain 的 encoder 和 decoder 组成一个 Auto-encoder
```
![](./images/1581607111148.png)
```
这样造成的问题是这两个 encoder 和 decoder 没有任何关联。
可以加一个 discriminator 进来。
encoder、decoder、discriminator 就是 VAE-GAN

怎么解决这件事？
```
>**Projection to Common Space**

![](./images/1581607213171.png)
```
1、第一个方法
常见的解法是让不同 domain 的 encoder 和 decoder，它们的参数是 tai 在一起的
不同 domain 的 encoder，前面几个 hidden layer 参数是不一样的，后面几个 hidden layer 共用的
不同 domain 的 decoder，前面几个 hidden layer 参数是共用的，后面几个 hidden layer 不一样
```
![](./images/1582462810402.png)
```
2、第二个方法
加一个 discriminator，给 discriminator 的 latent vector，鉴别是来自 x domain 的 image 还是来自 y domain 的 image，两个 encoder 希望骗过 domain discriminator。
如果 domain discriminator 无法判断出这个 vector 来自哪一个 domain，意味着说两个 domain 的 image 变成 code 的时候，它们的 distribution 是一样的。就期待说同样的维度就代表了同样的意思。
```
![](./images/1581607452939.png)
```
3、第三个方法
Cycle Consistency
根据图片所示理解。
```
![](./images/1581607899917.png)
```
4、第四个方法
Semantic Consistency
将一张 image 通过 x domain 的 encoder，通过 y doomain 的 decoder，
```
>**Voice Conversion**

![](./images/1581608045654.png)
```
把 A 的声音转换成 B 的声音。
```
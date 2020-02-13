---
title: Generative Adversarial Network 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

## Introduction

![](./images/1581243846564.png)
![](./images/1581243938600.png)
![](./images/1581252934972.png)
![](./images/1581252986224.png)

### Basic Idea of GAN

![](./images/1581253244715.png)
```
在 Generation 里面需要做的就是训练出一个 generator，从 Gaussian distribution random sample 一个 vector，把这个 vector 丢到 generator 里面，generator 就要产生一张 image，丢不同的 vector 就会产生不同的 image。
Conditional Generation：条件生成
输入文字让机器产生对应的图片，输入图片，让机器产生另一张图片。
```
![](./images/1581254118780.png)
```
vector -> Generator -> image(high dimensioal vector)
通常输入的每一个 dimension 会对应到图片的某种特征，也就是说，改变了其中一个 dimension 的数值，产生图片的某种特征有所改变。
```
![](./images/1581254988025.png)
```
假设要产生图片，就输入 image，假设要产生句子，就输入 句子
输出是一个 scalar，表示产生出来的这张 image 的 quality，这个 scalar 越大，产生出来的 image 的 quality 越高，看起来越像是真是的 image，看起来越 realistic。
```

>**Generator v.s. Discriminator**

![](./images/1581255467531.png)
![](./images/1581255712268.png)
![](./images/1581255871980.png)
![](./images/1581255976268.png)
```
相互依赖
```
>**Algorithm**

![](./images/1581256325691.png)
![](./images/1581256837381.png)
```
Step 1:
	固定 generator G，update discriminator D。
	怎么调整 discriminator 的参数呢？
	如果是从 database 中 sample 出来的 image 就给高分，
	如果是 generator 生成的 image 就给低分。
Step 2:
	固定 discriminator D，update generator G。
	怎么调整 generator 的参数呢？
	把一个 vector 丢到 generator 里面，generator会产生一张图片，把这张图片丢到 discriminator 里面，discriminator 会给出一个分数。
	固定住 discriminator 的参数，只去调整 generator 的参数。
	把 generator 和 discriminator 和起来，一块运行。固定最后几个 hidden layer，只调前面几个 hidden layer，让 output 的值越大越好。使用 Gradient Ascent(就是 - Gradient Descent)
```
![](./images/1581257746836.png)
```

```

![](./images/1581259259455.png)
![](./images/1581259290628.png)
```
可以计算两个 vector 之间的内差。
```
### GAN as structured learning

![](./images/1581259408512.png)

>**Structured Learning**

![](./images/1581259500882.png)
![](./images/1581259685749.png)
![](./images/1581259864876.png)
```
Regression、Classification and so on.
Machine Translation
Speech Recognition
Chat-bot
```
![](./images/1581260120751.png)
![](./images/1581260291793.png)
```
如果机器需要解决 Structured Learning 的问题，必须有规划的概念，有大局观。在 Structured Learning 里面，真正重要的不是产生了什么 components，而是 component 和 component 之间的关系。
```

![](./images/1581260699378.png)
```
GAN 是 Structured Learning 的 solution。
Bottom Up: 自下而上
Top Down: 自上而下
```

### Can Generator learn by itself?

![](./images/1581260766215.png)
![](./images/1581261123213.png)
![](./images/1581261234170.png)
```
怎么产生 vector?
learn 一个 Encoder。
```
![](./images/1581261347897.png)
![](./images/1581261462713.png)
```
希望 input 和 output 越接近越好。
Decoder 就相当与 Generator。
```
![](./images/1581261569455.png)
![](./images/1581261961280.png)
![](./images/1581262113341.png)
```
遇到的问题可能是？
Training data 的 image 是有限的。所以对 0.5*a + 0.5*b 无法确定生成的结果。
```
![](./images/1581262325892.png)
```
Variational Auto-Encoder(VAE)
可以解决 0.5*a + 0.5*b 无法确定生成的结果的问题。
```

![](./images/1581262985524.png)
![](./images/1581263153751.png)
```
generator 的目标就是它的 output 跟某一张图片越像越好，什么叫做越像越好呢？
通常计算的是这两张图片 pixel 和 pixel 的差距。把这两张图片表示成两个 vector，算这两个 vector 的 Euclidean Distance(欧氏距离)。
真正在 training 的时候，generator 是会犯错的。在什么位置犯错就变得很重要了。
```
![](./images/1581263386003.png)
```
如果不想用 discriminator，想单纯的用 Auto-Encoder 做 Generator 这件事，假设有同样的 Network，一个用GAN train，一个用 Auto-Encoder train，往往用 GAN 的能产生图片，Auto-Encoder 则需要一个更大的 Network 才能产生和 GAN 相近的结果。
```
![](./images/1581263795478.png)
```
使用 Auto-Encoder 遇到的问题。
```

### Can Discriminator generate?

![](./images/1581263872770.png)
![](./images/1581264051942.png)
![](./images/1581264265862.png)
```
Discriminator 是可以产生 image，但非常卡。
在不同的领域中有不同的叫法。
Evaluation function、Potential function、Energy function
当产生完一张完整的图片以后，检查这张图片里面 component 和 component 之间的关系是比较容易的。
```
![](./images/1581264517685.png)
```
依赖 x~ = arg max D(x) 产生
假设已经有了一个 discriminator，这个 discriminator 可以实现鉴别好坏。
怎么使用 discriminator 做生成呢？
穷举所有可能的 x，一个一个丢到 discriminator 里面，看哪一个 x，discriminator 可以给它最高的分数，discriminator 给高分的 x 就是生成的结果。
```
![](./images/1581264635586.png)
![](./images/1581264747381.png)
```
当在训练 discriminator 的时候，给它很多好的 example，告诉它这是高分的，给它很多烂的的 example，告诉它这是低分的。
但是 discriminator 经常会有 positive example，只有正面的例子，会出现看的什么都会觉得是正面的，都会给高分。
有好的 Negative example 才能 train discriminator。
```
![](./images/1581264863259.png)
![](./images/1581264921687.png)
```
使用 Iteration 的方法解决，怎么训练 discriminator？
假设有一堆 positive example、negative example
	positive example：人画的
	negative example：random sample 出来的
在每一个 iteration 里面，就是给 positive example 高的分数，给 negative example 低的分数。
当学出来一个 discriminator 以后，可以用 discriminator 做 generation。(discriminator 是可以做 generation 的，只要会解 arg maxD(x)，用这个 discriminator generate 出一些它觉得好的 image，这些 image 用 x~ 表示)
在下一个 iteration 里面，将原来 random 的 image 换成它觉得好的 image。
反复循环下去
```

![](./images/1581265043382.png)
![](./images/1581265299582.png)
```

```

![](./images/1581265444085.png)
```
GAN 可以被视为是 Structured Learning 的一个
```

![](./images/1581265575396.png)
```
1.Generator
Pros:
	很容易做生成
Cons：
	不容易考虑 component 和 component 之间的 correlation，只学了 pixel 和 pixel 之间的相似程度，学不到大局。

2.Discriminator
Pros:
	可以考虑大局
Cons：
	不容易生成，需要解一个 arg max D(x)
```
![](./images/1581265674933.png)
```
generator 取代了 arg max D(x)，generator 就是在学怎么去解 arg max D(x) 这个 problem。
```

>**Benefit of GAN**

![](./images/1581265730788.png)
```
用 generator 来解 arg max D(x)，更加有效，更加一般化。
```
![](./images/1581265860870.png)
![](./images/1581265980723.png)
![](./images/1581266037495.png)

## Conditional Generation of GAN

```
你可以输入比如文字，产生对应那个文字的图片。可以操控输出的结果。
```

### Text-to-Image

![](./images/1581598403431.png)
```
输入文字产生对应的图片，可以当作单纯的 supervised learning problem 来看，需要一大堆的图片，每一张图片都有对应的文字的描述。
输入一段文字，输出一张图片，希望输出的图片和目标越接近越好。
```

>**Conditional GAN**

![](./images/1581598821681.png)
![](./images/1581599289355.png)
```
在原来的 GAN 中，input 一个从 normal distribution sample 出来的 z 到 generator 里，根据这个 z 产生一张 image。
在 Conditional Generation 里，generator 不止需要 input z，同时还有 input Conditional c。
在原来的 discriminator 里，input image x，然后 discriminator 告诉你 x 的 quality 好不好。
希望机器按照我们输入的 condition，产生不同的 image。
在作 Conditional GAN 的时候，不可以只看 generator 的 output，要同时看 generator 的 input 和 output。input condition c 和 object x 到 discriminator，然后产生一个 scalar，这个 scalar 做两件事情
	1、x 是不是真实的
	2、x 和它的 condition c 合起来是不是可以凑成一对

有两种 case 要给低分的
	1、输入一段文字给 generator，generator 产生一张模糊的图给低分
	2、给一张清晰的图，但随便给它加上随机的文字，给低分
```
>**Conditional GAN - Algorithm**

![](./images/1581599585412.png)

>**Conditional GAN - Discriminator**

![](./images/1581600222873.png)
```

```
>**Stack GAN**

![](./images/1581600418777.png)

### Image-to-Image

![](./images/1581600507370.png)
![](./images/1581600602731.png)
![](./images/1581600717687.png)

>**Patch GAN**

![](./images/1581600827276.png)

>**Speech Enhancement**

![](./images/1581600971503.png)
![](./images/1581601028247.png)

>**Video Generation**

![](./images/1581601133403.png)

![](./images/1581601312049.png)
[小精灵](https://github.com/dyelax/Adversarial_Video_Generation)


## Unsupervised Conditional Generation

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

![](./images/1581604947835.png)
```
没有 domain x 和 domain y 的 link，generator 怎么知道产生 domain y 的 image？
需要一个 y domain 的 discriminator，这个 discriminator 看过很多 y domain 的 image，所以给它一张 image，它可以鉴别是 x domain image 还是 y domain image。
generator 要做的事情是骗过 discriminator，那 generator 产生出来的 iamge 就会像是 y domain image。
问题：generator 可以产生一张像梵高的画作，但可以产生和 input 无关的东西。
所以 generator 不只是骗过 discriminator 就好，同时 generator 的 input 和 output 有一定程度的关系。
```

![](./images/1581605050443.png)
```
1、第一个方法就是不管它，直接 train 下去。
如果 generator 比较 shallow，input 和 output 特别像
如果 generator 很深，就可以让 input 和 output 不一样，那就需要做额外的处理，免得让 input 和 output 变得完全不一样。
```
![](./images/1581605127114.png)
```
2、第二个方法就是拿一个 pre-trained 好的 network，比如 VGG，然后把 generator 的 input 和 output 都给 pre-trained 好的 network，output 出 embedding。
在 train 的时候，generator 一方面是想要骗过 discriminator，让 output 的 image 看起来像梵高的画作，同时 generator 还有另一个任务，希望 pre-trained 的 mode 它的 embedding output 不要差太多。
好处：如果 embedding 不会差太多，generator 的 input 和 output 就不会差太多。
```
![](./images/1581605208936.png)
```
3、第三个方法就是 CycleGAN，在 CycleGAN 里面要 train 一个 x domain 到 有domain 的 generator，同时 train 一个 y domain 到 x domain 的 generator。
目的：input 一张风景 image，第一个 generator 将它转成 y domain 的图，第二个 generator 将 y domain 的图还原成原来一摸一样的图。现在除了要让 generator 骗过 discriminator 以外，同时要让 input 和 output 越像越好。
```
![](./images/1581605262777.png)
```
CycleGAN 可以做双向的，再 train 另外一个 GAN，将 y domain 的图转成 x domain 的图，同时需要 discriminator 确保这个 generator 产生的图像是 x domain 的图，接下来再把 x domain 的图转回 y domainn 的图。一样希望 input 和 output 越接近越好。
```

>**Issue of Cycle Consistency**

![](./images/1581606285962.png)
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

![](./images/1581606645616.png)
![](./images/1581606690646.png)


### Projection to Common Space

![](./images/1581606722678.png)

![](./images/1581606940402.png)
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
![](./images/1581606985706.png)
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
![](./images/1581607333663.png)
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

## Theory behind GAN

>**Generation**

![](./images/1581335234255.png)
![](./images/1581335355568.png)
```
假设生成的是 image，每一张 image 都是 high dimensional 中的一个点，产生的 image 会有一个固定的 distribution，在整个 Image Space 里面，在整个 image 构成的高维空间中，只有少部分 sample 出来的 image 看起来像是人脸。
```

>**Maximum Likelihood Extimation**

![](./images/1581336349032.png)
```
在 GAN 之前怎么做 Generation 这件事呢？
现在是不知道 Pdata(x) 的 formulation，自己找一个 distribution PG(x)，是由一组参数 θ 操控，调整 Gussion 的 mean 和 varince 使得得到的 distribution PG(x) 和 真实的 distribution Pdata(x) 越接近越好。
1、sample m 个 data from Pdata
2、对每一个 sample 出来的 x 计算它的 Likelihood，假设给定一组参数 θ，就可以知道 PG(x) 这个 probability distribution 的图形样子。就可以计算从这个 distribution sample 出某一个 xi 的几率。
3、找出一个 θ，使得 PG 和 Pdata 越接近越好，把所有的几率乘起来，就得到了 total likelihood，希望这个 total likelihood 越大越好。

Find θ* maximum the likelihood.
```

>**Maximum Likelihood Extimation = Minimize KL Divergence**

![](./images/1581336876552.png)
```

```

![](./images/1581337772596.png)
```
把从 Gussion Distribution sample 出来的 z 丢到 generator 中，得到另外的 sample，把这些 sample 集合起来，就可以得到另外一个 distribution。
prior distribution: 可以设置成 Normal Distribution、Uniform distribution.
PG(x) 和 Pdata(x) as close as possible
G* = arg min Div(PG, Pdata)：找一个 possible，这个 possible 定义出一个 distribution PG，这个 PG 和 Pdata 的 divergence 越接近接好。
```

>**Discriminator**

![](./images/1581337892962.png)
```
Pdata: 真实数据的 distribution
PG: 
前提是我们不知道 PG 和 Pdata 的 distribution 的样子，但是可以从这两个 distribution 中 sample data 出来，PG 是有 generator 所定义的，是从一个 probability distribution sample 一些 vector，每一个 vector 就会产生一张 image。
```
![](./images/1581339538762.png)
```
透过 discriminator 可以量这个两个 distribution 间的 divergence，根据 Pdata 和 PG 去训练一个 discriminator，给 Pdata 的 score 越大越好，给 PG 的 score 越小越好，训练的结果就会告诉我们 Pdata 和 PG 之间的 divergence 有多大。
Objective function	V(G,D)：和 Generator 和 Discriminator 有关，在 train Discriminator 的时候，会 fix 住 Generator，只调 Discriminator 的参数，maximum 结果，
如果 x 是从 Pdata 中 sample 出来的，希望 logD(x) 越大越好
如果 x 是从 generator 中 sample 出来的，希望 log(1 - D(x)) 越大越好，它的值越小越好。
Discriminator 做的事情和 Binary Classification 几乎一样，train 下去之后，Binary Classification 是在 minimum cross entropy 或者 minimum mean squared error。
当我们解完这个 optimization problem 之后，会得到一个最小的 loss，或者是会得到一个最大的 objective value。
```
![](./images/1581339778294.png)
```
假设 sample 出来的 data 靠的很近，对 Binary classify 来说，很难区别这两个类别的不同，loss 就没办法压低，就没有办法把 objective value 提高。没有办法找到一个 D，使得 V 的值很大。意味着这两个类别的 data 是很接近的，他们的 divergence 是小的。
```

>**证明 Objective Value 和 Divergence 有关系**

![](./images/1581342205355.png)
![](./images/1581342928413.png)
![](./images/1581343107140.png)
![](./images/1581343146075.png)
![](./images/1581343413521.png)
```
找到一个 function D，使得方程结果越大越好，前提 D(x) 可以是任意 function。
求 D*，什么样的 D 可以让微分值为 0，是 critical point。
```
![](./images/1581343792450.png)
![](./images/1581344309183.png)
```
本来是找到一个 G 来 minimum PG 和 Pdata 的 divergence
Div(PG,Pdata)，写出一个 objective function V(D,G)，找到一个 D 来 maximum V(D,G)，就是 PG 和 Pdata 的 divergence。
当 generator 是 G1 的时候，红色点是 discriminator 可以让 V(G,D) 最大。
V(G,D) 高就代表着 PG 和 Pdata 之间的 divergence。
```

>**Algorithm**

![](./images/1581344500679.png)
![](./images/1581344848650.png)
![](./images/1581345171878.png)
![](./images/1581345923556.png)
```
在 train generator 的时候，一次不能 update 太多
在 train discriminator 的时候，理论上应该把它 train 到底，update 多次，必须找到 maximum value，才是量 divergence。
```
![](./images/1581346320581.png)
```
Minimize Cross-entropy 等同于 Maximize objective function
```
![](./images/1581347438447.png)
```
2、从某一个 prior distribution(Gussion Distriution、Normal Distribution) 去 sample 出 m 个 vector，这些 vector 用 z表示。 
3、把 vector z 丢到 Generator 里面，会产生 image。
4、train discriminatior
```
![](./images/1581347705793.png)

![](./images/1581347870785.png)

![](./images/1581348295234.png)

## General Framework of GAN

```
对某种 objective function 就是在量 JS Divergence，能不能量其他的  Divergence，fGAN 告诉我们怎么量其他的 Divergence，用不同的 f-divergence 量 generate example 和 real example 之间的差距。
其实不止用 JS Divergence，任何 f-divergence 都可以放到 GAN 的架构里面去。
```

>**f-divergence**

![](./images/1581403479294.png)
```
p(x): 从 P 的 distribution sample 出来的几率
q(x): 从 Q 的 distribution sample 出来的几率
Df(P||Q) 的是某一种 f-divergence 的条件
	f is convex 凸函数
	f(1) = 0
f 放不同的 function，就是不同的 divergence。

为什么这个方程式可以看作是在衡量 P&Q 的差异呢嗯？
	if p(x) = q(x)，Df(P||Q) = 0，0 是这个方程式可以达到的最小距离，也就是说，P&Q 有些不同，算出来的 f-divergence 就会大于 0。
```
![](./images/1581403571751.png)
```
if f(x) = xlogx，就是 KL-divergence
if f(x) = -logx，就是 Reverse KL-divergence
if f(x) = (x-1)2，就是 Chi Square Divergence
f 不同，就会得到各式各样的 divergence
```

>**Fenchel Conjugate**

![](./images/1581404204090.png)
```
每一个 convex function f 都会有另一个 conjugate function f*
穷举所有的 t，看哪一个 max{xt-f(x)} 最大
假设 f*(t1) 是多少，将 t1 代进去，穷举各种不同的 x 值，看哪一个 x 值可以让 t1 最大。
x1t1 - f(x1)
x2t1 - f(x2)
x3t1 - f(x3)
...
这种方式比较麻烦
```
![](./images/1581404468829.png)
![](./images/1581404650664.png)
![](./images/1581404747706.png)
```
xt-f(x) 是直线，t 是变量，代不同的 x 值进去，就是不同的直线。
把所有 x 值造成的直线画出来，再取 upper bound，所以 f*(t) 是 convex。

f(x) = xlogx，把所有的 upper bound 连起来，f*(t) = exp(t - 1)
```

>**Connection with GAN**

![](./images/1581405111206.png)
```
f(x) 和 f*(t) 互为 conjugate 的。
假设有一个 convex function 叫做 f(x)，就可换成 max{xt - f*(t)}
假设有一个 f-divergence function Df(P||Q)，f(p(x)/q(x)) 是一个 convex function，将 x =  p(x)/q(x) 代入 max{xt - f*(t)}。
learn 一个 Discriminator，就是 input 一个 x，output 出一个 scalar，这个 scalar 就是 t，t 就用 D(x) 代表，代入之后的方程式是 f-divergence 的 lower bound。
```
![](./images/1581405811467.png)
```
V(G,D) 定义不同，就是在量不同的 divergence，
```
![](./images/1581405957379.png)

>**Mode Collapse**

![](./images/1581406262566.png)
```
update 不同的 divergence 的厉害之处
什么是 Mode Collapse？
	real data 的 distribution 是比较大的，
	generated data 的 distribution 是比较小的。
产生出来的 distribution 会越来越小。
```

>**Mode Dropping**

![](./images/1581406479319.png)
```
distribution 有很多的 mode，但 generator 只能产生同一群，没有办法产生两群不同的 data。
```

![](./images/1581406648342.png)

![](./images/1581406726905.png)
```
为了避免 Mode Collapse，可以 train 多个 generator。
```


## Tips for Improving GAN

>**JS divergence is not suitable**

![](./images/1581417630886.png)
```
最原始的 GAN 它量的是 generated data 和 real data 之间的 JS divergence，但是现在用 JS Divergence 来衡量的时候却有一个严重的问题。
问题的根源：generator 产生的 data distribution 和 real 的 data distribution 往往是没有重叠的。
	1、data 本质上的问题：image 是一个 high dimensional space 中的低维 manifold，PG 和 Pdata 的 overlap 几乎是可以忽略的。
	2、sample 出来的少量 data 里面，PG 和 Pdata 是没有重合的。
```
![](./images/1581417933573.png)
```
当 PG 和 Pdata 没有重合的时候，用 JS Divergence 来衡量 PG 和 Pdata 之间的距离，会对 training 造成很大的伤害。
JS Divergence: 如果两个 distribution 没有任何的重合的话，算出来的结果就是 log2，不管这两个 distribution 是否有接近，只要没有重合，没有 overlog，算出来就是 log2。

对它来说 PG0 和 PG1 是一样差的，generator 是不会把 PG0 update PG1 的。
intuition: 只要不重合，loss 就是一样的，Binary Classifier 很容易区别 PG0 & Pdata，PG1 & Pdata，算出来的 loss 是一样的。train 到最后，得到的 loss 或者 objective value 就是 JS divergence。

注：生成器更新的的过程是 minimize 散度，然而所有散度都一样，相当于都是最小值，生成器就不会更新，JS 这种特性使其不能衡量无重合 distribution 的距离，生成器无法正常的优化。
```

>**Least Square GAN(LSGAN)**

![](./images/1581418202293.png)
```
蓝色是 fit data 的 distribution，绿色是 real data 的 distribution，learn 一个 Binary Classifier，Binary Classifier 会给绿蓝色点 0 fen，给绿色点 1 分，Binary Classifier 的 output 是 sigmod function，在接近 1 和 0 的地方特别平，即梯度消失，train 不动。

Least Square GAN(LSGAN) 将 sigmod 换成了 linear，本来是一个 classification problem，现在是一个 regression problem
如果是 positive example 就让它的值越接近 1 越好，如果是 negative example 就让它的值越接近 0 越好。
```
>**Wasserstein GAN(WGAN): Earth Mover's Distance**

![](./images/1581418382540.png)
```
在 Wasserstein GAN 里面用的是 Earth Mover's Distance，或者叫做 Wasserstein Distance 来衡量两个 distribution 的差异。
P 向 Q 走的平均距离就是 Earth Mover's Distance。
```

![](./images/1581418485899.png)
![](./images/1581418540097.png)
```
把 P 的土铲倒 Q 的位置的时候，有很多组不同的铲法，推土机走的平均距离不一样，Wasserstein Distance 是说穷举所有可能铲土的方法，叫做 moving plan，每一个 moving plan，推土机平均的距离算出来，看哪一个距离最小。
```
![](./images/1581418721793.png)
```
把 P 的土挪到 Q 那边，首先定义一个 moving plan，可以化成一个 matrix γ，γ 上的每一个 element 就代表说要从纵坐标的这个位置挪多少土到横坐标这个位置，element 值越亮，就表示挪的土越多。
计算 γ 挪土要走的距离？
每一个纵轴的值叫做 xp，每一个横轴的值叫做 xq，穷举所有的 xp、xq 的组合，再算从 xp 要挪多少土到 xq，再算 xp 和 xq 的距离，summation 所有的。
Earth Mover's Distance 要做的就是穷举所有的 γ，看哪一个 γ 算出来的距离最小，最小距离就是 Wasserstein distance。
```
![](./images/1581418886558.png)
```
怎么用 discriminator 来衡量 Wasserstein Distance，如果 Wasserstein Distance 来衡量两个 distribution 的距离有什么好处？
对 Wasserstein Distance 来说，PG0 和 Pdata 的差异是 d0，PG50 和 Pdata 的差异是 d50，d50 要比 d0 小的，所有 d50 的 distribution 要比 d0 的 distribution 要好的，所以对 generator 来说，它会 update 参数，将 d0 的 distribution 挪到 d50 的 distribution 的位置，直到最后 generator 的output 和 real data 真正的重合。
```
![](./images/1581419061811.png)
```
如果 x 是从 Pdata 中 sample 出来的，让它的 discriminator 的 output 越大越好，如果 x 是从 PG 中 sample 出来的，让它的 discriminator 的 output 越小越好，但这些是不够的，还必须有 constraint，discriminator 必须是 1-Lipschitz function，是很平滑的，很 smooth 的。

如果不考虑 constraint，real data 代到方程式里面越大越好，generated data 代到方程式里面越小越好，如果 generated data 和 real data 没有 overlag，这个 training 永远不会收敛。
所以必须有一个额外的 constraint，是说这个 discriminator 足够 smooth。
```
![](./images/1581419363920.png)
```
Lipschitz Function
当 input 有了 change 的时候，output change 不能太大，把 K = 1 is 1-Lipschitz Function，意味着说 output change 比 input change 小，这个 function 就是一个 smooth function。
discriminator 需要 constraint，原始的做法是 Weight Clipping
用原来的 Gradient Ascent train discriminator，train 完后，如果 weight 大于事先设置好的参数 c 后，就设置 w=c，if w<-c，就设置 w=-c。限制住了 weight 的大小，是比较平滑的，让 discriminator 在 output 的时候没有办法产生非常剧烈的变化。
```

>**Improved WGAN(WGAN-GP)**

![](./images/1581429311381.png)
```

```

![](./images/1581429458934.png)
```
Ppenalty 的样子
从 Pdata、PG sample 出任意一个点出来，把两个点相连起来，在着两个点之间 random sample，sample 出来的 x 就当成从 Ppenalty sample 出来的。
train GAN 的时候，让 generator 顺着 discriminator 给出的 gradient 的方向挪到 Pdata 的位置，generated output distribution 和 real data distribution 中间连线的区域才会影响结果，因为 PG 是看着 gradient 的方向update 参数的。
```

![](./images/1581429683144.png)

>**Spectrum Norm**

![](./images/1581429761069.png)

>**Algorithm of WGAN**

![](./images/1581429846744.png)
![](./images/1581429939167.png)
```
在 WGAN 里面，把 sigmod 换成 linear。
```

>**Energy-base GAN(EBGAN)**

![](./images/1581430097357.png)
```
更改了 network 的架构，将 discriminator 的 Binary Classifier 改成了 Auto-encoder。
```
![](./images/1581430183152.png)
```

```

## Feature Extraction

>**InfoGAN**

![](./images/1581491660255.png)
```
GAN 会 input 一个 random vector，就 output 一个 object，期望 input 的 vector 每一个 dimension 代表了某种 specific characteristics，改了 input 的 dimension，output 就会有一个对应的变化，就可以知道每一个 dimension 做的事情是什么。
假设二维平面代表 generator input 的 random vector space，期望在这个 letter space 上面，不同的 characteristic object 的分布是有某种规律性的，但实际上它的分布是不规则的。
期待是改变了 input vector 的某一个维度，它就会从 绿色->黄色->橙色->蓝色，有一个固定的变化，但实际上不是那样。
```
![](./images/1581492008112.png)
```
InfoGAN 就是来解决这个问题。
在 INfoGAN 里面，会把 input vector 分成两个部分(假设 input vector 是 20 个 dimension，前 10 维叫做 c，后 10 维叫做 z')，在 InfoGAN 里面，会 train 一个 classifier(作用：看 generator 的 output，然后决定根据 generator 的 output 去预测现在 generator input 的 c 是什么)
将 classifier 视为 decoder，将 generator 视为 encoder，generator 和 classifier 合起来就是 "Auto-encoder"，但和传统的 Auto-encoder 做的事情相反
	传统的 Auto-encoder：给一张 image，把它变成一个 code，再把 code 解回原来的图片
	InfoGAN：给一个 code 通过 generator 产生一张 image，classifier 根据 image 决定原来的 code 的样子。
Discriminator 一定要存在，Discriminator 检查这个 image 像不像 real image，如果 generator 为了 classifier 猜出 c 是什么，而刻意的把 c 原封不动的贴在 image 上，Discriminator 就会发现不对，所以 generator 并不能把 c 放在 image 里面透露给 classifier 知道。
InfoGAN 在实作上，Discriminator 和 Classifier 会 share 参数，因为 input 是同一个 image，只是 output 不一样，一个是 scalar，一个是 code vector。
```
![](./images/1581492142382.png)
```
加上 classifier 的好处
需要解决的问题是 input 的 feature 对 output 的影响不明确。
为了使 classifier 可以成功的从 image x 里面知道原来 input c 是什么，generator 要做的事情是，必须让 c 的每一个 dimension 对 output x 都有一个明确的影响，如果 generator 可以学到 c 的每一个 dimension 对 output x 都有一个明确的影响，那 classifier 就可以轻易的根据 output image 反推出原来的 c 是什么。
input 中的 c 代表了 image 的某些特征，对 image 有明确的影响
input 中的 z' 纯粹代表了随机的东西
input 中定义的 c 不是代表了image 的某些特征而被归类为 c，而是它被归类为 c，所以代表了 image 的某些特征。
```
![](./images/1581492198924.png)

>**VAE-GAN**

![](./images/1581492390150.png)
```
可以看作是 VAE 强化 GAN，也可以看着是 GAN 强化 VAE
在 train VAE-GAN 的时候，一方面 encoder、decoder 让 reconstruction  error 越小越好。Decoder 希望它的 output image 越 realistic 越好。
	从 VAE 的角度来看，原来在 train VAE 的时候，希望 input 和 output 越接近越好，但是对 image 来说，单纯只让 input 和 output 越接近越好，VAE 的 output 不见得会变得 realistic，通常产生的 image 是很模糊的，因为不知道怎么算 input 和 output x 的 loss，那就加一个 discriminator，迫使 Auto-encoder 在生成 image 的时候不是 minimize reconstruction error，同时还要产生比较 realistic image 让 discriminator 觉得是 realistic。
	从 GAN 的角度来看...

Encoder：Minimize reconstruction error，同时希望 z 分布接近 normal distribution
Generator：Minimize reconstruction error，同时 Cheat discriminator
Discriminiator：将分辨 image 是 real image 还是 generated image。
```
>**VAE-GAN - Algorithm**

![](./images/1581492839189.png)
```
初始化 Encoder，Decoder，Discriminator
在一个 iteration 中：
	sample M 个 real image
	再产生 M 个 image codes，把 code 写作 z~
	把 image codes 丢到 Decoder 里面，再产生 reconstructed image，写作 x~
	从 normal distribution sample 出 M 个 image codes z
	把 image code z 丢到  Decoder 里面产生 image x^
	Encoder 的目的是希望原来的 image 和 reconstructed image，||x~ - x|| 越接近越好，希望 x 产生出来的 z~ 和 normal distribution 越接近越好
	Decoder 的目的是 Minimize reconstruction error，希望它产生出来的东西可以骗过 discriminator，希望 discriminator 给它高的分数，Decoder 会产生两种东西，1、reconstructed image x~，2、mechine 自己 generated image
	Discriminator 的目的是如果是一个 real image 就给高分，如果是 fit image 就给低分，fit image 分为两种：reconstructed image、generated image
	
注：Discriminator 其实也可以是 3 个 class 的 classifier，鉴别一张 image 是 real、generated、reconstructed 的，generated、reconstructed image 还是挺不像的。
```

>**BiGAN**

![](./images/1581492962532.png)
```
VAE-GAN 是去修改了Auto-encoder
BiGAN 也是。
BiGAN 并不会把 Encoder 的 output 丢给 Decoder
加上一个 Discriminator，把 Encoder 和 Decoder 的 input image x 和 output code z 当成 Discriminator 的 image x 和 code z，鉴别 x 和 z 的 prior 是从 Encoder or Decoder。
```

>**BiGAN - Algorithm**

![](./images/1581493125145.png)
```
Initialize encoder En, decoder De, discriminator Dis
In each iteration:
	sample M 个 real image
	把 image 丢到 Encoder 里面，Encoder 会 output code，得到 M z~
	从 normal distribution 中 sample 出 M 个 code
	把 code 丢到 Decoder 里面，得到 x~
	learn 一个 Discriminator，给 Encoder 的 input 和 output 高分，给 Decoder 的 input 和 output 低分。
	Encoder 和 Decoder 联手起来，让 Discriminator 给 Encoder 的 input 和 output 低分，给 Decoder 的 input 和 output 高分。
```
![](./images/1581493270704.png)
```
Discriminator 做的事情是 evaluate 两组 sample 出来的 data 到底接不接近
把 Encoder 的 input 和 output 合起来当成 join distribution，P(x,z)
把 Decoder 的 input 和 output 合起来当成 join distribution，Q(x,z)
Discriminator 来衡量这两个 distribution 的差异，P 和 Q 越接近越好。
如果 P 和 Q 的 distribution 一摸一样
就会 En(x') = z' -> De(z') = x'...
```
![](./images/1581493426857.png)
```
让 Encoder 和 Decoder 的 input 和 output 越像越好
BiGAN learn 的 optimum 结果和同时 learn 一个 Encoder/Decoder optimum 结果是一样的，但是它们的 error surface 是不一样的。
BiGAN 如果跟 Auto-encoder 比起来，它们 optimum solution 是一样的，但特性是不一样的，BiGAN 的 Auto-encoder 比较能够抓到语义上的资讯。
```
>**Triple GAN**

![](./images/1581493587401.png)
```
Triple GAN 就是一个 Conditional GAN，是一个 supervised learning 做法，再 Triple GAN 里面，假设有少量的 label data，有大量的 unlabel data。
```

>**Domain-adversarial training**

![](./images/1581493685101.png)
```
Training data 和 Testing data 不 match，通过 generator 抽出 feature，在 Training data 和 Testing data 算它们的 domain，透过 domain 抽出的 feature 有同样的 distribution，是 match 的。
```
![](./images/1581493806194.png)
```
learn 一个 generator，就是 feature extractor，input a image 就 output a feature，Domain classifier 就是 discriminator，discriminator 判断这个 feature 来自哪个 domain(假设有两个 domian x、y)，同时也要有另外一个 classifier，这个 classifier 的工作是根据这个 feature 判断它属于哪一个 class。
```

>**Feature Disentangle**

![](./images/1581493988392.png)
```
learn 一个 encoder，把发音有关的部分丢到语音辨识系统进行辨识，把语者资讯丢到声纹比对去。
learn 两个 encoder
	一个 encoder 的 output 是 发音的资讯
	一个 encoder 的 output 是 语者的资讯
```
![](./images/1581494053340.png)
![](./images/1581494093461.png)
```
加一些额外的 constraint，对 Speaker Encoder 来说，给同一个人的声音讯号，output 的 vector 越接近越好，
```
![](./images/1581494155732.png)
```
再 train 另外一个 classifier
作用：对两个 vector，判断这两个 vector 是同一个人说的，还是不同人说的。
```
![](./images/1581494269357.png)

>**GAN + Autoencoder**

![](./images/1581515228316.png)
```
怎么反推出现在 input vector 每一个 dimension 对应的特征是什么？
先 train 好一个 generator，这个 generator 可以根据一个 vector z，会产生一个 image x，做一个逆向的工程，去反推如果给了一张现成的 image，什么样的 z 可以生成这现成的 image。
再 learn 另外一个 Encoder，这个 Encoder 和 Generator 合起来就是 Auto-encoder，在 train Auto-encoder 的时候，input 一张 image x，把 x 压成 vector z，把 z 丢到 Generator 以后，它 output 是原来那张 image，在 train 的过程中，Generator 的参数是固定不动的，在实作中，Encoder 和 Discriminator 很像，可以用 Discriminator 的参数初始化 Encoder 的参数。
```

>**Attribute Representation**

![](./images/1581515344478.png)
```
将短发的人脸的 code 反推出来，平均就会得到短发人脸的代表
将长发的人脸的 code 反推出来，平均就会得到长发人脸的代表
再相减得 zlong
Short Hair x -> En(x) + zlong = z' -> Gen(z') Long Hair
```

>**Basic Idea**

![](./images/1581516884873.png)
```
首先 train 一个 GAN，train 一个 Generator，，在 space 上随便 sample 一个点(vector)，丢到 generator 中，就会产生一个 image。
将 短袖图片 反推出它在 code space 的位置，然后在 code space 做小小的移动，就会产生一张新的图。
```
>**Back to z**

![](./images/1581516939218.png)
```
怎么做从一张 image 反推出原来的 code 样子？
1、
```
![](./images/1581516981830.png)
```
找一个 z，一方面把 z 丢到 generator 里产生一张 image，要符合 constraint，希望新的 z 和旧的 z0 越接近越好。
```

>**Image Completion**

![](./images/1581517019527.png)
```
修复
```





## Evaluation

```
有一些客观的方法来衡量说产生出来的 object 是好或者不好。
```
>**Likelihood**

![](./images/1581519392293.png)
```
传统上衡量 generator，计算 generator 产生 data 的 likelihood，计算 PG Generator 产生 xi 这张 image 的几率，计算所有的再平均，就是 Likelihood，Likelihood 就代表了 generator 的好坏。
假设 Generator 是一个 network 用 GAN train 出来的话，无法计算 PG(xi)，这个 network 可以通过一些 vector 产生一些 data，但无法算出产生某一笔 data 的几率。
```

>**Likelihood - Kernel Density Estimation**

![](./images/1581519535202.png)
```
让 Generator 产生很多的 data，接下来再用一个 Gaussian distribution 逼近产生的 data。
假设有一个 Generator，让他产生一大堆的 vector 出来，如果是做 image generation 的话，产生的 image 就是 high dimension vector，把这些 vector 当作 Gaussian Model 的 mean，每一个 mean 有一个固定的 varince，然后再把这个 Gaussian 叠在一起，就得到了一个 Gaussian Model，
Gaussian Model 计算产生 real data 的几率，就可以估测出这个 generator 产生出 real data 的 likelihood。
```
![](./images/1581519683978.png)

>**Objective Evaluation**

![](./images/1581519834735.png)
```
常常看到的一种 Evaluation function，是拿一个已经 train 好的 classifier 来评价产生的 object。可以是 VGG、Inception net。
1、input 一张 image 给 image classifier，它会产生一个 class distribution，给每一个 class 一个几率。如果产生出来的几率越集中，表示产生出来的图片的品质越高。它可以轻易判断出 image 的内容。
2、需要考虑 Mode Collapse 的问题，还需要从 diverse 的方向衡量，让机器产生一堆图（举例为 3 张 image），让这 3 张图丢到 CNN 里面，然后产生 3 个 distribution，把这 3 个 distribution 平均起来。如果平均后的 distribution 很 uniform，那就意味着说每一种不同的 class 都有被产生到，代表说产生出来的 output 是 diverse。如果平均后某一个 class score 特别高，mode 倾向于产生 x 的东西，就代表它的 output 不够 diverse。
```

![](./images/1581596514236.png)
```
定义出来一个 score，常用的 score 是 Inception Score，因为使用 Inception net evaluate 的。
怎样的 generator 叫做好？
好的 generator 产生的单一的图片丢到 Incepting net 里面，某一个 class 的 score 越大越好。把所有的 output 丢到 classifier 里面，产生一堆 distribution，把所有 distribution 作平均，越平滑越好，即越平均越好。
```
![](./images/1581596724614.png)
```

```

>**Mode Dropping**

![](./images/1581596965219.png)

>**Minni-batch Discrimination**

![](./images/1581597045894.png)
```

```

>**Optimal Transport GAN（OTGAN）**

![](./images/1581597100232.png)
```
pass
```


### Concluding Remarks

![](./images/1581597359944.png)
![](./images/1581597495060.png)
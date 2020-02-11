---
title: Generative Adversarial Network 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

### Introduction

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


### Theory behind GAN

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

### General Framework of GAN

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


### Tips for Improving GAN

![](./images/1581417630886.png)
```
image 是一个 high dimensional space 中的低维 manifold，

```

![](./images/1581417933573.png)
```

```

![](./images/1581418202293.png)
```

```

![](./images/1581418382540.png)
```

```

![](./images/1581418485899.png)
![](./images/1581418540097.png)
![](./images/1581418721793.png)
```

```
![](./images/1581418886558.png)
```

```

![](./images/1581419061811.png)
![](./images/1581419363920.png)
```

```
![](./images/1581429311381.png
```

```

![](./images/1581429458934.png)

![](./images/1581429683144.png)

![](./images/1581429761069.png)

![](./images/1581429846744.png)
![](./images/1581429939167.png)

![](./images/1581430097357.png)

![](./images/1581430183152.png)


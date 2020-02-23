---
title: Theory behind GAN
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


>**Generation**

![](./images/1582463078785.png)
![](./images/1581335355568.png)
```
假设生成的是 image，每一张 image 都是 high dimensional 中的一个点，产生的 image 会有一个固定的 distribution，在整个 Image Space 里面，在整个 image 构成的高维空间中，只有少部分 sample 出来的 image 看起来像是人脸。
```

>**Maximum Likelihood Extimation**

![](./images/1581336349032.png)
```
在 GAN 之前怎么做 Generation 这件事呢？
现在是不知道 Pdata(x) 的 formulation，自己找一个 distribution PG(x)，是由一组参数 θ 操控，调整 Gaussian 的 mean 和 varince 使得得到的 distribution PG(x) 和 真实的 distribution Pdata(x) 越接近越好。
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
前提是我们不知道 PG 和 Pdata 的 distribution 的样子，但是可以从这两个 distribution 中 sample data 出来，PG 是由 generator 所定义的，是从一个 probability distribution sample 一些 vector，每一个 vector 就会产生一张 image。
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
![](./images/1582463188503.png)
```
2、从某一个 prior distribution(Gussion Distriution、Normal Distribution) 去 sample 出 m 个 vector，这些 vector 用 z表示。 
3、把 vector z 丢到 Generator 里面，会产生 image。
4、train discriminatior
```
![](./images/1581347705793.png)

![](./images/1581347870785.png)

![](./images/1582463337476.png)
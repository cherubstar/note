---
title: Tips for Improving GAN
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


>**JS divergence is not suitable**

![](./images/1581417630886.png)
```
最原始的 GAN 它量的是 generated data 和 real data 之间的 JS divergence，但是现在用 JS Divergence 来衡量的时候却有一个严重的问题。
问题的根源：generator 产生的 data distribution 和 real data distribution 往往是没有重叠的。
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
蓝色是 fit data 的 distribution，绿色是 real data 的 distribution，learn 一个 Binary Classifier，Binary Classifier 会给绿蓝色点 0 分，给绿色点 1 分，Binary Classifier 的 output 是 sigmod function，在接近 1 和 0 的地方特别平，即梯度消失，train 不动。

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
把 P 的土铲倒 Q 的位置的时候，有很多组不同的铲法，推土机走的平均距离不一样，Wasserstein Distance 是说穷举所有可能铲土的方法，叫做 moving plans，每一个 moving plan，推土机平均的距离算出来，看哪一个距离最小。
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
discriminator 需要 constraint
原始的做法是 Weight Clipping，用原来的 Gradient Ascent train discriminator，train 完后，如果 weight 大于事先设置好的参数 c 后，就设置 w=c，if w<-c，就设置 w=-c。限制住了 weight 的大小，是比较平滑的，让 discriminator 在 output 的时候没有办法产生非常剧烈的变化。
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

![](./images/1582464062204.png)

>**Algorithm of WGAN**

![](./images/1581429846744.png)
![](./images/1582464001290.png)
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
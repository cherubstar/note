---
title: Ensemble
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1580890238841.png)
```
Ensemble：聚合
```

### Bagging
```
Bagging 用在很强的 model
```

![](./images/1580890720612.png)
```
简单的 model: Large Bias & Small Variance
复杂的 model: Small Bias & Large Variance
```
![](./images/1580890855240.png)
```
比如一个复杂的 model，取多个 model 的平均值，也许这个 model 就会很接近正确的答案。
```

![](./images/1580891623665.png)
![](./images/1580892069269.png)
```
假如有 N 个 training examples，随机 Sampling N'(usually N=N') 组成 4 个 dataset，再通过 1 个复杂的 model 做 leaning 找出 4 function。
再将 Testing data 放入这 4 个 function 中，把得出来的结果做 Average 或者是 voting。通常比只有 1 个 function 时的 performance 要好（variance 会比较小）。比较不容易 overfiting
如果做的是 regression 的问题时，应该会使用 Average 将 4 个不同的 function 的结果组合起来。如果时 classifier 问题，应该会用 voting 将 4 个不同的 function 的结果组合起来。
注意：当 model 很复杂的时候，担心 model 很容易会 overfiting 的时候，才会做 Bagging，做 Bagging 的目的是减小 variance。
```
>**Decision Tree**

![](./images/1580893027098.png)
```
pass
下面图片上所示是 Decision Tree 分类。
```
![](./images/1580893573862.png)
```
Single Decision Tree 对'初音'的结果
```
![](./images/1580893856486.png)
```
树够深，decision tree 可以做出任何 function。
```
>**Random Forest**

![](./images/1580894263953.png)
```
因为 Decision Tree 太容易 Overfiting，所以单用 Decision Tree 往往不见得达到好的结果。
所以在 Decision Tree 做 Bagging 就是 Random Forest。
	- 使用传统的方法做 random forest，但得到的 tree 每一个棵没有差太多。
	- 在每一次要产生 decision tree foreigh 的时候，都 random 的决定哪一些 features、questions 是不能用的。
Out-of-bag: 可以不用将 label data 切成 training set 和 validation set，一样有 validation 的效果。每一个 model train 的时候只用了部分的 data。
```

![](./images/1580905783524.png)
```
Random Forest 对'初音'的结果
做 Bagging 并不会使 model 更 fit 这个 data。结果是平均之后的。
```

### Boosting

![](./images/1580906448841.png)
```
Boosting 用在很弱的 model
Bagging 可以在 Random Forest 100 个 Decision Tree 的时候是可以平行做。
把 Decision Tree 使用 Bossting 变强的时候，必须是按顺序的。
```

>**How to abtian different classifiers?**

![](./images/1580907471774.png)
```
Re-sampling your training data to form a new set
Re-weighting your training data to form a new set
改变不同的 weight。
```
>**Idea of Adaboost**

![](./images/1580908483585.png)
![](./images/1580909203034.png)
![](./images/1580910461051.png)
```
假如有 4 笔 training data，假设 u1,u2,u3,u4 的 weight 都是 1，用这 4 笔 training data 去训练一个 model，去训练一个 classifier f1(x)，改变 data 的 weight，把 u 的值变一下，让 f1(x) 在这新的 training dataset 的 weight 上的 error rate 变成 0.5。
如何让 error 变大？
让打勾的 weight 变小，打叉的 weight 变大，这样可以让 f1(x) 的 error rate 变大。
使用修改后的 u1,u2,u3,u4 在新的 training data 上训练，f2(x) 在新的 weight 上训练的，所以 error rate < 0.5。和 f1(x) 互补。
```

![](./images/1580910765490.png)
![](./images/1580911154526.png)
```
d1 = (1 - ε1)/ε1 的开方，因为 ε1 < 0.5，所以 d1 > 1。
```
>**Algorithm for AdaBoost**

![](./images/1580912580025.png)
![](./images/1580912859582.png)
```
错误率比较低的 εt，得到了 αt 会比较大
错误率比较高的 εt，得到了 αt 会比较小
```
>**Toy Example**

![](./images/1580913868395.png)
![](./images/1580913946593.png)
![](./images/1580914014148.png)
![](./images/1580914127454.png)

>**Warning of Math**

![](./images/1580914487803.png)
![](./images/1580915190131.png)
![](./images/1580915764147.png)
![](./images/1580916041790.png)
![](./images/1580916493128.png)
![](./images/1580916665428.png)

```
Adaboost + Decision Tree
```
![](./images/1580916802855.png)

### Gradient Boosting

![](./images/1580917088905.png)
![](./images/1580917478487.png)
![](./images/1580917608348.png)
![](./images/1580917800304.png)

### Stacking

![](./images/1580917921505.png)
![](./images/1580918093514.png)

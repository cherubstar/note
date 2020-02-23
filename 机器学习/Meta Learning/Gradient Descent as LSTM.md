---
title: Gradient Descent as LSTM
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581172585022.png)
```
把 Learning Algorithm 当成 RNN。
```
>**Review - RNN**

![](./images/1581172762774.png)
```
RNN 擅长处理 input 是一个 Sequence 的状态。
```
>**Review - LSTM**

![](./images/1581172949758.png)
![](./images/1581173061530.png)
![](./images/1581173174830.png)
![](./images/1581173223742.png)

>**Similar to gradient descent based algorithm**

![](./images/1581173358582.png)
![](./images/1581173491379.png)
![](./images/1581173564315.png)
![](./images/1581173675282.png)
![](./images/1581173805553.png)
```
将 ct-1 当成 θt-1 来看，将 ht-1，xt 换成 -△θl
zf 这个 vector 中的每一个 dimension 都是 1,
zi 这个 vector 中的每一个 dimension 都是 η,
可以说 Gradient Descent 就是 LSTM 的简化版。
```
![](./images/1581174079726.png)
![](./images/1581174151483.png)
```
为了减少更改 code，假设 θ 和 input 的关系不存在。当成一般的 LSTM 硬 train 下去。
```

![](./images/1581174433079.png)
```
所有的参数共用同一个 LSTM，LSTM 只能出一个参数，同样的 LSTM 被用在所有的参数书上。
θ 的处理方式是一样的。不用担心会算出同样的值。
初始从参数 θ 不一样，gradient 算出的结果也不一样。
```
![](./images/1581174551425.png)
![](./images/1581174638582.png)
![](./images/1581174736702.png)
![](./images/1581174903136.png)
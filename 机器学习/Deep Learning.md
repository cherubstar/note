---
title: Deep Learning
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

>**Attracts lots of attention**

![](./images/1577502850135.png)
```
谷歌内部应用方面很广
去搜索一下
```
> **Ups and downs of Deep Learning**

![](./images/1577512120316.png)

>**Three Steps for Deep Learning**

### 1、Define a set of function

![](./images/1577512854901.png)
![](./images/1577512898396.png)
```
每个 Logistic Regression 都有不同的 weights 和 bias。
```
> **Fully Connect Feedforward Netword**

![](./images/1577513359107.png)
![](./images/1577513393302.png)

```

```
![](./images/1577515524371.png)
```
一个 network 如果还没使用参数使它架起来，其实它就是一个 function set。
```
![](./images/1577516389798.png)
```
整个 network 需要一组 input,对每一个 Layer1 的 neuron 来说，每个 neuron 就是 input layer 的每一个 dimension。
```

>**Deep 的定义**

![](./images/1577516846920.png)
![](./images/1577516978821.png)
```
Residual Network 不是一般的 Fully Connect Feedforward Netword。
```
![](./images/1577517492613.png)
![](./images/1577517393038.png)
```
使用 matrix（矩阵） 计算
```

![](./images/1577519008205.png)
```
将第一层的 weights 集结起来，当成一个 matrix w1，把全部的 bias 集结起来当成 matrix b1，后面依次如何
把 input layer 的 x 集结起来当成 matrix x.
计算结果
```
![](./images/1577519319931.png)
```
matrix operation。
```
> **Output Layer**

![](./images/1577519800911.png)
```
Feature extractor 代替 以前的
```

> **Example Application**
 
![](./images/1577520733587.png)
```
手写数字识别，需要一个 function
```
![](./images/1577520808473.png)
```
需要一个 Neural network
```
![](./images/1577520560010.png)
```
intput: 256维
output: 10维
制作一个 function 需要我们自己 design:
自定义 input layer 的维度
自定义 output layer 的维度
自定义 hidden layer 的层数
```
![](./images/1577521519477.png)

### 2、Goodness of function

![](./images/1577521731045.png)
```
input 这个 image，通过 Neural network，得到一个 output，记为 y
然后计算 y 和 y^ 的 cross entropy，调整 Neural network 参数，使得 cross entropy 越小越好。
```
![](./images/1577521980952.png)
![](./images/1577522079282.png)
![](./images/1577522114471.png)

> **计算微分的框架**

![](./images/1577522256680.png)

> **视频**

![](./images/1577522407242.png)
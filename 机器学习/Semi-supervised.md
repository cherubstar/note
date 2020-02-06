---
title: Semi-supervised 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

![](./images/1580979715290.png)
```
Semi-supervised：半监督学习
在 label data 中有一批 unlabel data，usually U >> R
Why semi-supervised learning?
因为收集数据容易，但收集 "labelled" 的数据难。
```
>**Why semi-supervised lerning helps?**

![](./images/1580980247496.png)
![](./images/1580980410575.png)

###  outline

![](./images/1580981390993.png)

#### Semi-supervised Learningg for Generative Model

>**Supervised Generativee Model**

![](./images/1580981845416.png)

>**Semi-supervised Generativee Model**

![](./images/1580982107065.png)

```
两个 label data 是一样多的，但 class2 的 data 是比较多的，所以 class2 的 paramerter proobability 是比较大的。。
```
![](./images/1580983020618.png)
![](./images/1580983537664.png)
```

```



#### Semi-supervised Learning Low-density Separation

![](./images/1580983696561.png)

>**Self-Training**

![](./images/1580984431173.png)
![](./images/1580985232637.png)
```

```
>**Entropy-based Regularization**

![](./images/1580985822835.png)
```

```
>**Outlook: Semi-supervised SVM**

![](./images/1580986104863.png)


#### Semi-supervised Learning Smoothness  Assumption

![](./images/1580986187551.png)
![](./images/1580986495223.png)
```
1、假设 "similar" 的 x，就会出现有 "same" 的 y。
2、如果 x 分布不平均，如果 x1 和 x2 是由 high density region 连接，那么 y^1 和 y^2 是一样的。。
```
>**Example**

![](./images/1580987115606.png)
![](./images/1580987601460.png)
![](./images/1580987704641.png)
```
Classify astronomy vs. travel articles
```
![](./images/1580988031878.png)
```
橙色：Class 1
绿色: Class 2
蓝色：unlabelled data
```
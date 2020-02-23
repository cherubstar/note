---
title: Metric-based Approach
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581239332963.png)
![](./images/1581239489080.png)
```
既做了 learning，又做了 testing。
Face Verification：Training & Testing
```
![](./images/1582455435645.png)

>**Siamese Network**

![](./images/1582455500286.png)
![](./images/1582455547042.png)
```
Train 的图和 Test 的图都通过 CNN，得到两个 embedding，CNN 的参数通常是一样的，计算两个 embedding vector 的 similarity。
Train 资料和 Test 资料是同一个人，output 出的 score 越大越好。
Train 资料和 Test 资料不是同一个人，output 出的 score 越小越好。
```

>**Siamese Network Intuitive Explanation**

![](./images/1581175863241.png)
![](./images/1581176078082.png)
```
Siamese Network 就是一个单纯的 Binary classification problem，每一个 train tas 就是 training 的一笔资料，每一笔资料都有两张 images。
用 CNN 将所有的人脸投影到一个空间上，同一个人的脸就比较接近，不同人的脸就比较远。
```
![](./images/1581176158425.png)

>**N-way Few / One-shot Learning**

![](./images/1581176596400.png)
```
5-ways 1-shot: 有 5 个 classes，每一个 class 都只有一个 example，
```

>**Prototypical  Network**

![](./images/1582455718768.png)

>**Matching  Network**

![](./images/1582455795909.png)

>**Relation  Network**

![](./images/1581177098717.png)


>**Generate Data**

![](./images/1582455920782.png)
![](./images/1582455857414.png)
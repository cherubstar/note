---
title: Meta Learning
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581089585071.png)
```
让机器学习如何做学习这件事。
机器学过一大堆任务以后，根据过去任务所汲取的经验，变成了一个 learner，往后有一个新的任务，它可以学习的更快。
例如：
在过去学习了 speech recognition，image recognition，那在做 text classification 会做的更好。
Life-Long: 不断的用同一个 model 来做学习，同一个 model 学习很多技能。
Meta: 不同的任务有自己的 model，机器可以从过去的学习经验里面学到一些东西使得未来要去 train 一个新的 model 的时候，可以训练的更快更好。
```
![](./images/1581090304896.png)
```
让机器自动的找出 learning algorithm，找出生成 function 的 function。
```
![](./images/1581090432906.png)
```
Machine Learning: 找出一个函数 f 的能力
Meta Learning: 找出一个可以生成 function 的函数 F 的能力。
```
![](./images/1581090570824.png)
```
1、define a set of Learning algorithm
2、goodness of Learning algorithm
3、pick the best Learning algorithm
```

### Define a set of Learning algorithm

![](./images/1581154410429.png)
```

```

### Defining the goodness of Learning algorithm

![](./images/1581154676461.png)
```

```
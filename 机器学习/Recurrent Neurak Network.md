---
title: Recurrent Neurak Network
renderNumberedHeading: true
grammar_cjkRuby: true
---


>**Example**

![](./images/1579072975450.png)
```
系统能自动知道每一个词汇属于哪一个 Slot。
把一个词汇放到 Network 中去，需要把词汇变成一个向量。
```
![](./images/1579079024220.png)
![](./images/1579079169173.png)
```
有可能有一些词汇没有见过，需要在 1-of-N encoding 中多加一个 dimension。
这个 dimension 代表 other。
```

>**Probelm**

![](./images/1579079833742.png)
```
input 同样的东西，output 的也是同样的。
比如 input Taipei，output Taipei。Taipei 可以是出发地，也可以是目的地。
这时候需要 Network 是有记忆的。
```

![](./images/1579080210867.png)
```
每一次 hidden layers 中的 neuron 产生 output 的时候，这个 output 都会 存到 memory 中去。
对 neuron 而言，除了 x1，x2 以外，存在 memory 中的值 a1，a2 也会影响 neuron 的 output。
```
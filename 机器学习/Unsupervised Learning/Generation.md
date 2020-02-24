---
title: Generation
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

### Creation

![](./images/1582471655113.png)
```
pass
```

### Creation - Image Processing

![](./images/1582471677075.png)
```
对 machine 来说，machine 在影像处理上可以做到 classify，可以鉴别 猫和狗的不同，但或许 machine 并不真的了解猫和狗。
在未来 machine 可以画出一个猫之后，就会有不同的概念了。
```

## Generative Models

![](./images/1582471705117.png)
```
可以分为三个方法。
```

### PixelRNN

![](./images/1582471738677.png)
```
让 machine 自己画一张 3x3=9 个 pixel 的 image，怎么画呢？
假设先随机 random 给 image 涂一个红色 pixel，接下里 learn 一个 NN，input 是已经涂上红色的 pixel，它的 output 就是接下来要吐出什么颜色的 pixel。
怎么描述一个 pixel？
一个 pixel 就是 rgb 三种颜色构成的，一个 pixel 就是一个三维的 vector。
三维的 vector(红色) -> NN -> 另外的三维的 vector(蓝色)
三维的 vector(红色，蓝色) -> NN -> 另外的三维的 vector(浅蓝色)

这个一个 unsupervised 的，不需要任何的 label。
```
![](./images/1582471765454.png)
```
遮住班长图片，让 machine 画出。
```
![](./images/1582510892308.png)
```
给定前面的声音讯号，predict 下一个 sample 的结果。
硬 train 下去，就可以合出一段声音。
也可以在影像的应用，比如 input 一个 video，predict 接下来的剧情。
```

>**Example - Pokemon Creation**

![](./images/1582471808422.png)
```
要让 machine 看过这些小图之后，自己产生新的宝可梦出来。
```
![](./images/1582471832821.png)
```
一个 pixel 应该用三维的 vector，三个数字来描述它们。
```
![](./images/1582471856190.png)
```
每一个 row 就代表了一张 image，每一个数字就代表了一个颜色。
```
![](./images/1582471897276.png)
```
覆盖的面积不同，产生的结果也不同。
很难被 evaluate。
这是给了开头，让 machine 画下去。
```

![](./images/1582471924440.png)
```
这是什么都不给，让 machine 从头开始画下去。
每次的结果都不一样。
```

### Variational Autoencoder(VAE)

![](./images/1582471946800.png)
![](./images/1582471968291.png)
```
原来 image -> encoder -> code -> decoder -> 新的 image
希望新的 image 和 原来 image 越接近越好。
把 decoder 拿出来，给 decoder 一个 random 的 vector as code。希望 output 就是一张 image。
```
![](./images/1582472002394.png)
```
Auto-encoder: 这么做的话，得到的 performance 不一定很好。
VAE: 得到的结果比较好。
原来 image -> encoder -> output 两个 vector(如果 code 是三维的，output 的 vector 也是三维的 [(m1,m2,m3),(σ1,σ2,σ3)])，用 normal distribution 去 generate 一个三维的 vector(e1,e2,e3)，
接下来 ci = exp(σi) * ei + mi。
(c1,c2,c3) 才是 code。
丢到 decoder 里面，希望 decoder minimize reconstruction error。
同时 minimize ∑(1+σi - (mi)2 - exp(σi))。
```
![](./images/1582472025502.png)
```
VAE 的结果。
```
![](./images/1582472047764.png)
```

```
![](./images/1582513509965.png)
```
结果展示
```

>**Writing Poetry**

![](./images/1582472086223.png)
```
用 VAE 写诗。
sentence -> ecoder -> code -> decoder -> sentence

先 random 学两个 sentence，通过 encoder 把这两个句子在 code sapce 上面找出对应的 code。把这两个点相连，在中间等间隔的取一些点，把等间隔的取一些点丢到 decoder 里面，再还原出句子。
```
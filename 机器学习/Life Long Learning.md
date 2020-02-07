---
title: Life Long Learning 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581066789256.png)

### Knowledge Retention

![](./images/1581066954727.png)
```
知识保留，但不是不妥协
```

>**Example - Image**

![](./images/1581067431814.png)
```
3 layers，50 neorons，
先 train task1，再 train task2，将训练完 task1 的 model 作为 task2 的 initial paramter。结果是对 task1 的辨识率降低。
```
![](./images/1581067648699.png)
```
同时 train task1 和 task2，得到的结果还是可以的。
```
>**Example - Question Answering**

![](./images/1581068099826.png)
```
1、训练一个 model 去解 20 种不同的题型
2、20 种题型分别训练 20 个 model 去解 20 种不同的题型
```
![](./images/1581068456615.png)
![](./images/1581068578589.png)
```
分别学习，train 出来的 model 会把之前学的遗忘掉
同时学习，虽然达不到 100%，但是遗忘的较少。
```
>**Wait a minute ......**

![](./images/1581068955538.png)

>**Elastic Weight Consolidation**

![](./images/1581069360772.png)
![](./images/1581069625454.png)
```
当机器学完过去的以后，有一些 weight 是重要的，有一些 weight 是不重要的，重要的 weight 巩固起来，不做太大的更动，只调那些不重要的 weight。
假设过去的任务学完以后，得到的 model 参数是 θb (θb 是一个 network，里面包含一大堆的 weight & bias)，θb 中的每一个参数都会有一个 guard，用 bi 表示。θbi 表示 θb 中第 i  参数。第 i 个参数会有一个 guard，告诉我们这个参数有多重要，有多不应该更改这个参数。

L'(θ) = L(θ) + λ∑ bi(θi - θbi)2
θi: 每一个参数
θbi: 之前任务所学到的参数
θi-θbi: 每一个参数和之前任务所学到的参数之间的距离。
```
>**Example - Common**

![](./images/1581071895494.png)
```
首先初始化一个参数，random initial θ0，使用 Gradient Descent 调整 θ0，
当机器学完以后，就得到 θb。
让机器继续学习 task2，将 θb 作为 task2 的 initial parameter，因为 task1 和 task2 是不一样的，所以 error surface 也是不一样的，同样的参数在 task1 和 task2 上得到的 loss 值是不一样的，使用 Gradient Descent 调整 θb 在 task2 上的 loss 变小，新的参数叫做 θ*。
回头看 task1 的 loss，loss 变大，换句话说，机器忘记了在 task1 的所学。
```

>**Example - EWC**

![](./images/1581080526200.png)
![](./images/1581080641832.png)
```
使用 EWC，会为每一个参数设置一个守卫 bi，可以算每一个参数的二次微分。
从 θ1 的方向来看，θb 落在了一个平坦的盆地里面，改变 θ1 的参数，对 loss，并不会太大，就可以给 b1 比较小的值，就是说在 train 的过程中，θ1 这个参数被动到了也没有关系，因为 θ1 对 task1 的 loss 影响不大。
从 θ2 来看，θ2 落在了峡谷之中，θ2 的二次微分值比较大，就给 b2 比较大的值，如果改变 θ2 的参数，就会对 task2 的 loss 影响就会很大，就要把 θ2 这个参数保护起来，在之后训练的过程中，尽量不要动到 θ2。
```
![](./images/1581080891463.png)
![](./images/1581081015138.png)

>**Generating Data**

![](./images/1581083183717.png)
```
训练一个可以生成 task1 data 的 model，然后将生成的 task1 data 加入 task2 中，训练出一个可以生成 task 1&2 的 model，...
```
![](./images/1581083393802.png)

### Knowledge Tranfer

![](./images/1581083455004.png)

>**Wait a minute**

![](./images/1581083609007.png)
```
为什么要为每一个任务训练为一个 model？
1、因为分别为每一个任务 train 出一个 model，每一个 model 是没法相关联的，这不是最终想要的，希望 train 出一个 model，将来会对新 train 的 model 有帮助
2、容量太大，存不下。
```
>**Life-Long vs. Transfer**

![](./images/1581083930807.png)
```
Transfer Learning: 当学习 task2 的时候，是不关心 task1 的。
Life-Long Learning: 比 Transfer Learning 多考虑一步，希望 train task2 的时候做的更好，还能使得 task1 的做的好。
吃烧饼不掉芝麻。
```

>**Evaluation**

![](./images/1581085613981.png)
![](./images/1581085642412.png)
```

```
![](./images/1581085967578.png)
![](./images/1581086079036.png)


### Model Expansion

![](./images/1581086238498.png)

>**Progressive Neural Networks**

![](./images/1581086419763.png)
```
Task2 接收 Task1 的 output，以此类推，这种方式不太实用。
```
>**Expert Gate**

![](./images/1581086560752.png)
```
model 的参数成长速度和 model 成长速度和 task 的增加速度几乎一样快。
```
>**Net2Net**

![](./images/1581086938632.png)
```

```

>**Curriculum Learning**

![](./images/1581087201357.png)
```
任务的顺序会影响 机器学习的进度
```
![](./images/1581087626837.png)
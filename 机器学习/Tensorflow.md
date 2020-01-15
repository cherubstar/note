---
title: Tensorflow
renderNumberedHeading: true
grammar_cjkRuby: true
---

>**Tensorflow 基本概念**

![](./images/1578624929208.png)

>**Tensorflow 结构**

![](./images/1578625153945.png)
```
图代表一个计算过程，图在 Session 中进行。
```

>**激活函数**

![](./images/1578971669536.png)

```
思考问题
为什么要用激活函数？
```

>**归一化与批归一化**

![](./images/1578971870671.png)
![](./images/1578971975863.png)

```
经过归一化的是一个正圆，法线方向是圆心。归一化之后训练速度会加快。
思考问题
为什么归一化有效？
```

>**Dropout**

![](./images/1578972047777.png)
```
把一些 neuron 弃用掉。弃用是随机的，训练模型的时候，这次弃用的单元数和下次弃用的是不一样的。
```
>**这样随机的丢弃 neuron 有什么作用？**

![](./images/1578972188724.png)
```
思考问题
为什么 Dropout 有效？
```

>**访问 Tensorboard 界面**
>**tensorboard --logdir=callbacks**

![](./images/1579007088641.png)

>**127.0.0.1:6006**

![](./images/1579007156239.png)

>**Wide & Deep 模型**

[Wide & Deep 模型](https://arxiv.org/pdf/1606.07792v1.pdf)


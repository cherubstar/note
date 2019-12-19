---
title: Gradient Descent
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### 1、Review
![](./images/1576720844875.png)
![](./images/1576721059408.png)
```
计算出 Gradient，然后和 η：learning rate 相乘取负值，再和 θ相加计算出新的 θ再计算 Gradient。
```

### 2、Tuning learning rates

![](./images/1576723706258.png)
```
蓝色箭头：η过小，梯度下降的比较慢
红色箭头：η调整适度刚好
绿色箭头：η过大，没有办法走到特别低的地方
黄色箭头：η太大，update 参数以后，loss 越来越大

右边的坐标图显示的是 参数的调整对 Loss 的变化，所以在做 Gradient Descent 的时候应该把这个图画出来，这样就能实时观测到 Loss 的变化，从而适度调整 η的值
```



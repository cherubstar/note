---
title: Logistic Regression 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### 1、Funcion Set

![](./images/1577408282744.png)
![](./images/1577409783142.png)
```
所有 w & b 集合起来就是 function set
posterior probability
x 属于 C1 的几率
```

### 2、Goodness of a Function

![](./images/1577441285117.png)
```
其中 L(w,b) 是由线性方程的概率计算的
找到最好的 w*,b*，则需要使得 L(w,b) 最大
```
![](./images/1577443701437.png)
![](./images/1577443596536.png)
```
找到 w*,b*，使得L(w,b)最大化，等同于找到 ln L(w,b)，取 L(w,b) 的 ln，order是不会变的，找到 w*,b* 最小化 -ln L(w,b)
左边方程和右边方程是相等的，找到的是同一个 w*,b*。
```
![](./images/1577446329425.png)
```
伯努利方程
```
### 3、Find the best function

![](./images/1577447308755.png)
![](./images/1577447435927.png)
```
划减之后结果
```
![](./images/1577447754017.png)
```
w 的 update 取决于三部分
·η：Learning rate 自定义的
·xi：来自于 training data
·(y^n - f(xn))：代表现在的 output 和理想的目标的差距有多大
```

>**Logistic Regression 和 Linear Regression 的对比**

![](./images/1577448728381.png)

>**解释为什么 Logistic Regression 不能使用 Square Error**

![](./images/1577449214834.png)
![](./images/1577449298749.png)
```
y^n = 1 
y^n = 0
```
![](./images/1577449707784.png)
[内容来自于 PDF 文档](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf)
```markdown
Cross Entropy: 当独立目标值很近的地方，微分值很小，当距离目标值很远的地方，微分值很大
Square Error: 距离目标值很近或者很远的地方，微分值都很小

当随机选取一个初始值时，如果使用 Square Error 时，距离远的初始值和距离近的初始值的微分值都很小，无法确定 Gradient Descent 计算出来的微分值小的时候距离目标值的距离。
```
>**Discriminative VS Generative**

![](./images/1577453681661.png)
```
左边和右边找出来的 (w,b) 不是同一组
```
![](./images/1577463232690.png)
```
Discriminative 要比 Generative 好
```
>**Example**

![](./images/1577463470698.png)
```
使用 Naive Bayes :朴素贝叶斯;朴素贝叶斯算法;朴素贝叶斯方法;
```
![](./images/1577463812278.png)
![](./images/1577464064392.png)
```
使用 Naive Bayes 计算出来的结果属于 Class2
```

![](./images/1577464387567.png)
```
Discriminative 不是总是比 Generative 好
Discriminative 会由于 data 的数量影响，data 越多，它的 error 越小
```

> **Multi-class Classification**

![](./images/1577464764956.png)
![](./images/1577465126021.png)
```
归一化
概率论
```
>**Limitation of Logistic Regression**

![](./images/1577465252268.png)
![](./images/1577465316790.png)
```
Logistic Regression 有一定的局限性
```

>**解决方法**

![](./images/1577465484462.png)
```
将不好区分的映射到另外一个好区分的空间，进行 boundry 划分
```
![](./images/1577465692817.png)
```
可以把很多个 Logistic Regression 连接起来
两个 input, x1、x2,通过 sigmod 函数输出为 x1',x2'，如果在新的 transform 上面是可以用一条直线划分的，那么最后只要再接另一个 Logistic Regression Model。
```
![](./images/1577466135146.png)
![](./images/1577466249835.png)
![](./images/1577466321138.png)
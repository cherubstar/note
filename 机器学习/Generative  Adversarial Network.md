---
title: Generative Adversarial Network 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

### Introduction

![](./images/1581243846564.png)
![](./images/1581243938600.png)
![](./images/1581252934972.png)
![](./images/1581252986224.png)

### Basic Idea of GAN

![](./images/1581253244715.png)
```
在 Generation 里面需要做的就是训练出一个 generator，从 Gaussian distribution random sample 一个 vector，把这个 vector 丢到 generator 里面，generator 就要产生一张 image，丢不同的 vector 就会产生不同的 image。
Conditional Generation：条件生成
输入文字让机器产生对应的图片，输入图片，让机器产生另一张图片。
```
![](./images/1581254118780.png)
```
vector -> Generator -> image(high dimensioal vector)
通常输入的每一个 dimension 会对应到图片的某种特征，也就是说，改变了其中一个 dimension 的数值，产生图片的某种特征有所改变。
```
![](./images/1581254988025.png)
```
假设要产生图片，就输入 image，假设要产生句子，就输入 句子
输出是一个 scalar，表示产生出来的这张 image 的 quality，这个 scalar 越大，产生出来的 image 的 quality 越高，看起来越像是真是的 image，看起来越 realistic。
```

>**Generator v.s. Discriminator**

![](./images/1581255467531.png)
![](./images/1581255712268.png)
![](./images/1581255871980.png)
![](./images/1581255976268.png)
```
相互依赖
```
>**Algorithm**

![](./images/1581256325691.png)
![](./images/1581256837381.png)
```
Step 1:
	固定 generator G，update discriminator D。
	怎么调整 discriminator 的参数呢？
	如果是从 database 中 sample 出来的 image 就给高分，
	如果是 generator 生成的 image 就给低分。
Step 2:
	固定 discriminator D，update generator G。
	怎么调整 generator 的参数呢？
	把一个 vector 丢到 generator 里面，generator会产生一张图片，把这张图片丢到 discriminator 里面，discriminator 会给出一个分数。
	固定住 discriminator 的参数，只去调整 generator 的参数。
	把 generator 和 discriminator 和起来，一块运行。固定最后几个 hidden layer，只调前面几个 hidden layer，让 output 的值越大越好。使用 Gradient Ascent(就是 - Gradient Descent)
```
![](./images/1581257746836.png)
```

```

![](./images/1581259259455.png)
![](./images/1581259290628.png)
```
可以计算两个 vector 之间的内差。
```
### GAN as structured learning

![](./images/1581259408512.png)

>**Structured Learning**

![](./images/1581259500882.png)
![](./images/1581259685749.png)
![](./images/1581259864876.png)
```
Regression、Classification and so on.
Machine Translation
Speech Recognition
Chat-bot
```
![](./images/1581260120751.png)
![](./images/1581260291793.png)
```
如果机器需要解决 Structured Learning 的问题，必须有规划的概念，有大局观。在 Structured Learning 里面，真正重要的不是产生了什么 components，而是 component 和 component 之间的关系。
```

![](./images/1581260699378.png)
```
GAN 是 Structured Learning 的 solution。
Bottom Up: 自下而上
Top Down: 自上而下
```

### Can Generator learn by itself?

![](./images/1581260766215.png)
![](./images/1581261123213.png)
![](./images/1581261234170.png)
```
怎么产生 vector?
learn 一个 Encoder。
```
![](./images/1581261347897.png)
![](./images/1581261462713.png)
```
希望 input 和 output 越接近越好。
Decoder 就相当与 Generator。
```
![](./images/1581261569455.png)
![](./images/1581261961280.png)
![](./images/1581262113341.png)
```
遇到的问题可能是？
Training data 的 image 是有限的。所以对 0.5*a + 0.5*b 无法确定生成的结果。
```
![](./images/1581262325892.png)
```
Variational Auto-Encoder(VAE)
可以解决 0.5*a + 0.5*b 无法确定生成的结果的问题。
```

![](./images/1581262985524.png)
![](./images/1581263153751.png)
```
generator 的目标就是它的 output 跟某一张图片越像越好，什么叫做越像越好呢？
通常计算的是这两张图片 pixel 和 pixel 的差距。把这两张图片表示成两个 vector，算这两个 vector 的 Euclidean Distance(欧氏距离)。
真正在 training 的时候，generator 是会犯错的。在什么位置犯错就变得很重要了。
```
![](./images/1581263386003.png)
```
如果不想用 discriminator，想单纯的用 Auto-Encoder 做 Generator 这件事，假设有同样的 Network，一个用GAN train，一个用 Auto-Encoder train，往往用 GAN 的能产生图片，Auto-Encoder 则需要一个更大的 Network 才能产生和 GAN 相近的结果。
```
![](./images/1581263795478.png)
```
使用 Auto-Encoder 遇到的问题。
```

### Can Discriminator generate?

![](./images/1581263872770.png)
![](./images/1581264051942.png)
![](./images/1581264265862.png)
```
Discriminator 是可以产生 image，但非常卡。
在不同的领域中有不同的叫法。
Evaluation function、Potential function、Energy function
当产生完一张完整的图片以后，检查这张图片里面 component 和 component 之间的关系是比较容易的。
```
![](./images/1581264517685.png)

![](./images/1581264635586.png)
![](./images/1581264747381.png)
```

```
![](./images/1581264863259.png)
![](./images/1581264921687.png)
```

```

![](./images/1581265043382.png)
![](./images/1581265299582.png)
```

```

![](./images/1581265444085.png)

![](./images/1581265575396.png)
![](./images/1581265674933.png)

>**Benefit of GAN**

![](./images/1581265730788.png)

![](./images/1581265860870.png)

![](./images/1581265980723.png)

![](./images/1581266037495.png)
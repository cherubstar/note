---
title: Introduction
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1582458032785.png)
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
假设要产生图片，就输入 image，假设要产生句子，就输入句子
输出是一个 scalar，表示产生出来的这张 image 的 quality，这个 scalar 越大，产生出来的 image 的 quality 越高，看起来越像是真是的 image，看起来越 realistic。
```

>**Generator v.s. Discriminator**

![](./images/1582458265094.png)
![](./images/1582458225309.png)
![](./images/1582458332347.png)
```
相互依赖
```
>**Algorithm**

![](./images/1582458418478.png)
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
![](./images/1582458525507.png)
```
可以计算两个 vector 之间的内差。
```
### GAN as structured learning

![](./images/1581259408512.png)

>**Structured Learning**

![](./images/1581259500882.png)
![](./images/1581259685749.png)
![](./images/1582458625162.png)
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

![](./images/1582458912695.png)
![](./images/1582458866321.png)
```
generator 的目标就是它的 output 跟某一张图片越像越好，什么叫做越像越好呢？
通常计算的是这两张图片 pixel 和 pixel 的差距。把这两张图片表示成两个 vector，算这两个 vector 的 Euclidean Distance(欧氏距离)。
真正在 training 的时候，generator 是会犯错的。在什么位置犯错就变得很重要了。
```
![](./images/1582458791315.png)
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
```
依赖 x~ = arg max D(x) 产生
假设已经有了一个 discriminator，这个 discriminator 可以实现鉴别好坏。
怎么使用 discriminator 做生成呢？
穷举所有可能的 x，一个一个丢到 discriminator 里面，看哪一个 x，discriminator 可以给它最高的分数，discriminator 给高分的 x 就是生成的结果。
```
![](./images/1581264635586.png)
![](./images/1581264747381.png)
```
当在训练 discriminator 的时候，给它很多好的 example，告诉它这是高分的，给它很多烂的的 example，告诉它这是低分的。
但是 discriminator 经常会有 positive example，只有正面的例子，会出现看的什么都会觉得是正面的，都会给高分。
有好的 Negative example 才能 train discriminator。
```
![](./images/1581264863259.png)
![](./images/1581264921687.png)
```
使用 Iteration 的方法解决，怎么训练 discriminator？
假设有一堆 positive example、negative example
	positive example：人画的
	negative example：random sample 出来的
在每一个 iteration 里面，就是给 positive example 高的分数，给 negative example 低的分数。
当学出来一个 discriminator 以后，可以用 discriminator 做 generation。(discriminator 是可以做 generation 的，只要会解 arg maxD(x)，用这个 discriminator generate 出一些它觉得好的 image，这些 image 用 x~ 表示)
在下一个 iteration 里面，将原来 random 的 image 换成它觉得好的 image。
反复循环下去
```

![](./images/1581265043382.png)
![](./images/1581265299582.png)
```

```

![](./images/1581265444085.png)
```
GAN 可以被视为是 Structured Learning 的一个
```

![](./images/1581265575396.png)
```
1.Generator
Pros:
	很容易做生成
Cons：
	不容易考虑 component 和 component 之间的 correlation，只学了 pixel 和 pixel 之间的相似程度，学不到大局。

2.Discriminator
Pros:
	可以考虑大局
Cons：
	不容易生成，需要解一个 arg max D(x)
```
![](./images/1581265674933.png)
```
generator 取代了 arg max D(x)，generator 就是在学怎么去解 arg max D(x) 这个 problem。
```

>**Benefit of GAN**

![](./images/1581265730788.png)
```
用 generator 来解 arg max D(x)，更加有效，更加一般化。
```
![](./images/1582459225195.png)
![](./images/1581265980723.png)
![](./images/1581266037495.png)
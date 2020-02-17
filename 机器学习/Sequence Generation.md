---
title: Sequence Generation 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


### Outline

![](./images/1581902143903.png)

### RNN with Gated Mechanism

#### Recurrent Neural Network

![](./images/1581914699883.png)
```
RNN 特别的地方是里面有一个 basic function，这个 basic function 是 f 表示。在 RNN 里面会被反复的使用，input 是两个 vector (h,x)，output 是另外两个 vector (h',y)，处理 RNN 使用的情景是 input 是 vector sequence。
现在这个 vector sequence 用 x1,x2,x3 表示。
f 是把 (x1,h0) 当成 input，generate 两个 vector (h1,y1)，新的 x2 进来，同样的 f 会被反复的再用一次。
假设要反复使用 function f，function f 所有的 h0、h1、hi dimension 是一样的。
比较 powerful 的地方，不管 input sequence 有多么长，他都是可以的。RNN 都只有一个 basic function。RNN 的参数量完全不会随着 input sequence 的长度而改变。
```

#### Deep RNN

![](./images/1581914721621.png)
```
本来有 basic function f1，再加一个 basic function f2，f2 的 input 是 f1 的 output y1 和 initial vector b0，output 就是 (x1, c1)
```

#### Bidirectional RNN

![](./images/1581914754006.png)
```
有两个 function，f1 和 f2，f2 的 input sequence 的方向 f1 不一样。
f1 先 input x1、x2、x3，产生一排 vector
f2 先 input x3、x2、x1，产生一排 vector
需要有另外一排 function f3，把生成的 vector input 进去，得到 network 最终的 output。
```

####  Naive RNN

![](./images/1581914831267.png)

#### LSTM

![](./images/1581914864636.png)
![](./images/1581914894736.png)
![](./images/1581943894681.png)
```
要想做的更好，可以使用 peephole，本事是把 x 和 h 串在一起，现在是把 x、h、c 串在一起，合成一个更长的 vector。乘以一个更长的 matrix，通过 tanh，得到 z。
因为 input vector 变长了，所以 W 的参数变多了，实际上这样做 performance 并不会比较好。参数变多容易 overfitting，在实作上强迫 matrix  的一部分是 diagonal。
x * W 的前 1/3
h * W 的中间 1/3
c * W 的 diagonal 部分
这样可以减少参数的使用量
```
![](./images/1581914969428.png)
```
input 的 ct-1 和 output 的 ct 有什么关系？
ct = zf⊙ct-1 + zi⊙z，变化比较小。有一些不太想要改变的资讯可以放在里面。之间没有 activation function。
ht = zo⊙tanh(ct)
yt = σ(W'ht)
```
![](./images/1581914998760.png)
```
同样的 block 会重复使用，RNN 的特殊之处就是 function 会被反复使用。
```

#### GRU

![](./images/1581915053929.png)
```
GRU 和 LSTM 挺像的，在 GRU 里面，h 的角色较像 LSTM 里面的 c。
input x、h，将 x 和 h 接起来，乘以一个 transform。
```

>**LSTM: A Search Space Odysey**

![](./images/1581915081984.png)
![](./images/1581915114854.png)


### Sequence Generation

![](./images/1581915162462.png)
```
怎么让机器产生一个 sentence？
要做 generation 的话，写一个句子的话，可以用 character 为单位，也可以用 word 为单位。
RNN 是一个 basic function，input (h,x)，output (h',y)，在做 generation 的时候，x 是前一个时间点所产生的 token(不管是 character 还是 word)
怎么把 word 或者 character 描述成一个 vector，最简单的方法就是 1-of-N encoding，就是假设知道英文有 10万 个word，就开一个 10万 维的 vector，这个 vector 只有一维是 1，其他维是 0。这个 vector 的每一个 dimension 都对应一个 word。比如某一个对应 apple，那 apple 那个值是 1，其他是 0。

input token 之后，y 是现在 machine 要产生出来的 token 的 distribution(y 不是一个 token，而是 token 的 distribution)，但是让机器产生的不是 token distribution，而是某一个 token，某一个 word or character。

最后实际上会做的事情是从 y 的 distribution 里面做 sampling，在这个 distribution 里面 sample 出 token。 
```
![](./images/1581949658993.png)
```
machine 实际上是怎么产生一个完整的句子。
在一开始的时候，在产生句子的第一个 character 的时候，前面没有产生任何东西，这时候会给机器一个特殊的字源，只有在句子的开头才会发生，通常第一维就代表 <BOS> Begin Of Sentence，对应这个 vector 的第一个 dimension，把这个 token input 进去，就会告诉在句首的时候应该放哪一个 character。
y1:P(w|<BOS>)：给定句首这个 token 的时候，某一个 character 出现的几率，或者换句话说，某一个 character 被放在句首的几率。

什么时候停止呢？
会设置一个特别的符号 <EOS> End Of  Sentence
```
![](./images/1581949691419.png)
```
怎么找出这个 basic function f 呢？
首先给 f 训练资料，当给 basic function f <BOS> 这个 token 时，它第一个 word distribution 和春越接近越好，会 minimize y1 和'春' 的 cross-entropy。
和分来问题是一样的，分类中 model 的 output 是一个 distribution，正确答案是 1-of-N encoding。
```
![](./images/1581949742226.png)
```
可以让机器画个图。
image 是由一大堆的 pixels 组成的。可以把每一个 pixel 看成一个字，image 就看做一个 sentence。
```

>**PixelRNN**

![](./images/1581949794201.png)
```
pass
```

### Conditional Sequence Deneration

![](./images/1581949833850.png)
```
到刚刚为止 train 出来的 RNN，只能写出一些随机的诗，没有办法即景生情。
```
>**Image Caption Generation**

![](./images/1581951224419.png)
```
给机器看一张 image，然后告诉你 image 中有什么东西。
不是要机器随便说一句话，而是根据输入说一句话。
怎么影响机器根据 input image 说一句话呢？
需要把 image 的资讯 input RNN 中。
把 image 通过 CNN 变成一个 vector，把这个 vector 在每一个 text step 也丢到 RNN 里面去。
```
>**E.g.: Machine translation**

![](./images/1581951269092.png)
```
在 Machine translation / Chat-bot 中，input 是一个 sentence，把 sentence 变成一个 vector，就和 Image Caption Generation 中间的过程一样。
怎么把 sentence 变成一个 vector？

机器学习 -> RNN -> Machine Learning
只要最后一个时间点 output 的 y。最后一个时间点 output 的 y 可能包含了 input 所有的 information。有的时候不见得会取 y，可能会取 h、c、(h,c) 的 connect。都可以。

将得到的红色的 vector 丢到 generator 里面，每一个 text step 都 input 一次。希望可以产生 machine learning.。

在 training 的时候，
前面部分叫做 Encoder：把 sequence 变成 vector
后面部分叫做 Decoder：把 vector 变成 sequence
这种方式叫做 Sequence-to-sequence learning，Sequence-to-sequence model

Sequence-to-sequence 的技术应用非常广，语音合成，
```

>**E.g.: Chat-bot**

![](./images/1581951298249.png)

#### Dynamic Conditional Generation

![](./images/1581951334809.png)
```
Encoder：把 sequence 变成 vector
Decoder：把 vector 变成 sequence


会遇到问题：也许 Encoder 没有足够的能力把所有的东西都压到一个 vector 里面去。Decoder 在每一个 text step input 都是同样的 vector。
Dynamic Conditional Generation 想要做到的事情是 Decoder 在每一个 text step input 的 information 是不一样的。

h1,h2,h3,h4 表示 RNN 的 output，把 h1,h2,h3,h4 想成一个 database，把 h1,h2,h3,h4 这 4 个 vector 存在 database 里面。

在每一个 text step，Decoder input 的 conditional representation 都是不一样的。至于要 input 什么样的资讯，由 Decoder 自己取去看 Encoder 所存的 database，自己决定。
```

#### Machine Translation

![](./images/1581951369073.png)
```
Attention-based model 怎么做的呢？
input 一个中文的 character sequence，每一个 character 依序 input RNN 里面，RNN 的每一个 text step 都会产生一个 vector，(h1,h2,h3,h4)，RNN 把 (h1,h2,h3,h4) 存起来，形成了一个 database。
z0：它是搜寻这个 database 的关键字。
z0 是怎么搜寻 database 的呢？
z0 和 (h1,h2,h3,h4) 每一个 vector 算一个匹配程度。

match 是什么？
方式1：计算 h 和 z 的 inner product(内积)，然后得到 α。
方式2：把 match 当成一个小的 NN，
方式3：α = hTWz

假设 match 里面是有参数的(比如方式2，match 是个 NN)，不需要把 match 额外的参数找出来，match 是和整个 NN 合在一起学的。
```

![](./images/1581951418854.png)
```
(α01,α02,α03,α04) 可以做一个 sofemax，(做一个 normalization，让它们和是 0，可做可不做)。
得到 (α^01,α^02,α^03,α^04) 值以后，c0 = ∑a^0i*hi。
把 c0 当成 Decoder 的 input，Decoder 每一个 text step 都会 input 不同。
根据 z0 c0，Decoder 就会 output 一个 word。
```
![](./images/1581955359108.png)
![](./images/1581955537580.png)
```
z1 和 h1 计算一个 match，得 α11
z2 和 h2 计算一个 match，得 α12
z3 和 h3 计算一个 match，得 α13
z4 和 h4 计算一个 match，得 α14

然后再和第一步一样计算下去。
```
![](./images/1581955632642.png)

#### Image Caption Generation

![](./images/1581955741941.png)
```
有一张 image 进来，一整张 image 用一个 vector 来表示，input 给 decoder，但这样太粗了，decoder 没有办法把一整张 image 里面各个细节统统存在一个 vector 里面。把 image 切成一块一块的。现在一张 image 就是一把 vector。
```
![](./images/1581955770808.png)
```
z0 和这一把每一个 vector 都计算 match 的 score，计算的结果不一样，每一个 vector 都乘以计算出来的 score，把他们 weighted sum(加权和) 起来，input 到 decoder 里面，就会告诉产生的 word 是什么。
```
![](./images/1581955803323.png)
```
以此类推
```

#### Example

![](./images/1581956430618.png)
![](./images/1581956496277.png)
```
有的时候 machine 会犯错。
```

### Tips for Generation

![](./images/1581951552971.png)
```
有什么样的 tip 呢？
对 match 的 score 做一些限制。
希望 machine 在看一篇影片的时候，在看 input 的时候，它的 attention 是平均的。
对 attention 下一个 Regularization weight，下一个 constraint，summarization over t 对同一个 friend，不同时间点的 attention 的总和。
比如，把 (a11,a21,a31,a41) 和起来，希望跟某一个值 τ 越接近越好。
```

![](./images/1581957302265.png)


![](./images/1581957333334.png)




![](./images/1581957375660.png)



![](./images/1581957429866.png)


![](./images/1581957466755.png)


![](./images/1581957490265.png)


![](./images/1581957535584.png)

![](./images/1581957578235.png)


![](./images/1581957627173.png)

![](./images/1581957655011.png)


![](./images/1581957689157.png)


### Pointer Network



### Sequence Generation for setting Hyperparameters
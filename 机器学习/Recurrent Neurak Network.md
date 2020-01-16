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

![](./images/1579089991804.png)
```
input 是一个 sequence，在使用 RNN 之前，需要给 memory 初始值。
激活函数是 linear 的。
[1] -> [4]，[2] 就会被存储到 memory 中。
[1]	   [4]  [2]
```
![](./images/1579090159048.png)
```
在输入 [1] -> [12]，[6] 就会被存储到 memory 中
			 [1]	[12]  [6]
```
![](./images/1579090463126.png)
```
在输入 [2] -> [32]，[16] 就会被存储到 memory 中
			 [2]	[32]  [16]
...			 
如果改变 sequence 的 order，也会有不同的 output。
```

![](./images/1579090709198.png)
```
同一个 network 在三个不同的时间点使用了三次。这里同样的 weight 用了同样的颜色来表示。
```
![](./images/1579093791444.png)
```
这样就有了 memory 之后，输入同样的词汇，输出不同的 output 这个问题就有可能被解决。比如输入 Taipei 之后，红色 Taipei 前面接入的是 leave，绿色 Taipei 前面接入的是 arrive。因为 leave 和 arrive 的 vertor 不一样。所以 hidden layer 的 output 也会不同。所以存在 memory 里的值也会不同。虽然 x2 是一样的，但是因为存在 memory 里的值是不同的，所以 hidden layer 的 output 是不一样的。最后的 output 也是不一样的。
```

![](./images/1579094058258.png)
```
RNN 可以是 Deep 的。
```

> **Elman Network & Jordan Network**

![](./images/1579094231240.png)
```
Elman Network: 是把 hidden layer 的值存起来，在下一个时间点读出来。
Jordan Network: 存储的是整个 network 的 output 的值，在下一个时间点读出来。
相比之下，Jordan Network 可能要比 Elman Network 好一些，因为 Elman Network 没有 target 的，有点难控制他学到什么。Jordan Network 是有 target 的，比较清楚放在 memory 里面的是什么。
```

> **Bidirectional RNN**

![](./images/1579094507879.png)
```
双向 RNN，同时可以 train 一个正向的 Neural Network 和一个负向的 Neural Network。把俩个 hidden layer 的 output 拿出来，都接给 output layer，得到最后的 y。
好处：network 产生 output 时候，看的范围是比较广的。network 看的不只是从 x1 到 xt 的所有的 input，还看了从句尾到 xt 的 output。
```

**>Long Short-term Memory(LSTM)**

![](./images/1579095272960.png)
```
Input Gate: 
Output Gate:
Forget Gate:
都是可以自己学出来的。
4 inputs，1 output。
```

![](./images/1579095781394.png)
```
c' = g(z)f(zi) + cf(zf)，c' 就是新的存在 memory 中的值。
f(zi) 就是 control g(z) 可不可以 输入的关卡，假如当 f(zi) = 0，g(z)f(zi) = 0，就好像没有输入一样; f(zi) = 1，g(z)f(zi) = f(zi)，就等于直接把 f(zi) 输入一样; 
f(zf) 就是要不要把存在 memory 中的值清洗掉，f(zf) = 1，Forget Gate 开启的时候，c 会直接通过，就被保留下来; f(zf) = 0，Forget Gate 关闭的时候，memory 里的值就会等于 0。
c' 通过 function h 得到 h(c')，output 受 zo 操控，f(zo) = 1，h(c') 就可以通过 output gate; f(zo) = 0，output 就等于 0，存在 memory 里的值就没办法被读取出来。
```

>**LSTM - Example**

![](./images/1579097195473.png)
![](./images/1579097545277.png)
```
input gate 和 output gate 通常是关闭的，forget gate 通常是开启的。
```
![](./images/1579097659538.png)
```
[x1, x2, x3] = [3, 1, 0]
y = 0
c = 3
```
![](./images/1579097890902.png)
```
[x1, x2, x3] = [4, 1, 0]
y = 0
c = 7
```
![](./images/1579097990671.png)
```
[x1, x2, x3] = [2, 0, 0]
y = 0
c = 7
```
![](./images/1579098104586.png)
```
[x1, x2, x3] = [1, 0, 1]
y = 7
c = 7
```
![](./images/1579098163766.png)
```
[x1, x2, x3] = [3, -1, 0]
y = 0
c = 7
```

![](./images/1579098564893.png)
```
LSTM 原来 Neural Network 的 4 倍的参数。因为 LSTM 需要操控另外三个 Gate。
```

![](./images/1579100943399.png)
```
假设有一整排的 LSTM，每个 LSTM 里面的 memory 都存了一个值。每一个 LSTM 中的 cell 都存了一个 sclar，把所有的 sclar 接起来就是一个 vector。
input a vector xt，xt 首先会乘以一个 Linear transform（一个 matrx），变成另一个 vector，z 中的每一个 dimension 就代表了操控每一个 LSTM 的 input，z 的 dimension 正好是 LSTM 的 memory cell 的数目。
```

![](./images/1579101047375.png)
```
xt 乘以另外一个 transform 变成 zi，zi 的 dimension 也和 cell 的数目一样。每一个 dimension 都会操控一个 memory。
```

![](./images/1579101224965.png)
```
xt 乘以另外一个 transform 变成 zf，zf 的 dimension 也和 cell 的数目一样。每一个 dimension 都会操控一个 memory。
xt 乘以另外一个 transform 变成 zo，zo 的 dimension 也和 cell 的数目一样。每一个 dimension 都会操控一个 memory。

xt 乘以 4 个不同的 transform 得到 4 个不同的 vector，这 4 个 dimension 都和 cell 的数目是一样的。
```
![](./images/1579101361203.png)
![](./images/1579101495460.png)
```
这 4 个 vector 是可以同时计算的，相加的结果就是 memory 中的值。这只是 simple 形态。
```

![](./images/1579101655296.png)
```
会把上一个 hidden layer 的 output 当成下一个时间点的 input。
```

>**Multiple-layer LSTM**

![](./images/1579101744155.png)
![](./images/1579101877867.png)
```
Keras 支持三种 RNN，LSTM、GRU、SimpleRNN。
```

>**在 RNN 中定义 Loss Function**

![](./images/1579153595229.png)
```
把 arrive 丢到 RNN 中去，会得到一个 output y1，y1 会和 reference vector 计算 cross entropy。在做 training 时候，不能把 word sequence 打散，而是当成一个整体来看。
Loss Function = Cross Entropy 之和
```

![](./images/1579154252042.png)
```
RNN 是可以用 Gradient Descent 训练的。
```

![](./images/1579154451503.png)
![](./images/1579156161219.png)
```
RNN 不是那么容易被 train 的。
RNN 的 error surface，total loss 对参数的变化是非常陡峭的。有一些地方非常平坦，有一些地方非常陡峭。
图上显示的是 w1，w2 对 total loss 的影响。
activation function 不是这个问题的关键点。
```

![](./images/1579155515821.png)
```
RNN 在 train 的时候有问题。
当输入[1,0,0,0,0...] 的时候。改变 w 的之后，对 network 的 output 有多大影响。
w 有小小的变化，output 就会有很大的影响。
当 gradient 很大的时候，learning rate 设置小一点。
当 gradient 很小的时候，learning rate 设置大一点。

RNN 为什么会有问题？
RNN 的 training 问题来自于它把同样的东西在 transozaton 的时候，在时间和时间转换的时候，反复使用，在 neuron 接到 memeory 的那一组 weight，不同的时间点都是被反复使用的。所以 w 一有变化， 它有可能没有造成任何影响，如果可以造成影响，影响是天崩地裂的。RNN 不好训练的原因并不是来自于 activation，而是同样的 weight 在不同的时间点被重复使用。
```

>**解决这个问题的技巧**

![](./images/1579163030443.png)
```
LSTM 可以把一些平坦的地方拿掉。
LSTM 可以解决 Gradient Vanishing 的问题，但不能解决 Gradient Explode。

面试会问？
为什么 把 RNN 换成 LSTM？
因为 LSTM 可以 handle Gradient Vanishing 的问题。
LSTM 的 memory 是相加的，会一直存在。
RNN 中的每个时间点都会被 forget 掉。
```

![](./images/1579163249578.png)
```
其他可以解决 Gradient Vanishing，
Clockwise RNN
Structurally Constrained Recurrent Network(SCRN)
```

>**More Applications**

![](./images/1579163693884.png)
```
1、input 一个 vector sequence，output 一个 vector。
把最后一个 hidden layer 通过几个 transform 就得到最后的 axis。最后是一个分类问题，可以用 RNN 处理这个问题。
```

![](./images/1579164237216.png)
```
2、Key Term Extraction：
```

![](./images/1579164501136.png)
![](./images/1579164595358.png)
![](./images/1579164827394.png)
```
More to More
当 output 比 input 短的时候。
e.g:语音辨识
CTC: 使用 extra symbol Φ，解决叠字的问题。
```
![](./images/1579164963504.png)
```
将中间的 Φ 去掉，再拼接。
```

![](./images/1579165334772.png)
```
Sequence to Sequence learning: 不确定 input 和 output 谁长谁短。
e.g: Machine Translation，将 input 英文 word sequence 翻译成 中文的 character sequence。
memory 里面就存了所有 input 的 sequence 的 information。
```

![](./images/1579165529658.png)
![](./images/1579167271501.png)
```
就开始 output character sequence。Machinie Learning 不可能 output 所有可能，它还有一个可能的 output 是 "==="(断)
```

![](./images/1579167731529.png)
```
e.g: Syntactic parsing
```
![](./images/1579167810817.png)
```
有相同的词汇，但是 word 的 order 不同，有不一样的结果。
```
![](./images/1579167971318.png)
```
把一个 document 变成一个 vector，Input a Sequence：
Mary was hungry.she didn't find any food.
通过一个 RNN 变成一个 invalid vector，再把这个 invalid vector 当作 Decode 的输入，让这个 Decode 找回一个一摸一样的句子。
Sequence-to-Sequence Auto-encoder 还有另一个版本，sigsor。
如果使用 Sequence-to-Sequence Auto-encoder，input 和 output 都是同一个句子。得到的 code 比较容易表达文法的意思。
如果使用 sigsor，output target 会是下一个句子。得到的 code 比较容易表达语义的意思。
```
![](./images/1579168664530.png)
```
每一个句子都可以得到一个 vector，
Mary was hungry 得到一个 vector1，
she didn't find any food 得到一个 vector2，
再把这两个 vector1、vector2 加起来，变成一个整个 high label vector，
再将整个 high label vector 产生一串 sentence vector，
再根据每一个 sentence vector 解回 word sequence。
这是一个 4 层的 LSTM。
```
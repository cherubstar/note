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
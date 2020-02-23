---
title: MAML
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1581089585071.png)
```
让机器学习如何做学习这件事。
机器学过一大堆任务以后，根据过去任务所汲取的经验，变成了一个 learner，往后有一个新的任务，它可以学习的更快。
例如：
在过去学习了 speech recognition，image recognition，那在做 text classification 会做的更好。
Life-Long: 不断的用同一个 model 来做学习，同一个 model 学习很多技能。
Meta: 不同的任务有自己的 model，机器可以从过去的学习经验里面学到一些东西使得未来要去 train 一个新的 model 的时候，可以训练的更快更好。
```
![](./images/1581090304896.png)
```
让机器自动的找出 learning algorithm，找出生成 function 的 function。
```
![](./images/1581090432906.png)
```
Machine Learning: 找出一个函数 f 的能力
Meta Learning: 找出一个可以生成 function 的函数 F 的能力。
```
![](./images/1582449641925.png)
```
1、define a set of Learning algorithm
2、goodness of Learning algorithm
3、pick the best Learning algorithm
```

### Define a set of Learning algorithm

![](./images/1581154410429.png)
```
准备一组 learning algorithm，在将 Deep Learning 的时候，当定出 Neuron Network 的时候，就有了 function set，
```

### Defining the goodness of Learning algorithm

![](./images/1581154676461.png)
```
f1、f2 表示 task1 学习出来的结果，task1、task2 的 Test data 放入 f1 中得到结果 l1、l2。
假设有 N 个 task，每个 task 都能算出一个 loss 值，loss 值全部加起来得到L(F)，用 L(F) 来评估 Learning algorithm 好坏。
```
>**Meta Learning 和 Machine Learning 的区别**

![](./images/1582449809311.png)
```
Machine Learning 需要准备 train image 和 test image。
Meta Learning 需要准备 train task 和 test task，每一个 task 中都有 train image 和 test image。
在 Meta Learning 中训练资料叫做 train task，测试资料叫做 test task。
Support set		Query set
```

### pick the best Learning algorithm

![](./images/1581157187732.png)
```
最好的 learning algorithm 就是待入这个 Loss Function，算出来最小的 leanring algorithm。
```
>**Omniglot**

![](./images/1582449869049.png)
![](./images/1581158772397.png)
```
20 ways 1 shot：20 个 classes，每一个 classes 有 1 个 examples。
从 train data 中 sample N 个 characters，K 个 examples。
从 test data 中 sample N 个 characters，K 个 examples。
```

>**Techniques Today - MAML**

![](./images/1581162480999.png)
![](./images/1581162847502.png)
```
MAML 要做的事情是学一个最好的 initial parameter
定义一个 Loss Function，Loss Function 的 input 就是初始化的参数，如何知道初始化的参数好或者不好，将初始化的参数放在各个不同的 task 的上面做训练，不同的 task 上面学出来的 model 是不一样的。
θ^n 表示是在第 n 个 task 学出来的 model。
ln(θ^n)：表示 θ^n 这一组参数最终拿去 task n 的 test set 的 loss 值。
L(Φ)：表示 N 个 loss 值总和的 Loss Function。
```

![](./images/1581164020070.png)

>**MAML vs Model Pre-training**

![](./images/1581164163474.png)
```
虽然 Φ 本身去做 task1 和 task2 并不是很强，但是把 Φ 拿去训练以后，用 task2 的 data 训练之后可以变得很强，Φ 就是一个好的 Φ。
```
![](./images/1581164444819.png)
```
寻找一个最好的 Φ。
```
![](./images/1581164568568.png)
```
MAML: 找到一个 Φ 在任务上经过可以得到好的结果。
Model Pre-training: 找到一个 Φ 可以在训练任务上得到好的结果。
```
![](./images/1581165365686.png)
```
假设 MAML 在 training 的时候只 update 一次，初始的 Φ 是 model 寻找出来的，计算过一次 gradient 以后，被 update 以后得到的 θ^ 就当作最终的结果。
```

>**Toy Example**

![](./images/1581165822745.png)
![](./images/1582450019875.png)
![](./images/1581166350495.png)


### Warning of Math

![](./images/1581170031318.png)
![](./images/1581170299261.png)
![](./images/1581170381813.png)

### Real Implementation

![](./images/1581170626609.png)
```
MAML：初始值 Φ 有一个初始值 Φ0，每一个 task 就是一笔训练资料，update 两次参数，第二次 update 的参数更新 Φ0，Φ0 -> Φ1 移动的方向就跟第二步 gradient 算出来的方向是一样的。以此类推
Model Pre-training：计算 task m 对 Φ0 的 gradient，根据 gradient 的方向移动。
```

>**Example - Translation**

![](./images/1581170788938.png)
```
MAML 要比 Model Pre-training 要好。
```

### Reptile

![](./images/1581171305409.png)
![](./images/1581171506680.png)
```
初始值 Φ 有一个初始值 Φ0，sample a training task m，使用 Φ0 训练 task m，Reptile 没有限制 update 参数的次数，所以可以 update 很多次参数，最后找出 θ^m，Φ0 -> Φ1 移动的方向使 Φ0 -> θ^m 指向的方向。以此类推。
```
![](./images/1581171620355.png)
![](./images/1581171767142.png)
---
title: Deep Reinforcement Learning
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1582637628269.png)
```
pass
```

### Scenario of Reinforcement Learning

![](./images/1582637656988.png)
```
Agent: 会有 Observation(又叫做 State)，会看事件
State: Environment 的状态，不是系统的状态
Action: 即 machine 做的工作，会影响环境
Reward: 告诉 Agent 是好的还是不好的

抽象举例：
machine 看到一杯水，然后 machine take a action，action 是把水打翻。Environment 就得到一个 negative reward，在 reinforcement learning 里面这些发生的事情都是连续的。接下来 machine 看到的 Observation 就是水被打翻了。 
```
![](./images/1582637685875.png)
```
接下来 machine 看到的 Observation 就是水被打翻了，然后 take a action，然后 machine 要把它马上擦干净，Environment 就得到一个 positive reward。
```

>**Learn to play Go**

![](./images/1582637718192.png)
```
一开始 machine 的 Observation 就是一个棋盘(19x19)，take a action，action 是放一个棋子到棋盘上。Environment 就是对手下棋的位置。 
```
![](./images/1582637745914.png)
```
接下来看到新的 Observation 之后，会决定一个新的 action，machine 新再放一个棋子到棋盘上。
```

>**Learn to play Go - Supervised v.s. Reinforcement**

![](./images/1582637778722.png)
```
· Supervised
Supervised Learning 可以 learn 一个会下棋的 Agent，但是不是最厉害的 Agent。
是通过老师学习。
· Reinforcement
找人下围棋，if win，就得到 positive reward。
if fail，就得到 negative reward。
是从过去的经验中学习。

Alpha Go is supervised learning + reinforcement learning
```

>**Learning a chat-bot**

![](./images/1582637806927.png)
```
Reinforcement Learning 也可以用在 chat-bot 上。
之前是 learn 一个 sequence-to-sequence model，input 是一句话，output 是机器的回答。
```
![](./images/1582637829038.png)
```
· Supervised
有 label 的 training，即有人对 chat-bot 说 "Hello"，chat-bot 就要回答 "Hi"。
· Reinforcement
让 machine 胡乱跟别人讲话，自己判断结果。
```

>**Learning a chat-bot - Reinforcement Learning**

![](./images/1582637871126.png)
```
learn 两个 Agent。
```
![](./images/1582637898608.png)
```
需要定义一些 rules to evaluate 对话的好或不好。
可以用 GAN 做 chat-bot。
```

### More applications

![](./images/1582637920212.png)
![](./images/1582637949944.png)

>**Example: Playing Video Game**

![](./images/1582637979993.png)


![](./images/1582638007864.png)


![](./images/1582638035990.png)


### Dfficult of Reinforcement Learning

![](./images/1582638063359.png)
```
· Reward delay
· Agent's actions affect the sunsequent data it receives
```

![](./images/1582638095292.png)
![](./images/1582871670373.png)


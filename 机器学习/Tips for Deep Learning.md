---
title:  Tips for Deep Learning 
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---

![](./images/1578065304384.png)
```
在 Training Data 上得到好的结果，但在 Testing Data 上没有得到好的结果，这个时候才叫做 Overfitting。
```

![](./images/1578065617983.png)
```
在得到的结果是 Overfitting 之前，先检查 Training Data 上的结果，对于 不用检查，但对于 Neural Network 是需要检查的。
```

![](./images/1578065798003.png)
```
不同的方法对应不同的症状。
```

![](./images/1578066522713.png)
```
当把 Network 叠的很深的时候，在最靠近 input layer 的地方，参数对最后 Loss Function 的微分值会是很小的，比较靠近 Output 的地方微分值会是很大的。
当设定同样的 Learning rate 的时候，靠近 input layer 的地方，参数 update 的是很慢的，靠近 Output 的地方，参数 update 的是很快的。
靠近 input layer 的地方，参数还是 random 的时候，靠近 Output 的地方，就已经根据 random 的结果找到了一个 Local minima，然后就 converge 了。
```

![](./images/1578068044269.png)

```
某一个参数 w 对 total Cross 的偏微分，意思是说，当对某一个参数 w 做一个小小的变化的时候，它对 Cross 的影响是怎么样，以此来决定说这个参数 w 它的 Gradient 的值有多大。
在第一个 layer 中的某一个参数加上 Δw，看看 Neural Network 的 Output 对 Target 的 loss 有什么样的影响。
如果加上的 Δw 很大，通过 sigmod function 的时候，这个 output 是会变小的，也就是说改变了一个参数的 weight，对某一个 neuron 的 output 的值有影响，但是影响是会衰减的，比如用的是 sigmod function，sigmod function 会把(-∞, +∞)的值硬压到(0, 1)之间。
如果有很大的 input 变化，通过 sigmod function 之后，它的 output 变化会是很小的，每通过一次 sigmod function，output 就衰减一次，所以当 network 越深，它的衰减次数就越多，直到最后它对 output 的影响是很小的，因此最后对 Cross 的影响是很小的，因此就造成靠近 input 的那些 weight，它的 gradient 的值是小的。
```
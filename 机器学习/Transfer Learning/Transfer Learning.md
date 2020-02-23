---
title: Transfer Learning
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---


![](./images/1582456388425.png)
```
Transfer Learning 意思是假设现在又一些跟现在进行的 task 没有直接相关的 data，能不能直接用这些没有相关的 data 来帮助我们做一些事情。
比如：input 的 distribution 是相似的，都是动物的图片，但 task 的 label 是无关的，Similar domian，different tasks
还有一种就是 Different domain，same tasks。
Transfer Learning 做的是能不能在有一些不相干的 data 的情况下，来帮助现在要做的 task。
```
![](./images/1582456456294.png)
```
用其他不相干的 data 帮助现在做的 task。
```
![](./images/1581751250907.png)
```
通过一些关系的映射
```
![](./images/1581751869176.png)
```
现在有一个 task，和 task 有关的 data，叫做 Target Data，和 task 无关的 data，叫做 Source Data
Target Data 和 Source Data 都是有可能有 labelled 和 unlabelled，所以就会有四种可能。
```

### 第一象限

```
Target Data 是 labelled
Source Data 是 labelled
```

#### Model Fine-tuning

![](./images/1581751839154.png)

>**Task description**

![](./images/1581755104766.png)
```
在 task 里面，Target Data 和 Source Data 都是有 labelled 的，假设说 Traget Data 的量是非常少的，Source Data 的量是非常多的。
如果 Target Data 量非常少，少到只有几个 example，这个叫做 One-shot Learning。
可以做的 example：speaker adaption
处理方式：用 Source Data 直接 train 一个 model，接下来用 Target Data fine-tune 这个 model。把 Source Data train 出来的 model 当做事 training 的 inital value，再用 target data train 下去。
Challenge:only limited target data,so be careful about overfitting 
```
>**Conservative Training**

![](./images/1581755604420.png)
```
技巧 1
现在有大量的 Source Data(如果在语音辨识里面，就是很多不同 speaker 的声音)，train 一个语音辨识的 Neural Network，接下来是 Target Data(某个 speaker 的声音)，不能直接用 Target Data train 这个 model。
在 training 的时候，train 完的 model 和旧的 model 不要差太多。希望新的 model 的 output 和旧的 model 的 output 看到同一笔 data 的时候，它们的 output 越接近越好。
```

>**Layer Transfer**

![](./images/1581756955722.png)
![](./images/1581755807328.png)
```
技巧 2
使用 Source Data train 好了一个 model，把 model 中的某几个 layer 拿出来，直接 copy 到新的 model 里面去，接下来用 Target Data 只去 train 没有 copy 的 layer，这样的好处就是 Target Data 只需要考虑非常少的参数，就可以避免 Overfitting 的问题。
```
![](./images/1581755951479.png)
```
哪些 layer 应该被 transfer，哪些 layer 不应该被 transfer？
语音辨识：copy 最后几层，重新 train input layer。
Image：copy 前面几层，只 train 最后几层。
```
>**Layer Transfer - Image**

![](./images/1582456697890.png)
![](./images/1581756037857.png)

#### Multitask Learning

![](./images/1581757573469.png)
```
Multitask Learning 和 Fine-tuning 略有不同的地方？
在 Fine-tuning 里面，我们 care 的是 Target domain 做的好不好。
在 Multitask Learning 里面，我们同时 care Source domain 和 Target domain 做的好不好。
```
![](./images/1581769254815.png)
```
假设有两个不同的 task，用的同样的 feature，比如说影像辨识，只是影像辨识的 class 不一样。learn 一个 Neural Network，input 就是两个不同 task 的 feature，中间会分叉一部分 network，一部分 network 的 output 是 task A 的 答案， 一部分 network 的 output 是 task B 的 答案， 这样做的好处是 task A 和 task B 在前面几个 layer 就会使共用的，前面几个 layer 会同时使用 task A 和 task B 的 data，前面几个 layer 使用比较多的 data train 的，所以可能有比较好的 performence。
当 input 没有办法 share 的时候，两种 task 不同的 input 都用不同的 Neural Network，把它 transform 同一个 domain 上，然后再分叉。
```
>**Multitask Learning - Multilingual Speech Recognition**

![](./images/1581769356325.png)
```
前面 share layer，后面分开。
```
![](./images/1581769480821.png)

>**Progressive Neural Networks**

![](./images/1581769516619.png)
```
Transfer Learning 有些负面效应。
```

### 第二象限

```
Target Data 是 unlabelled
Source Data 是 labelled
```

![](./images/1581769560298.png)

>**Task description**

![](./images/1581771103758.png)
```
Source Data 有 function 的 input 和 output，是 MNIST 的 image，是有 label 的，视作 Training data。
Traget Data 只有 function 的 input，是 MNIST-M 的 image，是没有 label 的，实作 Testing data。

Training data 和 Testing data 是非常的 mismatch 的。
怎么把在 Source data learn 出来的 model 放在 Target data 上？
```
#### Domain-adversarial training

![](./images/1582456925373.png)
```
直接 learn 一个 model，train 下去之后结果是会烂掉的。
如果把一个 Neural Network 当成一个 feature extractor，让 Neural Nework 的前面几层可以看作是在抽取 feature，后面几层可以看作是在做 classification。
如果前面几层是在抽取 feature，不同 domain 的 data，它的 feature 完全不一样。
```
![](./images/1581772856485.png)
```
希望前面的 feature extractor 可以把 domain 的特性去除掉，也就是 feature extractor 的 output 不应该是红点和蓝点分成两区域，不同 domain 不应该分成两区域，而是不同 domain 混在一起。
怎么 learn feature extractor 呢？
后面接一个 Domain classifier，把 feature extractor 的 output 丢给 Domain classifier，做的是根据 feature extractor 给它的 feature，判断这个 feature 来自于哪一个 domain。
只要 domain classifier 是不够的。
```
![](./images/1581772900204.png)
```
给 feature extractor 增加任务的难度
feature extractor output 的 feature 不止骗过 domain classifier，还有同时让 label predictor 做的好。

feature extractor：Maximize label classification accuracu, minimize domain classification accuracy
Label predictor：Maximize label classification accuracy
Domain classifier：Maximize domain classification accuracy
```
![](./images/1581772935811.png)
![](./images/1581772964552.png)

#### Zero-shot learning

![](./images/1581772992747.png)

>**Task description**

![](./images/1582457053907.png)
```
Source Data 有 function 的 input 和 output，是有 label 的，视作 Training data。
Traget Data 只有 function 的 input，是没有 label 的，实作 Testing data。

Training data 和 Testing data 是 Different tasks。
比如 Source data 的 image 是 cat、dog
在 Target data 的 image 是 草泥马，在 Source data 从来都没有出现过草泥马。
这样的 task 在语音上很早就有 solution，语音本来常常会遇到 Zero-shot Learning 的问题，Testing data 的词汇有些是在 Training data 中没有出现的。
```
![](./images/1581775162727.png)
```
在影像上可以把每一个 class 用它的 attribute 来表示，有一个 database，这个 database 里面有所有不同可能的 object 跟它的特性，假设现在做的是辨识动物，但是 training data 和 testing data 中的动物是不一样的，attribute 要定义的够丰富，每一个 class 都要有它独一无二的 attribute。
在 training 的时候，不直接辨识说每一张 image 属于哪一个 class，而是去辨识说每一张 image 里面它具备什么样的 attribute。
```
![](./images/1581775188171.png)
```
在 testing 的时候，就算来了一种从来没有见过的 image，只要能把它的 attribute 找出来，就查表说在 database 里面哪一种动物它的 attribute 和现在 model 的 output 最接近。
```
![](./images/1582457156030.png)
```
有可能 attribute 比较复杂，attribute 的 dimension 的可能很大，甚至可以做 attribute embedding。
有一个 embedding space，把每一张 image 通过 transform 变成 embedding space 上的一个点，把所有 attribute 也变成 embedding space 上的点。
f(*) and g(*) can be NN，f(*) and g(*) as close as possible。
在 testing 的时候，如果有一张没有看过的 image，这个 image 的 attribute embedding 跟哪个 attribute 最像。
就是 Grass-mud horse。

把 image 和 attribute 都可以描述成 vector，把 image 和 attribute 投影到同一个 embedding space 里面
对 image 的 vector x，attribute 的 vector y 降维，降到同样的 dimension。
怎么找 f(*) and g(*)？
f(*) and g(*) 就是 Neural Network，input image 变成一个 vector，input attribute 变成 vector。
假设已经知道 y1 是 x1 的 attribute，y2 是 x2 的 attribute，找一个一个 f(*) and g(*) 可以让 x1 和 y1 投影到 embedding space 以后越接近越好，让 x2 和 y2 投影到 embedding space 以后越接近越好。
假如有一张没有看过的 image x3 在 testing data 里面，它也可以同 f(*) 变成 embedding space 的一个 vector，接下来就看这个 embedding vector x3 和 y3 的 embedding 最接近，y3 就是它的 attribute。
```
![](./images/1581775284433.png)
```
有时候会遇到的问题: 如果没有 database，根本不知道每一个动物的 attribute 是什么，怎么办？
可以借用 word vector，word vector 的每一个 dimension 就代表了这个 word 的某种 attribute。
```
![](./images/1581775320644.png)
```
xn 通过 f(x)，yn 通过 g(y)，它的距离越接近越好，这是有问题的。
如果知道 xn 和 yn 是同一个 pair，要让它们的距离越接近越好
如果知道 xn 和另外一个 yn 不是同一个 pair，要让它们的距离越接远越好
最小值是 0，当不是 0 的那一项小于 0 的时候，loss 就是 0。
什么时候会有 zero loss 呢？
如果 f(xn) 和 g(yn) 的 innerkuaida，要大过所有其他的 f(xn) 和 g(ym) 的 innerkuaida，而且还要大过一个 margin k。
```
**>Convex Combination pf Semantic Embedding**

![](./images/1581775344837.png)
```
只需要有一个 word vector 和一个 语音辨识系统。
```
![](./images/1582457362924.png)
![](./images/1582457386347.png)

### 第三、四象限

![](./images/1581775529347.png)
```
Target data 有 label，Source data 没有 label 的情况叫做 Self-taught learning，可以算是 Semi-supervised learning。
Target data 没有 label，Source data 没有 label 的情况叫做 Self-taught Clustering

Self-taught learning 和 Semi-supervised learning 有一些不一样的地方，
Semi-supervised learning 的 labelled data 和 unlabelled data 是比较有关系的。
在 Self-taught learning 里面，labelled data 和 unlabelled data 是比较远的。
```
![](./images/1582457296621.png)
```
假如 Source data 够多，虽然是 unlabelled，可以去 learn 一个 feature extractor，总之有大量 data，它们没有 label，可以做的是用这些 data learn 好的 feature extractor，用这些 data learn 一个好的 representation。
用这个 feature extractor 在 target data 抽取 feature。
```
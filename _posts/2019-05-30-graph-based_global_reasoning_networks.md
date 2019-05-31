---
layout: post
title: GB-GRN(基于图形的全局推理网络)
date: 2019-05-30 
tags: 深度学习/闪马  
---

## 背景

一直在用resent18做外卖分类，最近看到了这篇论文，看到效果不错，就想把它放到自己的训练模型中看看效果增益。
	`书山有路勤为径，学海无涯苦作舟-----加油`
* 论文：《Graph-Based Global Reasoning Networks》
* 论文地址：https://research.fb.com/wp-content/uploads/2019/05/Graph-Based-Global-Reasoning-Networks.pdf? [点击阅读](https://research.fb.com/wp-content/uploads/2019/05/Graph-Based-Global-Reasoning-Networks.pdf?)

### 目录
* [介绍](#introduction)
* [基于图形的全局推理](#Graph-based Global Reasoning)
* [应用方法-图像分类](#Implementation Details-Image Classification)

### <a name='introduction'></a>介绍

在图像分类、分割、动作记录等许多计算机视觉任务中，任意形状的遥远区域之间的关系推理是至关重要的。人类可以很容易地理解图像/视频不同区域之间的关系，如图1(a)所示。然而，深度CNNs如果不叠加多个卷积层，就无法捕获这种相关性，因为单个层只能在局部捕获信息。这是非常低效的，因为特征图上任意形状的遥远区域之间的关系只能被一个具有足够大的接受域来覆盖所有感兴趣区域的近顶层捕获。举个例子来说，在含有16个残差单元的resnet50，在第11个单元(接近res4单元的尾端)接收域逐渐增大至覆盖整个图片的大小224x224。为了解决这一问题，论文中提出了利用一个单元，将感兴趣区域的特征投影到交互空间中，然后将其重新分配到原坐标空间中，直接执行全局关系重构的方法。这样，在CNN模型的早期阶段就可以进行关系推理。

具体地说，就是构造一个潜在的交互空间，直接执行全局推理，而不是仅仅依赖坐标空间中的卷积来隐式地对不同区域之间的信息进行建模和通信，如图1(c)所示。在这个交互空间中，共享相似符号的一组区域由单个特性表示，而不是由输入中的一组分散的特定于坐标的特性表示。从而将多个不同区域之间的关系推理简化为交互空间中相应特征之间的关系建模，如图1(c)顶部所示。在交互空间中建立了一个连接这些特征的图，并在图上执行关系推理。经过推理，更新后的信息被投影回原来的坐标空间用于下行任务。文中提出了一种叫全局推理单元(GloRe)，它通过加权全局池和加权扩散有效地实现了坐标交互空间映射过程，然后通过图卷积实现了关系推理，并且这一过程是可微的，能做到端到端的训练。不同于最近提出的非局部神经网络(NL-Nets)和双注意网络(Double Attention Networks),GloRe模型能够直接推理区域之间的关系，而双注意网络只专注于传递信息，推理依赖于卷积层。同理，不像我们提出的方法那样是为区域推理而设计的，se-net只关注通过全局平均池合并图像级特性，会导致交互图只包含一个节点。

<div align="center">
	<img src="/images/posts/GB-GRN/figure1.png" height="450" width="600">  (图1)
</div> 

### <a name='Graph-based Global Reasoning'></a>基于图形的全局推理

GloRe单元的目的是克服卷积运算对全局关系建模的固有限制。它由五个卷积组成，其中两个在输入特征X和输出特征Y上是用来升微和降维(最左边和最右边)，最上面的一个用于生成坐标与潜在交互空间(上图)之间的双投影B，中间两个适用于基于交互空间中的Ag图做全局推理的，其中V将区域特征编码为图节点，Wg表示图卷积的参数。

<div align="center">
	<img src="/images/posts/GB-GRN/figure2.png" height="350" width="600">  (图2)
</div> 

将特征从坐标空间投影到交互空间后，得到每个节点都具有特征描述符的图。原来通过捕获输入中任意区域之间的关系现在简化为捕获相应节点的特性之间的交互。

有几种可能的方法可以捕获新空间中特性之间的关系。最直接的方法是将这些特性连接起来作为输入，并使用一个小型的神经网络来捕获相互依赖关系。然而，即使是一个简单的关系网络在计算上也是很高的，还破坏了沿特征维的成对通信。通过图卷积进行关系推理如图3(a),图3(b)利用双向一维卷积实现图形卷积。

<div align="center">
	<img src="/images/posts/GB-GRN/figure3.png" height="450" width="600">  (图3)
</div> 


### <a name='Implementation Details-Image Classification'></a>应用方法-图像分类

将glore作为插入模块插入到resnet18点res4末端，实现我自己项目分类。

* 项目地址：https://github.com/DavidOcea/poseidon[点击跳转](https://github.com/DavidOcea/poseidon)
* 组合代码地址：https://github.com/DavidOcea/myNetModel/blob/master/model/glore_resnet18.py[点击跳转](https://github.com/DavidOcea/myNetModel/blob/master/model/glore_resnet18.py)
* 单元代码地址：https://github.com/DavidOcea/glore[点击跳转](https://github.com/DavidOcea/glore)

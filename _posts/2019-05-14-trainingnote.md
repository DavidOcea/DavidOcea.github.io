---
layout: post
title: 训练日记
date: 2019-05-14
tags: 闪马
---

## 介绍
* 5月14日 多云
* 数据集： 训练集：waimaitrainv0.3.json；验证集：waimaivalv0.1.json
* 训练外卖分类模型，（resnet18_112），从昨天的训练来看，训练过程仍然有过拟合的情况发生，所以加入mixup训练预训练模型看看情况会不会好点。初始学习率仍然从0.01开始，变化点为20，30，40，共训练50个epoch，数据集不变。训练结果发现，采用imagenet数据集的预训练模型+mixup的训练方式，的确解决了过拟合问题，但是得到的模型只有88%左右的acc，比不用mixup效果差一些，且从acc图中能看出0.01学习率的acc直线平滑上升，而0.001和0.0001学习率变化是acc没有明显变化。猜测肯能训练epoch太少，所以采用70个epoch，学习率变化点位40，50，60

|date|arc|epoch|base_rl|lr_schedule|pretrained|resume|dropout|weight_decay|mixup|mixup/alpha|bast/acc/eval|acc/map|
|--|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|5-14|resnet18_112|50|0.01|20,30,40|yes-imagenet|no|0.5|0.0001|yes|0.5|87.8%|<img src="/images/posts/trainnote/mixup+pre(50+0.01)5-14(1).png" height="80" width="100">|
|5-15|resnet18_112|70|0.01|40,50,60|yes-imagenet|no|0.5|0.0001|yes|0.5|87.9%|<img src="/images/posts/trainnote/mixup+pre(70+0.01)5-15(2).png" height="80" width="100">|


* 5月15日 阵雨
从昨天结果来看，加入mixup之后确实解决了acc最后下降到问题，而且，模型的acc始终在70个epoch内始终在缓慢上升最后达到87.9%。但是，训练的best-acc仍然没有达到比较理想的结果，而且加入imagenet预训练模型后的best-acc没有不用预训练模型来的好。

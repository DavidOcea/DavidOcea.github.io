---
layout: post
title: 训练日记
date: 2019-05-14
tags: 闪马
---

## 介绍
* 数据集：

| 版本 | 数据集 |
|-----|:-------:|
| v0.1 | 低点位数据：训练集(212440条)：http://pqemz4kka.bkt.clouddn.com/xuhuiwaimai/waimaitrainv0.3.json；验证集(6676条)：http://pqemz4kka.bkt.clouddn.com/xuhuiwaimai/waimaivalv0.1.json |

* 训练记录：

|date|arc|epoch|base_rl|lr_schedule|pretrained|resume|dropout|weight_decay|mixup|mixup/alpha|bast/acc/eval|acc/map|
|--|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|5-14|resnet18_112|50|0.01|20,30,40|yes-imagenet|no|0.5|0.0001|yes|0.5|87.8%|<img src="/images/posts/trainnote/mixup+pre(50+0.01)5-14(1).png" height="80" width="100">|
|5-15|resnet18_112|70|0.01|40,50,60|yes-imagenet|no|0.5|0.0001|yes|0.5|87.9%|<img src="/images/posts/trainnote/mixup+pre(70+0.01)5-15(2).png" height="80" width="100">|
|5-16|resnet18_112|50|0.01|20,30,40|no|no|0.5|0.001|no|/|89.5%|<img src="/images/posts/trainnote/nopre(50-0.01)5-16(1).png" height="80" width="100">|
|5-17|resnet18_112|60|0.01|30,40,50|no|no|0.5|0.001|no|/|89.5%|<img src="/images/posts/trainnote/nopre(60+0.01)5-17(1).png" height="80" width="100">|
|5-17|resnet18_112|80|0.01|50,60,70|no|no|0.5|0.001|no|/|90.1%||
|5-20|resnet18_112|80|0.01|50,60,70|no|no|0.5|0.002|no|/|89.3%||
|5-21|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|no|/|/||
|5-28|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.5|90.81%||
|5-28|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.2|91.15%||
|5-28|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.8|91.03%||
|5-29|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.2|90.00%||
|5-29|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.8|91.23%||
|5-29|resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|1.2|91.06%||
|5-30|glore_resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|no|/|90.30%||
|5-30|mixup_glore_resnet18_112|50|0.1|20,30,40|no|no|0.5|0.002|yes|0.2|90.96%||



|date|arc|epoch|base_rl|lr_schedule|pretrained|resume|feature_net|weight_decay|num_attentions|bast/acc/eval|remork|
|--|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|5-16|wsdan|50|0.001|20,30,40|no|no|resnet18_112|0.0005|32|83.7%||
|5-17|wsdan|50|0.01|20,30,40|no|no|resnet18_112|0.0005|32|86.7%||
|5-20|wsdan|50|0.001|20,30,40|no|no|resnet18_112|0.0005|32|87.2%|RandomHorizontalFlip+ColorJitter|
|5-21|wsdan|50|0.001|20,30,40|no|no|resnet18_112|0.0005|32|/|RandomResizedCrop+RandomHorizontalFlip+ColorJitter|



`5月14日 多云 外卖分类`
* 训练外卖分类模型，（resnet18_112），从昨天的训练来看，训练过程仍然有过拟合的情况发生，所以加入mixup训练预训练模型看看情况会不会好点。初始学习率仍然从0.01开始，变化点为20，30，40，共训练50个epoch，数据集不变。训练结果发现，采用imagenet数据集的预训练模型+mixup的训练方式，的确解决了过拟合问题，但是得到的模型只有88%左右的acc，比不用mixup效果差一些，且从acc图中能看出0.01学习率的acc直线平滑上升，而0.001和0.0001学习率变化是acc没有明显变化。猜测肯能训练epoch太少，所以采用70个epoch，学习率变化点位40，50，60。

`5月15日 阵雨 外卖分类`
* 从昨天结果来看，加入mixup之后确实解决了acc最后下降到问题，而且，模型的acc始终在70个epoch内始终在缓慢上升最后达到87.9%。但是，训练的best-acc仍然没有达到比较理想的结果，而且加入imagenet预训练模型后的best-acc没有不用预训练模型来的好。

`5月16日 阴 外卖分类`
* 外卖分类模型，由于采用预训练模型表现不佳，暂时不采用预训练模型。基于之前训练模型的结果来看，在不采用预训练模型的情况下，调整适当的参数，可将resnet18_112网络训练得到平均acc到91%左右的分类模型，最高acc出现在学习率为0.01时，所以本次训练以base_lr=0.01,epcho=50,lr_schedule=[20,30,40],dropout=0.5,weight_decay=0.001训练，其他参数不变。得到结果最高acc=89.5%，出现在第20个epoch。
* 采用ws-dan models结构都resnet18_112训练外卖分类，因为是全新的数据增广方法机训练模式，初始参数设定如下：base_lr=0.001,epcho=50,lr_schedule=[20,30,40],num_attentions=32,weight_decay=0.0005。

`5月17日 阵雨 外卖分类`
* 不采用预训练的resnet18_112最佳表现在20个epoch左右，为89.3%，依然不理想。从acc图上能看出依然存在过拟合现象，acc在lr为0.01的阶段上升比较明显，之后趋于平缓，再到后来出现下滑现象。所以判断在lr=0.01时，模型能学习到较多信息，因此拉长lr为0.01到时间段，再做一次训练测试。更新参数如下：lr_schedule=[30,40,50],epoch=60,其他不变。从结果上看最高acc出现在第30个epoch（即lr=0.01)acc=89.5%，之后随着lr变化逐渐趋于平缓，所以增加lr=0.01的训练圈数，更新参数：lr_schedule=[50,60,70],epoch=80。
* ws-dan的模式训练resnet18_112,从结果上看，最高acc为83.7%，首次测试结果并不理想，从acc-epoch图上看，前20个epoch结果非常不稳定，忽高忽低，20个epoch之后结果稳定于79%左右。鉴于第一次测试之前没有细微的调过初始化参数，准备采用5个epoch做测试训练，先确定处较好的初始化参数再训练模型。试验后定base_lr=0.01,其他参数不变，进行训练。

`5月20日 周一 阴 外卖分类`
* 从上次训练结果来看，最优结果出现在第50个epoch，lr=0.001，best_acc=90.1%，之后，acc随着lr的变化并没有明显提高，小波动稳定，最后acc回到88.2%,还是有过拟合的现场。
调整参数weight_decay(0.001>0.002)，其他不变。
* 从上次结果来看，最优结果出现在第17个epoch，lr=0.01，best_acc=86.7%，只有有下降的趋势。
之前训练输入的图像信息不做任何其他数据增强方案，现在尝试输入的图像加入随机水平翻转(RandomHorizontalFlip)和颜色抖动(ColorJitter)结合wsden的数据增强方式，base_lr=0.01，其他参数同上。

5月21日 周二 晴 外卖分类
* 上一次训练结果如下，bast_acc=89.3%,出现在第50个epoch,仍然有一定过拟合，但是不明显。综合前面几次试验分析，虽然加大epoch至80后bast_acc确实提高了一点，但整体优化不大。
所以重新调整epoch为50，schedule_lr=[20,30,40],base_lr=0.1,其他参数同上一次训练。
* 从上一次训练结果看，best_acc=87.23%,出现在第26个epoch。整体acc变化曲线比较平缓上升后平稳。
测试一下，RandomResizedCrop+RandomHorizontalFlip+ColorJitter+wsdan模式的效果，其他参数同上。
* 因为最近卡在升级，具体测试安排在24号

`5月28日 周二 阴`
* 1.训练外卖分类低点位模型mixup+res18_112,因为之前mixup有些问题，所以重新训一下，看看情况。参数选择：epochs: 50，base_lr: 0.1，lr_schedule: [20, 30, 40]，weight_decay: 0.002，dropout: 0.5，alpha: 0.5。训练结果：best_acc=90.81%,出现在第39个epoch。
* 调整参数alpha=0.2,其他参数不变，继续训练。训练结果：best_acc=91.15, 出现在第30个epoch。
* 调整菜蔬alpha=0.8，其他参数不变继续训练。训练结果：best_acc=91.09%,出现在第43个epoch

`5月29日 周三 晴`
* 1.训练外卖分类，昨天美团/饿了吗标签序号没有改过来，今天以相同的参数再训一边。
*	调整参数alpha=0.2,其他参数不变继续训练。训练结果：best_acc=91.00%,出现在第30个epoch。
*	调整参数alpha=0.8，其他参数不变继续训练。训练结果：best_acc=91.23%,出现在第40个epoch。
*	调整参数alpha=1.2,其他参数不变继续训练。训练结果：best_acc=91.06%,出现在第47个epoch。

`5月30日 周四 阴`
* 1.外卖分类训练，测试glore_resnet18_112，的效果
*	参数设置： base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果:best_acc=90.30%,发生在第30epoch。
鉴于acc在第30个epoch后有回落的趋势，使用mixup继续尝试。
*	参数设置：alpha=0.2,其他参数不变。训练结果：best_acc=90.96,在第30个epoch。



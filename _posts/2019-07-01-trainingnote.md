---
layout: post
title: 训练日记
date: 2019-07-1 14:50:06
tags: 闪马
---
## 介绍
* 数据集：
* （1）外卖：

| 数据集版本 | 数据集信息 |
|-----|:-------:|
| waimaiV0.1 | 低点位数据：标签（'ele': 1, 'meituan': 0, 'other': 2, 'person': 3） 训练集(212440条)：http://pqemz4kka.bkt.clouddn.com/xuhuiwaimai/waimaitrainv0.3.json；测试集(6676条)：http://pqemz4kka.bkt.clouddn.com/xuhuiwaimai/waimaivalv0.1.json |
| waimaiV0.2 | 低点位数据：标签（'ele': 1, 'meituan': 0, 'other': 2, 'person': 3） 训练集(213479条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/waimaitrainv0.4_213479.json；测试集(6676条)：http://pqemz4kka.bkt.clouddn.com/xuhuiwaimai/waimaivalv0.1.json |
| waimaiV0.2.2 | 加入每日徐汇现场返还数据，{waimaitrainv0.4_origin_89267+waimai0604-true-1560130072002+waimei0603_1-true-1560130054572} <br> 更新训练集(218533条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/waimai0610_train_218533.json  (lmdb_waimaitrain0610) <br> 测试集不变。 |
| waimaiV0.2.3 | 标签（'meituan': 0,'ele': 1, 'other': 2, 'person': 3）<br>重新整理打标了waimaiV0.2.2中的训练集。<br>训练集（229896）：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/waimai0625_train229896.json  (lmdb_waimaitrain0625)<br>测试集（6676）：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/waimaivalv0.3_update-0.2.json   （lmdb_waimaival0.2） |
| suswaimaiv01 | 标签 {'ele': 1, 'meituan': 0, 'other': 2, 'person': 3, 'suspect-ele':4, 'suspect-meituan':5} <br> 训练集（14256）：waimai0703_train.json（lmdb_waimaisustr0.1) <br> 测试集（306）：waimaitr0703_val.json（lmdb_waimaisusval0.1） |


* （2）大小车：

| vehicleV0.1_test | 低点位数据：标签（'bus': 0, 'car': 1, 'muck-truck': 2, 'pick-up': 3, 'truck':4, 'light-bus':5）<br>  训练集(229394条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_train0.1_229394.json；<br>  验证集(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_val0.1_1200.json |
|  vehicleV0.1 <br>vehicleV0.1_update |  大小车：标签 {'car': 0, 'pick-up': 1, 'light-bus': 2, 'bus': 3, 'truck':4, 'muck-truck':5} <br> 训练集(231541条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_train0.2_231541.json <br> 验证集(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_val0.2_1200.json <br>测试集重新整理更新(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/0610vehicle_val_update0606_1200.json. (lmdb_vechicle_val0.22)  |

* 训练
* （1）外卖分类：

| 训练日期 | 训练数据集 | 基础模型 | 默认参数 | 调整参数 | 训练结果 |
|-----|:-------:|:-------:|:-------:|:-------:|:-------:|
|  07-01   |   waimaiV0.2.3   |  resnet18_112   |  epochs: 50<br>train_batch: 256<br>test_batch: 200<br>base_lr: 0.01<br>lr_scheduler: 'step'<br>lr_schedule: [20, 30, 40]<br>gamma: 0.1<br>momentum: 0.9<br>weight_decay: 0.002<br>fix_bn: False<br>num_classes: 4<br>base_size: [128,128]<br>crop_size: 112<br>dropout: 0.5   |  alpha：0.8<br>lr_scheduler: 'cosine'   |  best_acc:93.92 <br> {‘meituan’: 92.30, 'ele': 93.88, 'other': 94.77, 'person': 91.36}  |
|  07-01   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8<br>lr_scheduler: 'cosine'   |  best_acc:93.94 <br> {‘meituan’: 92.30, 'ele': 93.35, 'other': 92.71, 'person': 93.62}  |
|  07-02   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8<br>loss_type: ‘label_smoothing'   |  best_acc:93.66 <br> {‘meituan’: 94.37, 'ele': 93.61, 'other': 92.57, 'person': 93.77}  |
|  07-02   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8<br>lr_scheduler: 'cosine'<br>loss_type: ‘label_smoothing'   |  best_acc:93.49 <br> {‘meituan’: 94.08, 'ele': 94.41, 'other': 94.49, 'person': 91.336}  |
|  07-03   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8<br>base_lr:0.001<br>loss_type: ‘label_smoothing'   |  best_acc:93.69 <br> {‘meituan’: 90.82, 'ele': 92.02, 'other': 92.81, 'person': 94.02}  |
|  07-15   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |  alpha：0.8<br>base_lr:0.001<br>loss_type: ‘label_smoothing'<br>aug_hue:0.5<br>arch: ‘mobilenet2’<br>pertained:mobilenet_v2_1.0_224   |  best_acc:91.67 <br> {‘meituan’: 90.82, 'ele': 91.22, 'other': 88.42, 'person': 93.71}  |
|  07-15   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |   alpha：0.8<br>base_lr:0.001<br>loss_type: ‘label_smoothing'<br>aug_hue:0.5<br>arch: ‘mobilenet2’   |  best_acc:92.17 <br> {‘meituan’: 91.71, 'ele': 90.69, 'other': 91.45, 'person': 91.97}  |
|  07-16   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |   alpha：0.8<br>base_lr:0.001<br>weight_decay:4e-5<br>aug_hue:0.1<br>arch: ‘mobilenet2’<br>pertained:mobilenet_v2_1.0_224    |  best_acc:92.52 <br> {‘meituan’: 92.60, 'ele': 94.14, 'other': 90.28, 'person': 92.89}  |
|  07-16   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |  base_lr:0.001<br>weight_decay:4e-5<br>aug_hue:0.1<br>arch: ‘mobilenet2’<br>pertained:mobilenet_v2_1.0_224    |  best_acc:92.70 <br> {‘meituan’: 91.12, 'ele': 92.28, 'other': 92.11, 'person': 90.97}  |
|  07-16   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |  alpha：0.8<br>base_lr:0.001<br>loss_type: ‘label_smoothing'   |  best_acc:93.69 <br> {‘meituan’: 90.82, 'ele': 92.02, 'other': 92.81, 'person': 94.02}  |
|  07-16   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |  alpha：0.8<br>base_lr:0.001<br>weight_decay:4e-5<br>aug_hue:0.1<br>arch: ‘mobilenet2’<br>pertained:mobilenet_v2_1.0_224    |  best_acc:93.77 <br> {‘meituan’: 93.78, 'ele': 92.81, 'other': 93.97, 'person': 92.40}  |
|  07-17   |   waimaiV0.2.3   |  mobilenet_v2   |  /   |  alpha：0.8<br>base_lr:0.001<br>weight_decay:4e-5<br>aug_hue:0.1<br>arch: ‘mobilenet2’<br>pertained:mobilenet_v2_1.0_224    |  best_acc:93.48 <br> {‘meituan’: 89.64, 'ele': 91.75, 'other': 93.18, 'person': 93.22}  |
|  07-03   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8<br>base_lr:0.001<br>loss_type: ‘label_smoothing'   |  best_acc:93.69 <br> {‘meituan’: 90.82, 'ele': 92.02, 'other': 92.81, 'person': 94.02}  |




* （2）大车小车：

| 训练日期 | 训练数据集 | 基础模型 | 默认参数 | 调整参数 | 训练结果 |
|-----|:-------:|:-------:|:-------:|:-------:|:-------:|
|  06-11   |   vehicleV0.1_update   |  resnet18_112   |  /   |  /   |  best_acc：90.15% <br>  {'car': 93.46, 'pick-up': 88.00, 'light-bus': 79.39, 'bus': 96.50, 'truck': 87.50, 'muck-truck': 100.00}   |

* 训练日记：
`7月1日 周一 晴`
* 1.外卖分类训练
* 采用cosine学习率变化曲线，增加mixup扰动测试，设置参数：alpha=0.8。训练结果：best_acc:93.92 ,第42个epoch，{‘meituan’: 92.30, 'ele': 93.88, 'other': 94.77, 'person': 91.36}
 * 采用cosine，在Macc上提高了0.4个点，用本次训练的模型作为预训练模型进行fine-turning。
* 参数设置：lr_scheduler: ‘step'，lr_schedule: [20,40]，alpha=0.8，其他默认。训练结果：best_acc:93.94 ,第10个epoch，{‘meituan’: 92.30, 'ele': 93.35, 'other': 92.71, 'person': 93.62}

`7月2日 周二 `阴
* 1.外卖分类：
* 采用Label Smoothing来优化，设置参数，loss_type: ‘label_smoothing',alpha=0.8，其他参数默认。训练结果：best_acc:93.66 ,第21个epoch，{‘meituan’: 94.37, 'ele': 93.61, 'other': 92.57, 'person': 93.77}
* 从结果分析，采用label_smoothing对美团的表现确实提高了一些，漏检减少了些，但同时误检增多了。
* 继续采用cosine学习率变化曲线训练，设置参数：lr_scheduler: ‘cosine'，loss_type: ‘label_smoothing’,alpha=0.8，其他参数默认。训练结果：best_acc:93.63 ,第49个epoch，{‘meituan’: 94.08, 'ele': 94.41, 'other': 94.49, 'person': 91.336}
* 结果分析，对美团和饿了么的召回率提高了，但是误检也多了。

`7月3日 周三 阴`
* 1.外卖分类：
* 用昨天训练的两个模型进行fine-turning，第一个模型：0702_waimai_m0.8_res18_112_21e_93.66.pth.tar，设置参数：base_lr:0.001,alpha:0.8,lr_scheduler: ‘cosine’,loss_type: ‘label_smoothing’,其他参数默认。训练结果：best_acc:93.69 ,第12个epoch，{‘meituan’: 90.82, 'ele': 92.02, 'other': 92.81, 'person': 94.02}
* 看得出来，效果并不理想，对美团饿了么物件虽然减少了，但是漏检增加了，整体表现不如原来的好。

`7月15日 周一 晴`
* 1.外卖分类训练:验证mobilenet2精度后，以mobilenet2训练，设置参数：arch: ‘mobilenet2’,pretrained :’mobilenet_v2_1.0_224’,aug_hue: 0.5，loss_type: ‘label_smoothing’,base_lr: 0.001,alpha: 0.8,其他默认。
* 训练结果：best_acc=91.67,在第40个epoch，
* {‘meituan’: 90.82, 'ele': 91.22, 'other': 88.42, ‘person’:93.71}
* 不采用与训练模型。设置参数：arch: ‘mobilenet2’,aug_hue: 0.5，loss_type: ‘label_smoothing’,base_lr: 0.01,alpha: 0.8,其他默认。
* 训练结果：best_acc=92.17,在第34个epoch，
* {‘meituan’: 91.71, 'ele': 90.69, 'other': 91.45, ‘person’:91.97}
* 2.去重后的疑似数据集测试训练。
* 设置参数：aug_hue: 0.5，loss_type: ‘label_smoothing’,base_lr: 0.01,alpha: 0.8,训练结果：bese_acc:89.39,在第43个epoch。
* {'meituan': 85.85, 'ele': 83.00, 'other': 92.70, 'person': 90.53, 'suspect-ele': 38.65, 'suspect-meituan': 34.04}
* 在疑似训练集训练的模型中，在验证集的误检会比较小，但是漏检的数量也比较多。



`7月16日 周二 多云`
* 1.外卖分类训练：仔细对比demo中的训练参数设定，设定本次训练参数：base_lr: 0.001，pretrained: ‘mobilenet_v2_1.0_224.pth.tar'，weight_decay: 4e-5，alpha: 0.8,其他参数默认。
* 训练结果：best_acc=92.52,在第29个epoch，
* {‘meituan’: 92.60, 'ele': 94.14, 'other': 90.28, ‘person’:92.89}
* 设置参数：base_lr: 0.001，pretrained: ‘mobilenet_v2_1.0_224.pth.tar'，weight_decay: 4e-5
* 训练结果：best_acc=92.70,在第17个epoch，
* {‘meituan’: 91.12, 'ele': 92.28, 'other': 92.11, ‘person’:90.97}
* 参数：base_lr: 0.01，pretrained: ‘mobilenet_v2_1.0_224.pth.tar'，weight_decay: 4e-5，alpha: 0.8
* 训练结果：best_acc=93.77,在第21个epoch，
* {‘meituan’: 93.78, 'ele': 92.81, 'other': 93.97, ‘person’:92.40}
* 评估：这个模型在验证集中的效果一般，虽然多见识别出了几张饿了么，同时，误检的数量也增加了，多了几张新的误检。

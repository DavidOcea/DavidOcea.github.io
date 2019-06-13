---
layout: post
title: 训练日记
date: 2019-06-O3
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

* （2）大小车：

| vehicleV0.1_test | 低点位数据：标签（'bus': 0, 'car': 1, 'muck-truck': 2, 'pick-up': 3, 'truck':4, 'light-bus':5）<br>  训练集(229394条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_train0.1_229394.json；<br>  验证集(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_val0.1_1200.json |
|  vehicleV0.1 <br>vehicleV0.1_update |  大小车：标签 {'car': 0, 'pick-up': 1, 'light-bus': 2, 'bus': 3, 'truck':4, 'muck-truck':5} <br> 训练集(231541条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_train0.2_231541.json <br> 验证集(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/vehicle_val0.2_1200.json <br>测试集重新整理更新(1200条)：qiniu:///supredata-internal-video/surveillance/xuhui/crops/201906/traindata/0610vehicle_val_update0606_1200.json. (lmdb_vechicle_val0.22)  |

* 训练
* （1）外卖分类：

| 训练日期 | 训练数据集 | 基础模型 | 默认参数 | 调整参数 | 训练结果 |
|-----|:-------:|:-------:|:-------:|:-------:|:-------:|
|  06-03   |   waimaiV0.2   |  resnet18_112   |  epochs: 50  <br> train_batch: 256 <br> test_batch: 200 <br> base_lr: 0.1 <br> lr_schedule: [20, 30, 40] <br> gamma: 0.1 <br> momentum: 0.9 <br> weight_decay: 0.002 <br> fix_bn: False <br> num_classes: 6 <br> base_size: [128,128] <br> crop_size: 112 <br> dropout: 0.5    |  /   |  best_acc：90.15% <br> {‘ele': 91.446, 'meituan': 87.075, 'other': 91.315, 'person': 87.508} |
|  06-04   |   waimaiV0.2   |  resnet18_112   |  /   |  alpha=0.2 <br> (mixup)   |  best_acc：90.97% <br> {‘ele': 90.020, 'meituan': 90.476, 'other': 89.732, 'person': 90.684} |
|  06-05   |   waimaiV0.2   |  glore_resnet18_112   |  /   |  /   |  best_acc：90.94% <br> {‘ele': 89.206, 'meituan': 88.435, 'other': 90.747, 'person': 89.554} |
|  06-11   |   waimaiV0.2.1   |  resnet18_112   |  /   |  /   |  best_acc：91.02 % <br> {'meituan': 90.47, 'ele': 90.63, 'other': 91.39, 'person': 89.27}  |
|  06-11   |   waimaiV0.2.1   |  resnet18_112   |  /   |  base_lr=0.01   |  best_acc：91.02 % <br> {'meituan': 90.47, 'ele': 90.63, 'other': 91.39, 'person': 89.27}  |
|  06-12   |   waimaiV0.2.1   |  resnet18_112   |  /   |  base_lr=0.01<br>alpha=0.2   |  best_acc：90.96% <br> {'meituan': 87.75, 'ele': 91.03, 'other': 89.04, 'person': 94.01}  |
|  06-12   |   waimaiV0.2.1   |  resnet18_112   |  /   |  base_lr=0.01<br>alpha=0.8   |  best_acc：91.32% <br> {'meituan': 92.97, 'ele': 92.87, 'other': 90.13, 'person': 91.63}  |




* （2）大车小车：

| 训练日期 | 训练数据集 | 基础模型 | 默认参数 | 调整参数 | 训练结果 |
|-----|:-------:|:-------:|:-------:|:-------:|:-------:|
|  06-03   |   vehicleV0.1_test   |  resnet18_112   |  epochs: 50  <br> train_batch: 256 <br> test_batch: 200 <br> base_lr: 0.1 <br> lr_schedule: [20, 30, 40] <br> gamma: 0.1 <br> momentum: 0.9 <br> weight_decay: 0.002 <br> fix_bn: False <br> num_classes: 6 <br> base_size: [128,128] <br> crop_size: 112 <br> dropout: 0.5    |  /   |  best_acc：65.64% <br>  {'bus': 97.500, 'car': 15.500, 'muck-truck': 100.000, 'pick-up': 67.337, 'truck':87.000, ‘light-bus':35.000}   |
|  06-06   |   vehicleV0.1   |  resnet18_112   |  /   |  /   |  best_acc：91.32% <br>  {'bus': 97.00, 'car': 89.95, 'muck-truck': 100.000, 'pick-up': 84.00, 'truck':95.00, ‘light-bus’:80.95.00}   |
|  06-07   |   vehicleV0.1   |  resnet18_112   |  /   |  /   |  best_acc：90.98% <br>  {'bus': 98.00, 'car': 94.97, 'muck-truck': 100.000, 'pick-up': 85.00, 'truck':95.00, ‘light-bus’:74.87}   |
|  06-10   |   vehicleV0.1   |  resnet18_112   |  /   |  alpha=0.2<br>base_lr=0.01   |  best_acc：91.74% <br>  {'bus':96.50, 'car': 96.98, 'muck-truck': 99.50, 'pick-up': 91.00, 'truck':95.50, ‘light-bus’:75.38}   |
|  06-10   |   vehicleV0.1   |  resnet18_112   |  /   |  base_lr=0.01   |  best_acc：best_acc:91.40% <br> {'car': 94.97, 'pick-up': 86.00, 'light-bus': 78.89, 'bus': 96.50, 'truck': 92.50, 'muck-truck': 100.00}    |
|  06-11   |   vehicleV0.1_update   |  resnet18_112   |  /   |  /   |  best_acc：90.15% <br>  {'car': 93.46, 'pick-up': 88.00, 'light-bus': 79.39, 'bus': 96.50, 'truck': 87.50, 'muck-truck': 100.00}   |




* 训练日记：

`6月3日 周一 晴`
* 1.车辆分类训练。采用resnet18_112,
* 	参数设置： base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果:best_acc：65.64%   {'bus': 97.500, 'car': 15.500, 'muck-truck': 100.000, 'pick-up': 67.337, 'truck':87.000, ‘light-bus':35.000}  因为结果比预期差很多，所以检查数据集。
* 外卖分类训练：采用resnet18_112,
* 	参数设置： base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果:best_acc：90.15% ，在第30个epoch，{‘ele': 91.446, 'meituan': 87.075, 'other': 91.315, 'person': 87.508} 

`6月4日 周二 晴`
* 1.外卖分类训练：加入mixup后继续测试新数据集中的表现效果
*	参数设置：alpha=0.2，其他参数不变。训练结果：best_acc:90.97% 在第32个epoch，{‘ele': 90.020, 'meituan': 90.476, 'other': 89.732, 'person': 90.684} 

`6月5日 周三 晴`
* 1.外卖分类训练：更改基础模型，采用glore_resnet18_112测试
* 	参数设置：base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果：best_acc:90.94 在第30个epoch，{‘ele': 89.206, 'meituan': 88.435, 'other': 90.747, 'person': 89.554} 

`6月6日 周四 晴`
* 大小车分类训练：用新整理过后的数据集重新训练
* 参数设置： base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果：best_acc:91.32%,在第22个epoch，{'bus': 97.00, 'car': 89.95, 'muck-truck': 100.000, 'pick-up': 84.00, 'truck':95.00, ‘light-bus’:80.95.00}

`6月7日 周五 晴` 
* 1.大小车分类训练：用新整理过后的数据集训练（因为昨天标签顺序没有改动，调整车辆从小到大的顺序，便于以后上线分析，所以调整标签顺序以原来的参数重新训练一次）
* 参数设置： base_lr: 0.1，lr_schedule: [20, 30, 40]， weight_decay: 0.002，dropout: 0.5，其他参数默认。训练结果：best_acc:90.98%,在第38个epoch，{'bus': 98.00, 'car': 94.97, 'muck-truck': 100.000, 'pick-up': 85.00, 'truck':95.00, ‘light-bus’:74.87}

`6月10日 周一 晴`
* 1.大小车分类训练：加入mixup扰动后继续测试
* 参数设置：alpha=0.2,其他参数默认。训练结果：best_acc:91.74%,在第32个epoch，{'bus': 96.50, 'car': 96.98, 'muck-truck': 99.50, 'pick-up': 91.00, 'truck':95.50, ‘light-bus’:76.38}
* 大小车分类，改变标签编号位置重训(因为7号那天忘了运行setup.py)，默认参数。训练结果：best_acc:91.74%,在第32个epoch，{'car': 0, 'pick-up': 1, 'light-bus': 2, 'bus': 3, 'truck':4, 'muck-truck':5}

`6月11日 周二 晴`
* 1.大小车分类（使用整理过后的测试集），修改参数：base_lr=0.1,测试。训练结果：best_acc:90.15%,在第26个epoch，{‘car’: 93.46, 'pick-up': 88.00, 'light-bus': 79.39, 'bus': 96.50, 'truck': 87.50, 'muck-truck': 100.00}
* 2.外卖分类训练，更新waimaiV0.2.1后，使用默认参数测试。训练结果：best_acc:90.44 在第30个epoch，{‘meituan': 90.47, 'ele': 90.63, 'other': 91.39, 'person': 89.27} 
* 修改参数：base_lr=0.01,其他默认。训练结果：best_acc:90.54 ,第20个epoch，{‘meituan': 90.47, 'ele': 90.22, 'other': 90.82, 'person': 91.17} 

`6月12日 周三 多云`
* 1.外卖分类训练(因为11号的训练class_num=6忘了改，所以重新训一下)，
* 修改参数base_lr=0.01,alpha=0.2。训练结果：bast_acc=90.96%,21个epoch，{‘meituan': 87.75, 'ele': 91.03, 'other': 89.04, 'person': 94.01}
* 修改参数base_lr=0.01,alpha=0.2。训练结果：bast_acc=91.32%,30个epoch，{‘meituan': 92.971, 'ele': 92.87, 'other': 90.13, 'person': 91.63}

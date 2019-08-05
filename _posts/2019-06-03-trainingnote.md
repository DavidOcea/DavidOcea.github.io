---
layout: post
title: 训练日记
date: 2019-06-10 14:50:06
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
|  06-13   |   waimaiV0.2.1   |  glore_resnet18_112   |  /   |  base_lr=0.01<br>alpha=0.8   |  best_acc：91.08% <br> {'meituan': 90.47, 'ele': 91.03, 'other': 90.82, 'person': 91.57}  |
|  06-13   |   waimaiV0.2.1   |  efficientnet_b0_9   |  /   |  base_lr=0.01   |  best_acc：87.41% <br> {'meituan': 90.70, 'ele': 91.24, 'other': 84.57, 'person': 90.07}  |
|  06-14   |   waimaiV0.2.1   |  efficientnet_b0   |  /   |  base_lr=0.01<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224<br>pretrained=efficientnet_b0(官方)   |  bast_acc=77.41%<br> {'meituan': 73.01, 'ele': 82.07, 'other': 70.17, 'person': 86.46}  |
|  06-14   |   waimaiV0.2.1   |  efficientnet_b0   |  /   |  base_lr=0.001<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224<br>pretrained=efficientnet_b0(官方)   |  bast_acc=92.50%<br> {'meituan': 89.34, 'ele': 95.72, 'other': 89.20, 'person': 94.28}  |
|  06-15   |   waimaiV0.2.1   |  efficientnet_b0   |  /   |  base_lr=0.001<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224<br>pretrained:<br>14_waimai_efficientnet-b0_10e_92.50.pth.tar   |  bast_acc=92.43%<br> {'meituan': 90.93, 'ele': 94.50, 'other': 90.82, 'person': 92.18}。  |
|  06-17   |   waimaiV0.2.1   |  efficientnet_b0   |  /   |  base_lr=0.0001<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224<br>pretrained:<br>14_waimai_efficientnet-b0_10e_92.50.pth.tar   |  bast_acc=92.52%<br> {'meituan': 90.47, 'ele': 95.72, 'other': 90.46, 'person': 93.12}。  |
|  06-18   |   waimaiV0.2.2   |  resnet18_112   |  /   |  base_lr=0.001  |  bast_acc=92.55%<br> {'meituan': 92.89, 'ele': 94.41, 'other': 94.86, 'person': 90.20}  |
|  06-18   |   waimaiV0.2.2   |  resnet18_112   |  /   |  base_lr=0.001<br>alpha=0.8   |  bast_acc=93.64%<br> {'meituan': 93.78, 'ele': 94.14, 'other': 94.72, 'person': 90.84}  |
|  06-18   |   waimaiV0.2.2   |  resnet18_112   |  /   |  base_lr=0.001<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224   |  bast_acc=93.27%<br> {'meituan': 92.89, 'ele': 92.55, 'other': 93.74, 'person': 90.84}<br>[[ 314    0   19    5]<br>[   1  348   27    0]<br>[   8    6 2008  120]<br>[  16   20  220 3022]]  |
|  06-19   |   waimaiV0.2.2   |  resnet18_112   |  /   |  base_lr=0.001<br>train_batch= 128<br>base_size: [256,256]<br>crop_size: 224<br>alpha：0.8   |  bast_acc=94.18%<br> {'meituan': 94.97, 'ele': 96.80, 'other': 94.91, 'person': 91.67}<br>[[ 321    0   13    4]<br>[   0  364   11    1]<br>[   11    6 2033  92]<br>[  13   23  237 3005]]  |
|  06-24   |   waimaiV0.2.2   |  resnet18_112   |  epochs: 50<br>train_batch: 256<br>test_batch: 200<br>base_lr: 0.001<br>num_classes: 4<br>base_size: [128,128]<br>crop_size: 112<br>dropout: 0.5   |  /   |  bast_acc=91/26%<br> {'meituan': 86.68, 'ele': 90.69, 'other': 93.69, 'person': 88.83}<br>[[ 293    0   39    6]<br>[   0  341   28    7]<br>[   9    14 2007  112]<br>[  14   24  328 2912]]  |
|  06-24   |   waimaiV0.2.2   |  resnet18_112   |  /   |  alpha：0.8   |  bast_acc=93.03%<br> {'meituan': 92.60, 'ele': 91.22, 'other': 94.44, 'person': 89.93}<br>[[ 313    0   24    1]<br>[   0  343   28    5]<br>[   18    6 2023  95]<br>[  17   17  296 2948]]  |
|  06-25   |   waimaiV0.2.3   |  resnet18_112   |  epochs: 50<br>train_batch: 256<br>test_batch: 200<br>base_lr: 0.01<br>lr_schedule: [20, 30, 40]<br>gamma: 0.1<br>momentum: 0.9<br>weight_decay: 0.002<br>fix_bn: False<br>num_classes: 4<br>base_size: [128,128]<br>crop_size: 112<br>dropout: 0.5   |     |  bast_acc=93.01%<br>resize_size: [112,112]测试：<br> {'meituan': 89.94, 'ele': 90.42, 'other': 95.00, 'person': 89.78}<br>[[ 304    0   28    6]<br>[   0  340   30    6]<br>[   14    5 2035  88]<br>[  23   13  299 2943]]<br>resize_size: [128,128]测试：{‘meituan’: 92.604, 'ele': 91.489, 'other': 91.643, 'person': 94.112}  |
|  06-25   |   waimaiV0.2.3   |  resnet18_112   |  /   |  alpha：0.8   |  bast_acc=93.56%<br> {'meituan': 94.37, 'ele': 94.41, 'other': 93.69, 'person': 91.64}<br>[[ 319    0   17    2]<br>[   0  355   16    5]<br>[   13    13 2007  109]<br>[  16   16  242 3004]]<br>resize_size: [128,128]测试：<br>{‘meituan’: 94.08, 'ele': 96.54, 'other': 90.57, 'person': 95.11}  |
|  06-27   |   waimaiV0.2.3   |  shuffle_resnet18_112   |  /   |  /   |  best_acc:92.14<br> {‘meituan’: 90.23, 'ele': 93.88, 'other': 91.36, 'person': 91.91}  |
|  06-27   |   waimaiV0.2.3   |  shuffle_resnet18_112   |  /   |  alpha：0.8   |  best_acc:91.08<br> {‘meituan’: 91.71, 'ele': 94.68, 'other': 89.72, 'person': 91.24}  |
|  06-28   |   waimaiV0.2.3   |  resnet18_112   |  /   |  lr_scheduler: 'cosine'   |  best_acc:92.32<br> {‘meituan’: 83.72, 'ele': 92.02, 'other': 93.93, 'person': 91.33}  |





* （2）大车小车：

| 训练日期 | 训练数据集 | 基础模型 | 默认参数 | 调整参数 | 训练结果 |
|-----|:-------:|:-------:|:-------:|:-------:|:-------:|
|  06-03   |   vehicleV0.1_test   |  resnet18_112   |  epochs: 50  <br> train_batch: 256 <br> test_batch: 200 <br> base_lr: 0.1 <br> lr_schedule: [20, 30, 40] <br> gamma: 0.1 <br> momentum: 0.9 <br> weight_decay: 0.002 <br> fix_bn: False <br> num_classes: 6 <br> base_size: [128,128] <br> crop_size: 112 <br> dropout: 0.5    |  /   |  best_acc：65.64% <br>  {'bus': 97.500, 'car': 15.500, 'muck-truck': 100.000, 'pick-up': 67.337, 'truck':87.000, ‘light-bus':35.000}   |
|  06-06   |   vehicleV0.1   |  resnet18_112   |  /   |  /   |  best_acc：91.32% <br>  {'bus': 97.00, 'car': 89.95, 'muck-truck': 100.000, 'pick-up': 84.00, 'truck':95.00, ‘light-bus’:80.95.00}   |
|  06-07   |   vehicleV0.1   |  resnet18_112   |  /   |  /   |  best_acc：90.98% <br>  {'bus': 98.00, 'car': 94.97, 'muck-truck': 100.000, 'pick-up': 85.00, 'truck':95.00, ‘light-bus’:74.87}   |
|  06-10   |   vehicleV0.1   |  resnet18_112   |  /   |  alpha=0.2<br>base_lr=0.01   |  best_acc：91.74% <br>  {'bus':96.50, 'car': 96.98, 'muck-truck': 99.50, 'pick-up': 91.00, 'truck':95.50, ‘light-bus’:75.38}   |
|  06-10   |   vehicleV0.1   |  resnet18_112   |  /   |  base_lr=0.01   |  best_acc：best_acc:91.40% <br> {'car': 94.97, 'pick-up': 86.00, 'light-bus': 78.89, 'bus': 96.50, 'truck': 92.50, 'muck-truck': 100.00}    |
|  06-11   |   vehicleV0.1_update   |  resnet18_112   |  /   |  /   |  best_acc：90.15% <br>  {'car': 93.46, 'pick-up': 88.00, 'light-bus': 79.39, 'bus': 96.50, 'truck': 87.50, 'muck-truck': 100.00}   |
|  06-26   |   vehicleV0.1_update   |  resnet18_112   |  /   |  base_lr=0.01<br>base_size:[108,108]<br>crop_size: 96   |  best_acc:90.48，第11个epoch <br> {‘car’: 84.92, 'pick-up': 88.55, 'light-bus': 82.58, 'bus': 94.92, 'truck': 90.50, ‘muck-truck': 100.00}   |
|  06-26   |   vehicleV0.1_update   |  resnet18_112   |  /   |  base_lr=0.01<br>base_size:[108,108]<br>crop_size: 96<br>alpha:0.8    |  best_acc:92.40，第18个epoch <br>  {'car': 93.46, 'pick-up': 98.01, 'light-bus': 85.57, 'bus': 94.92, 'truck': 83.00, ‘muck-truck': 100.00}   |
|  06-26   |   vehicleV0.1_update   |  resnet18_112   |  /   |  base_lr=0.01<br>base_size:[60,60]<br>crop_size: 56    |  best_acc:90.57，第21个epoch <br>  {'car': 92.46, 'pick-up': 84.57, 'light-bus': 74.62, 'bus': 96.44, 'truck': 93.00, ‘muck-truck': 100.00}   |
|  06-26   |   vehicleV0.1_update   |  resnet18_112   |  /   |  base_lr=0.01<br>base_size:[60,60]<br>crop_size: 56<br>alpha:0.8    |  best_acc:92.07，第22个epoch <br>  {'car': 93.97, 'pick-up': 90.54, 'light-bus': 73.63, 'bus': 97.46, 'truck': 92.00, ‘muck-truck': 100.00}   |






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

`6月13日 周四 阴`
* 1.外卖分类训练:
改用glore_resnet18_112作为基础网络训练:
* 修改参数：base_lr=0.01,alpha=0.8。训练结果：bast_acc=91.08%,49个epoch，{‘meituan’: 90.47, 'ele': 91.03, 'other': 90.82, 'person': 91.57}
改用修改的efficientnet_b0_9（'efficientnet-b0': (w:1.0,d: 0.5,r: 112, dropout:0.2)，9层MBConvBlock）作为基础网络训练:
* 修改参数：base_lr=0.01。训练结果：bast_acc=87.41%,第35epoch,{‘meituan’: 90.70, 'ele': 91.24, 'other': 84.57, 'person': 90.07}

`6月14日 周五 阴`
* 1.外卖分类训练，使用efficientnet_b0➕预训练模型。
* 修改参数：train_batch: 128；base_lr: 0.01;pretrained:efficientnet_b0，base_size: [226,226]，crop_size: 224，其他参数采用模型默认。训练结果：bast_acc=77.41%,第35epoch,{'meituan': 73.01, 'ele': 82.07, 'other': 70.17, 'person': 86.46}。效果并不好，可能是fine turning的lr太高了。
* 将lr调低再试一次，base_size: [256,256]，base_lr: 0.001，其他同上，训练结果：bast_acc=92.50%,第10epoch,{‘meituan': 89.34, 'ele': 95.72, 'other': 89.20, 'person': 94.28}。

`6月15日 周六 晴`
* 1.外卖分类训练：使用efficientnet_b0➕预训练模型训练到第10个epoch，然后采用第10个epoch作为预训练模型继续训练。
* 修改参数：train_batch: 128；base_lr: 0.001;pretrained:efficientnet_b0，base_size: [256,2562]，crop_size: 224，其他参数采用模型默认。训练结果：bast_acc=92.43%,第12epoch,{'meituan': 90.93, 'ele': 94.50, 'other': 90.82, 'person': 92.18}。

`6月17日 周一 晴`
* 1.外卖分类训练，降低学习率，再次以14_waimai_efficientnet-b0_10e_92.50.pth.tar作为预训练模型进行训练。
* 修改参数：train_batch: 128；base_lr: 0.0001;pretrained:efficientnet_b0，base_size: [256,256]，crop_size: 224，其他参数采用模型默认。训练结果：bast_acc=92.52%,第12epoch,{'meituan': 90.47, 'ele': 95.72, 'other': 90.46, 'person': 93.12}

`6月18日 周二 晴`
* 1.外卖分类训练，跟新训练集waimaiV0.2.2，以resnet18_112作为基础模型，修改参数：base_lr: 0.01，其他参数采用模型默认。
* 训练结果：bast_acc=92.55%,第20epoch，
{‘meituan’: 92.89, 'ele': 94.41, 'other': 94.86, 'person': 90.20}
* 调整参数：base_lr: 0.01，alpha：0.8，其他参数不变。
* 训练结果：bast_acc=93.64%,第43epoch，
{‘meituan’: 93.78, 'ele': 94.14, 'other': 94.72, 'person': 90.84}
* 调整参数：train_batch: 128；base_lr: 0.01;base_size: [256,256]，crop_size: 224，其他参数不变。
* 训练结果：bast_acc=93.27%,第25epoch，
{‘meituan’: 92.89, 'ele': 92.55, 'other': 93.74, 'person': 92.19}

`6月19日 周三 晴`
* 1.外卖分类训练：比较mixuo+[224,224]分辨率的效果。
* 调整参数：train_batch: 128；base_lr: 0.01;base_size: [256,256]，crop_size: 224，alpha：0.8，其他参数不变。
* 训练结果：bast_acc=94.18%,第42epoch，
{‘meituan’: 94.97, 'ele': 96.80, 'other': 94.91, 'person': 91.67}

`6月24日 周一 晴`
* 外卖分类训练：
* 使用Adam作为优化函数，
* 修改参数：使用Adam默认参数。训练结果：best_acc:91.26 ,第11个epoch，{‘meituan’: 86.68, 'ele': 90.69, 'other': 93.69, 'person': 88.83}，从平均acc曲线上看，30个epoch后明显下降，有过拟合现象。
* 修改参数：alpha: 0.8；使用Adam默认参数。训练结果：best_acc:93.03 ,第28个epoch，{‘meituan’: 92.60, 'ele': 91.22, 'other': 94.44, 'person': 89.93}，从平均acc曲线上看，35个epoch后明显下降，仍然有过拟合现象。

`6月25日 周二 雨`
* 外卖分类训练：
* 更新训练集，使用默认参数训练。训练结果：best_acc:93.01,第21个epoch，{‘meituan’: 89.94, 'ele': 90.42, 'other': 95.00, 'person': 89.78}
* 上面测试的时候用resize_size: [112,112],发现原来验证集中几张误检还在，下面用resize_size: [128,128]，结果如下，{‘meituan’: 92.604, 'ele': 91.489, 'other': 91.643, 'person': 94.112}，发现原来验证集中的几张物件已经不在了。
* 更新参数：alpha：0.8，其他参数不变。40个epoch，best_acc:93.56，{‘meituan’: 94.37, 'ele': 94.41, 'other': 93.69, 'person': 91.64}
* 上面的是resize_size: [112,112]，下面用resize_size: [128,128]，结果如下：{‘meituan’: 94.08, 'ele': 96.54, 'other': 90.57, 'person': 95.11}
* 比较两个训练结果，发现，112剪裁的时候在测试集中误检少于128，但是在验证集中用128有助于减少误检。

`6月26日 周三 阴`
* 1.大小车分类训练：
* 设定参数：base_lr=0.01，base_size: [108,108]，crop_size: 96，其他参数默认。训练结果：best_acc:90.48，第11个epoch，{‘car’: 84.92, 'pick-up': 88.55, 'light-bus': 82.58, 'bus': 94.92, 'truck': 90.50, ‘muck-truck': 100.00}
* 修改参数：base_lr=0.01，alpha:0.8,base_size: [108,108]，crop_size: 96，其他参数默认。训练结果：best_acc:92.40，第18个epoch，{‘car’: 93.46, 'pick-up': 98.01, 'light-bus': 85.57, 'bus': 94.92, 'truck': 83.00, ‘muck-truck': 100.00}
* 修改参数：base_lr=0.01，base_size: [60,60]，crop_size: 56，train_batch: 512，其他参数默认。训练结果：best_acc:90.57，第21个epoch，{‘car’: 92.46, 'pick-up': 84.57, 'light-bus': 74.62, 'bus': 96.44, 'truck': 93.00, ‘muck-truck': 100.00}
* 修改参数：base_lr=0.01，base_size: [60,60]，crop_size: 56，train_batch: 512，alpha:0.8，其他参数默认。训练结果：best_acc:92.07，第22个epoch，{‘car’: 93.97, 'pick-up': 90.54, 'light-bus': 73.63, 'bus': 97.46, 'truck': 92.00, ‘muck-truck': 100.00}

`6月28日 周五 晴`
* 1.外卖分类：
* 采用cosine学习率变化曲线，其他参数默认。训练结果：best_acc:91.08 ,第35个epoch，{‘meituan’: 83.72, 'ele': 92.02, 'other': 93.93, 'person': 91.33},这个把other的效果提高了，但是美团的效果变差了。

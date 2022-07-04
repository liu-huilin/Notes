# 小样本学习（few shot learning）

## 基础概念

#### n way k shot

表示n类，每类k个样本的任务

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/notes_few_shot_learning1.png)

#### support set

少量的已标记的数据，对于n way k shot任务，就是从训练集中随机选n个类，每个类再随机抽k个样本，组成一个support set

#### qurey set

support set与query set是同类的不同样本，即从support set中的每个类的抽剩的样本中随机抽一些样本，组成query set。



## 数据增强

#### mixup

[论文](https://arxiv.org/abs/1710.09412)：mixup: Beyond Empirical Risk Minimization

假设需要对狗和猫的图像进行分类，给出一组带有标签的图像集合(即[1,0]代表狗；[0, 1] 代表猫)，Mixup的过程是对两幅图像进行简单的平均，它们的标签对应一个新数据，具体地说，Mixup可以用以下数学概念来描述
$$
\begin{array}{r}
x=\lambda x_{i}+(1-\lambda)\left(x_{j}\right) \\
y=\lambda y_{i}+(1-\lambda)\left(y_{j}\right)
\end{array}
$$
其中x，y是混合后的图像及其标签，其中，图像$x_{i}$（标签为$y_i$)和图像$x_j$(标签为$y_j$)，$\lambda$为给定beta分布（0-1）的随机数。

##### mainfold-mixup

[论文](https://proceedings.mlr.press/v97/verma19a.html)：Manifold Mixup: Better Representations by Interpolating Hidden States



## 经典算法

### [MAML](meta_learning.md)

论文：Model-agnostic meta-learning forfast adaptation of deep networks

代码：https://github.com/dragen1860/MAML-Pytorch

**关键词**：元学习





### RN（Relation Network）

论文（CVPR）：Learning to compare: Relation network for few-shot learning

代码：https://github.com/floodsung/LearningToCompare_FSL

**关键词**：度量学习；注意力机制

#### 概述

用embedding module提取特征，

然后计算support set的特征均值，然后把query set的特征和support的特征的均值cat一下，再分类

RN 就是加了个注意力，在relation module那里，它不是比较support set和 query，而是直接将query和每类support set的平均点concat，再用一个网络去进行分类



#### 参考博文

[Relation Network详解](https://zhuanlan.zhihu.com/p/443661283)



### PN

论文：Prototypical networks for few-shotlearning

代码：https://github.com/orobix/Prototypical-Networks-for-Few-shot-Learning-PyTorch

**关键词**：度量学习

#### 概述

将support set里面的每一类所有图像取了一个平均点，然后用euclidean距离 衡量query跟哪一类的平均点哪个更接近

#### 具体

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/notes_few_shot_learning_rn1.png)

### MatchingNet

论文：Matching Networks for One Shot Learning

代码：https://github.com/gitabcworld/MatchingNetworks

**关键词**：度量学习

#### 概述

support set 和 query 提取完特征之后，直接用cosine距离 衡量query跟support set哪个更接近



### EASY

论文：EASY – Ensemble Augmented-Shot Y-shaped Learning: State-Of-The-Art Few-Shot Classification with Simple Ingredients

代码：https://github.com/ybendou/easy





![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/notes_few_shot_learning_easy1.png)

- **Inductive**：只有support set可见
- **Transductive**：support set和query set都可见
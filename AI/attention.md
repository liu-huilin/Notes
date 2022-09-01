# 注意力机制

注意力机制最早应用于NLP，后来被应用于CV。

注意力机制的本质就是定位到感兴趣的信息，抑制无用信息，结果通常都是以概率图或者概率特征向量的形式表示。CV中的注意力主要有如下三种：

- 空间注意力模型

- 通道注意力模型
- 空间和通道混合注意力模型



**软注意力**（soft attention）：软注意力是一个[0,1]间的连续分布问题，更加关注区域或者通道，软注意力是确定性注意力，学习完成后可以通过网络生成，并且是可微的，可以通过神经网络计算出梯度并且可以前向传播和后向反馈来学习得到注意力的权重

**硬注意力**（hard attention）：0/1问题，哪些被attention，哪些不被attention。更加关注点，图像中的每个点都可能延伸出注意力，同时强注意力是一个随机预测的过程，更加强调动态变化，并且是不可微，所以训练过程往往通过增强学习。





公式：
$$
\operatorname{Attention}(Q, K, V)=\operatorname{softmax}\left(\frac{Q K^{T}}{\sqrt{d_{k}}}\right) V
$$
Q：查询（query）

K：键（key）

V：值（value）



### self-attention

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/note_attention_selfa1.png)

即，输入N个向量，输出N个向量，**N事先未知**。

具体的计算步骤如下：

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/note_attention_selfa2.png)

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/note_attention_selfa3.png)

即self-attention输入为a，输出为b，其中，$W^k$、$W^q$和$W^v$是需要训练的参数，写成矩阵形式，公式如下：

![](https://gitee.com/liu-huilin/markdownimg/raw/main/img2022/note_attention_selfa4.png)
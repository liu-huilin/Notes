# 元学习（Meta Learning）

## 基础概念

learn to learn，元学习就是让机器学会如何“学习”



## 经典算法

### MAML

论文：Model-agnostic meta-learning forfast adaptation of deep networks

代码：https://github.com/dragen1860/MAML-Pytorch

#### 概述

MAMLd的目标是学习一组优秀的初始化参数$\phi$，使这组参数在面对新任务时能够快速收敛。MAML在训练阶段，对每个任务只有两次梯度计算，第一次梯度计算是更新当前任务的参数$\theta$，第二次计算$\theta$的梯度是为了更新$\phi$。

#### 具体

$$
\Large
\begin{array}{l}
\phi \leftarrow \phi-\eta \nabla_{\phi} L(\phi) \\
L(\phi)=\sum_{n=1}^{N} l^{n}\left(\hat{\theta}^{n}\right) \\
\hat{\theta}=\phi-\varepsilon \nabla_{\phi} l(\phi)
\end{array}
$$